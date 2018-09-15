## 概要
Dockerの学習をした際に学んだことを記載しておく。

## dockerコマンド
### イメージの操作
* イメージのダウンロード
```shell
$ docker image pull イメージ名[:タグ名]
```
* イメージの一覧表示
```shell
$ docker image ls [オプション: -all, -a 全て、-q イメージIDのみ]
```
* イメージの詳細情報表示
```shell
docker image inspect [--format=“{{ .hoge}}”] イメージ名
```
* イメージのタグ設定
```shell
$ docker image tag イメージ名 <ユーザ名>/<イメージ名>:[タグ名]
```
* イメージの検索
```
$ docker search [オプション] 検索キーワード
```
* イメージの削除
```shell
$ docker image rm [オプション] イメージ名…
```
* 未使用imageの削除
```shell
$ docker image prune [オプション: -all, -a 使用指定なイメージを全て削除、-f 強制的に削除]
```
* Docker Hubへのログイン
```shell
$ docker login [オプション] [サーバ]
```
* イメージののupload
```shell
$ docker image push <ユーザ名>/イメージ名[: タグ名]
```
