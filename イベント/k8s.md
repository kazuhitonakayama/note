## 目標
k8sを構成する要素を体系的に理解する

- [[コンテナ技術]]を理解する
- 基礎的な[[Docker]]コマンドを理解する
- [[k8s]]を何のために使うのか理解する

# コンテナ技術入門
## コンテナ技術入門
![[Pasted image 20211124132244.png]]
様々なところで使われている
- サービスの各コンポーネント
- CI/CD
- などなど

VM時代
- リソースコストが高い
- 起動が遅いことが多い
- ハイパーバイザーが異なるとポータビリティは高くない
  - 例えばAWSからGSPへの移行はVMだと大変

![[Pasted image 20211124132736.png]]

![[Pasted image 20211124133011.png]]
- ホストOSを共有するので制約がある。
	
#### ABI
![[Pasted image 20211124133239.png]]
- システムコール呼び出し
- ユーザー命令
が揃ってないといけない。

Linux系はシステムコールが統一されている。

### VMは負けたのか
VMとコンテナは共存している
VMは隔離性に優れているし、ライブマイグレーション技術が枯れている

### VMのオーバーヘッドは小さい


![[Pasted image 20211124133531.png]]

### Linuxにおける実装
- cgroup
	- ![[Pasted image 20211124133807.png]]
	- 各プロセスへの制限は木構造で表現される
	- cpuとか
- Namespace
	- 名前空間を提供
	- リソースを分離する
	- メリット
		- セキュリティにも使われる
			- 内から外を認知できなくしているから
- capabilityes
	- 特権プロセス

種類
- Docker
- k8s
- ECS
- EKS
- Fargate
- GCP
	- GKE
	- GKE Autopilot
	- Google Cloud Run


## Docker入門
## k8s入門

- 宣言的
- 運用の自動化
- ポータブル

リソースは全て宣言的なYAMLで定義される。
Desired State という概念である。

リソースとは
- Workloads リソース
  - コンテナ実行に係る
- Discovery リソース
  - コンテナを外部公開するためのリソース

Namespace による分離

やや

```
# レプリカセットの一覧を見たい時
> kubectl get replicaset

# 削除したいとき
> kubectl get delete replicaset Pod名
```

