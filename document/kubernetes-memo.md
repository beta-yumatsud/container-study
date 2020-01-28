## 概要
kubernetes(以下k8s)を勉強した時のメモを記載。

## kubectl
* バージョン
```shell
$ kubectl version
```
* 各コンポーネントのステータス
```shell
$ kubectl componentstatuses
```
* 各ノードの状態 (master, node)
```shell
$ kubectl get nodes
```
* デフォルトのNamespaceを変更
```shell
$ kubectl config set-context or use-context
```
* Pods、ReplicaSets、Services、Deploymentsなどは下記で取得可能
```shell
$ kubectl get 〜
```
* 取得系のオプションは下記
  * `-o wide` : 詳細な情報を追加で表示
  * `-o json` : 生のjson情報を表示
  * `-no-headers` : ヘッダー情報を消して表示
* 詳細情報
```shell
$ kubectl describe 〜
```
* 作成や更新(editで編集、deleteで削除)
```shell
$ kubectl apply 〜
```
* `label`、`annotation`を追加でき、更新は`--overwrite`を使う
* ログ
```shell
$ kubectl logs 〜
「-c」でコンテナ選択、「-f」で継続的にログを流せる
```
* `exec` はdockerと同じイメージ、シェルでログインしたりできる
