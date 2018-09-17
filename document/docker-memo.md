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

### コンテナの基本操作
* コンテナ生成 → あんまり使うイメージがない (Dockerfileを使うと余計＞＜)
```shell
$ docker container create
```
* コンテナ生成/起動
```shell
$ docker container run [オプション: 下記] イメージ名[: タグ名] [引数]
```
* 生成/起動オプション
  * `-d` : バックグラウンド実行
  * `-u` : ユーザ名を指定
  * `--rm` : コマンド実行完了後のコンテナを自動で削除
  * `-i` : 標準出力で `-t tty` と一緒に使うケースあり
  * `--name` : 実行コンテナ名
  * `-restart=[no | on-failure | on-failure:回数n | always | unless-stopped]` : コマンドの実行結果によってコンテナを再起動
  * `-p[ホストのポート番号]:[コンテナのポート番号]` : ネットワーク設定
  * `--add-host` : `/etc/hosts` に設定可能
  * `-c` : CPUリソースの設定
  * `-m` : メモリリソースの設定
  * `-v` : ホストとコンテナのディレクトリ共有
  * `-e` : 環境変数の設定
  * `--env-file=ファイル名` : 環境変数の設定(ファイル指定)
* コンテナのログ
```shell
$ docker container logs [-t タイムスタンプ表示]
```
* コンテナの一覧
```shell
$ docker container ls [オプション: -a 停止中も含めて全て、-f key-value方式でフィルタリング]
```
* コンテナの状態
```shell
$ docker container stats [コンテナ識別子]
```
* コンテナ起動
```shell
$ docker container start [コンテナ識別子]
restartで再起動、pauseで中断
```
* コンテナ停止
```shell
$ docker container stop [コンテナ識別子]
killで強制停止、unpauseで再開
```
* コンテナ削除
```shell
$ docker container rm [オプション: -f 強制、-v ボリュームも合わせて] コンテナ識別子
```

### その他の操作
* ネットワーク
```shell
$ docker network hogehogeでネットワークの設定が色々とできるくさい
```
* 稼動コンテナへの接続
```shell
$ docker container attach
$ docker container exec -it コンテナ識別子 /bin/bash
```
* 稼動コンテナのファイルコピー
```shell
$ docker container cp ＜＜コンテナ識別子:コンテナ内のファイルパス ホストのパス＞＞
→これの逆も可能
```
* コンテナからイメージを作成
```shell
$ docker container commit [オプション] コンテナ識別子 [イメージ名[:タグ名]]
```
* コンテナ⇆tarファイル
```shell
$ docker image import/export
```
* 不要なイメージ/コンテナを一括削除
```shell
$ docker system prune [オプション: -a 全て、-f 強制]
```
* ビルド
```shell
$ docker built -t [生成するイメージ名]:[タグ名] [Dockerfileの場所]
```
* ビルドオプション
　　* `-f` : Dockerfileを指定
 * `--no-cache` : 中間イメージなどでキャッシュを使いたくないときに指定
