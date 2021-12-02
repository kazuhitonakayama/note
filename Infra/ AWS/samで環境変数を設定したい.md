# samで環境変数を設定したい

## 前提
AWS SAM を用いてAWSのリソースをコードで管理できるようにしています。（Infrastructure as Code）
今回はSAMでAWS lambdaを作成し、その上でGoで書かれた関数を実行できるようにしております。
また、今回利用するSAMはAWS SAM [テンプレート](https://docs.aws.amazon.com/ja_jp/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-init.html)を使用してサーバーレスアプリケーションを作って作成しました。
環境は``default``環境のみで動作させてます。


## 結論
ローカル起動時(sam local start-api / sam local invoke)とデプロイ時(sam deploy)とで、環境変数の読み込み方法には違いがあります.

### ローカル起動時に環境変数を読み込むには
#### 1. env.jsonを利用する方法
1. ルートディレクトリに``env.json``の配置
2. ``template.yaml``にて、Globalsセクション、あるいは、ResourcesセクションのEnvironment > Variables 配下に環境変数のKeyを設置
3. local起動時に、``env.json``を読み込んでもらうオプションを指定する

が必要になります。

1. ルートディレクトリに``env.json``の配置

```json
{
    "Parameters": {
      "DB_USERNAME": "ユーザー名",
      "DB_PASSWORD": "パスワード",
      "HOSTNAME": "ホスト名",
      "PORT": "ポート名",
      "DB_NAME": "DB名"
    }
}
```

2. ``template.yaml``にて、Globalsセクション、あるいは、ResourcesセクションのEnvironment > Variables 配下に環境変数のKeyを設置

```yaml
Globals:
  Function:
    Timeout: 5
    Environment:
      Variables:
        DB_USERNAME:
        DB_PASSWORD:
        HOSTNAME:
        PORT:
        DB_NAME:
```

3. local起動時に、``env.json``を読み込んでもらうオプションを指定する

```
> sam local start-api --env-vars env.json
```

これであとはコード内で環境変数を読み込めばOKです！

```go:main.go
func Connect() *sql.DB{
	db_source_name := fmt.Sprintf("%s:%s@tcp(%s:%s)/%s", os.Getenv("DB_USERNAME"), os.Getenv("DB_PASSWORD"), os.Getenv("HOSTNAME"), os.Getenv("PORT"), os.Getenv("DB_NAME"))
	log.Print(db_source_name)
	db, err := sql.Open("mysql", db_source_name)
```

または、次のような方法もあります。

#### 2. samconfig.tomlを利用する方法
``sam deploy --guided``を行うことで作成される``samconfig.toml``に``start-api``あるいは``local-invoke``をする際に送られるパラメーターを上書きする方法になります。

```toml
version = 0.1
[default]
# start-apiする場合
[default.local_start_api.parameters]
parameter_overrides = "DbUsername=DB名 DbPassword=パスワード Hostname=ホスト名 Port=ポート名 DbName=DB名"
# local invokeする場合
[default.local_invoke.parameters]
parameter_overrides = "DbUsername=DB名 DbPassword=パスワード Hostname=ホスト名 Port=ポート名 DbName=DB名"
[default.deploy]
[default.deploy.parameters]
```
**parameter_overrides**に、上書きしたいパラメーターを記述します。
注意として、この上書きしたいパラメーターを記述する際の**Key名はキャメルケースでないと正常に環境変数を読み込んでくれません**。

次に、``template.yaml``に、パラメータを渡します。

```yaml
# More info about Globals: https://github.com/awslabs/serverless-application-model/blob/master/docs/globals.rst
Globals:
  Function:
    Timeout: 5
    Environment:
      Variables:
        DB_USERNAME: !Ref DbUsername
        DB_PASSWORD: !Ref DbPassword
        HOSTNAME: !Ref Hostname
        PORT: !Ref Port
        DB_NAME: !Ref DbName
```

任意のFunction内でのみ環境変数を渡したいときは以下のようにResourcesの中に書いてあげてください。
Globalsに書くと全てのFunctionsに環境変数が適用されます。

```yaml
Resources:
  HelloWorldFunction:
    Type: AWS::Serverless::Function
    Properties:
      Runtime: go1.x
      Environment: # More info about Env Vars: https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#environment-object
        Variables:
          DB_USERNAME: !Ref DbUsername
          DB_PASSWORD: !Ref DbPassword
          HOSTNAME: !Ref Hostname
          PORT: !Ref Port
          DB_NAME: !Ref DbName
```

これであとはコード内で環境変数を読み込めばOKです！

```go:main.go
func Connect() *sql.DB{
	db_source_name := fmt.Sprintf("%s:%s@tcp(%s:%s)/%s", os.Getenv("DB_USERNAME"), os.Getenv("DB_PASSWORD"), os.Getenv("HOSTNAME"), os.Getenv("PORT"), os.Getenv("DB_NAME"))
	log.Print(db_source_name)
	db, err := sql.Open("mysql", db_source_name)
```


### デプロイ時に環境変数を読み込むには

1. ``samconfig.yaml``にパラメーターを上書きするための記述をする

```toml
version = 0.1
[default]
[default.deploy]
[default.deploy.parameters]
parameter_overrides = "DbUsername=DB名 DbPassword=パスワード Hostname=ホスト名 Port=ポート名 DbName=DB名"
```

次に、``template.yaml``に、パラメータを渡します。

```yaml
# More info about Globals: https://github.com/awslabs/serverless-application-model/blob/master/docs/globals.rst
Globals:
  Function:
    Timeout: 5
    Environment:
      Variables:
        DB_USERNAME: !Ref DbUsername
        DB_PASSWORD: !Ref DbPassword
        HOSTNAME: !Ref Hostname
        PORT: !Ref Port
        DB_NAME: !Ref DbName
```

次に渡すパラメーターの型をParametersセクションに追加します

```yaml:template.yaml
Parameters:
  DbUsername:
    Type: String
  DbPassword:
    Type: String
  Hostname:
    Type: String
  Port:
    Type: String
  DbName:
    Type: String

Globals:
  Function:
    Timeout: 5
    Environment:
      Variables:
        DB_USERNAME: !Ref DbUsername
        DB_PASSWORD: !Ref DbPassword
        HOSTNAME: !Ref Hostname
        PORT: !Ref Port
        DB_NAME: !Ref DbName
```

これであとはコード内で環境変数を読み込めばOKです！

```go:main.go
func Connect() *sql.DB{
	db_source_name := fmt.Sprintf("%s:%s@tcp(%s:%s)/%s", os.Getenv("DB_USERNAME"), os.Getenv("DB_PASSWORD"), os.Getenv("HOSTNAME"), os.Getenv("PORT"), os.Getenv("DB_NAME"))
	log.Print(db_source_name)
	db, err := sql.Open("mysql", db_source_name)
```

## 参照
##### パラメーターの上書きの仕方
[AWS SAM CLI の設定ファイル](https://docs.aws.amazon.com/ja_jp/serverless-application-model/latest/developerguide/serverless-sam-cli-config.html)

##### localで環境変数の読み込ませ方
[sam local start-api](https://docs.aws.amazon.com/ja_jp/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-local-start-api.html)

##### GLobalな環境変数の指定の仕方と、Resourcesごとの環境変数の指定の仕方
[AWS SAM テンプレートの Globals セクション](https://docs.aws.amazon.com/ja_jp/serverless-application-model/latest/developerguide/sam-specification-template-anatomy-globals.html)


## タグ
[[AWS]]
[[SAM]]