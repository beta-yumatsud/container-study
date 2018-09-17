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
$ docker image inspect [--format=“{{ .hoge}}”] イメージ名
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

## Dockerfile
* ベースイメージの指定
```Dockerfile
FROM [ イメージ名 ] : [ タグ名 ]
```
* `RUN` : イメージのレイヤーを考えて、1行でかけるときは1行にするのが望ましい
```Dockerfile
■シェル形式(/bin/sh)
RUN apt-get install -y nginx

■Exec形式(json形式)
RUN [“/bin/bash”,“-c”,”apt-get install -y nginx”]
→引数で文字列を渡すときはシングルクォーテーションを使うこと
```
* `CMD(デーモンの実行)` : 生成したコンテナ内でコマンドを実行可能。Dockerfileに1つのCMD命令を記述可能。複数記載の場合、最後のコマンドのみ有効。
```Dockerfile
■Exec形式
CMD [“nginx”, “-g”, “daemon off;”]

■シェル形式
CMD nging -g ‘daemon off;’
```
* `ENTRYPOINT(デーモンの実行)` : runコマンドを実行した時に実行される。記述方法は、CMDと同様だが、CMDとの違いは下記。
```Dockerfile
■CMD
dockerのrunコマンド実行時に引数で新たなコマンドを指定した場合、そちらを優先する

■ENTRYPOINT
この命令で指定したコマンドは、必ずコンテナで実行されるが、実行時にコマンド引数を指定したいときは、CMD命令と組み合わせて利用
```
* `ONBUILD`
  * ビルド完了時に実行される命令
  * 次のビルドで実行されるコマンドをイメージ内に設定するための命令
  * ベースイメージからイメージを作るときなど(inspectコマンドで確認可能)
* `ENV` : 環境変数の設定
```Dockerfile
ENV [key] [value]
ENV [key]=[value](こちらは一度に複数設定可能)
→runコマンド実行時に- -envで変更可能
```
* `WORKDIR` : 作業ディレクトリの指定、Dockerfile内で複数回使用可能
* `LABEL` : maintainer、version、descriptionなどをつける時に利用
```Dockerfile
LABEL <キー名>=<値>
```
* `EXPOSE` : ポートの設定
* `ARG` : Dockerfile内で使用する変数設定( `--build-arg` とかでビルド時に変更可能)
* `VOLUME` : イメージにボリュームを割り当てる
* `ADD` : ファイルやディレクトリの追加、ホストのファイルがtarやgzipの時はディレクトリとして展開される。
```Dockerfile
ADD <ホストのファイルパス> <Dockerイメージのファイルパス>
```
* `COPY` : ファイルのコピー、構文はADDと同じで、ADDはリモート上のファイル配置に使えるが、ホスト上の時は基本的にCOPYでOKみたい。
* `.dockerignore` : ビルドで除外したいファイル名を記述できる
