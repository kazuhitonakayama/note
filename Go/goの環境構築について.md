## Goの環境変数を参照するには

```
go env
```

### GOROOT
Goのインストールパス
→自分でインストールディレクトリを変更した場合は、環境変数GOROOTの設定が必要である。

### GOPATH
外部パッケージのリソースなどが保存される.

### go get
これの使用により外部パッケージをインストールできる
→Rubyでいうところのgemみたいな感じ

### gore
GoのREPL（Read-eval-print loop）である。
→Rubyのirb的な