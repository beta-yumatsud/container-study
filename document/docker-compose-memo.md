## 概要
docker-composeを学んだ時のメモ。

## 設定ファイル(`docker-compose.yaml`)
* 基本 : `services` 、`networks` 、 `volumes` の3つを定義する。先頭にはversionを指定するのが一般的。
* イメージの指定(`image`): タグを指定してバージョンを固定する
* イメージのビルド(`build`): イメージをpullしてくるのではなく、ローカルのdockerfileからビルドする際は `build: .` のように指定する
  * contextでディレクトリやパスの指定、dockerfileでファイル指定も可能
  * ビルドする際に、argsでパラメータも指定可能
* コンテナ内で動かすコマンドの指定(`command/entrypoint`)
* コンテナ間の連携(`links`)
  * エイリアスもつけること可
  * 依存関係はdepnds_onを使い、起動する順番も指定可能(ただし、コンテナのアプリケーション起動を全て待つわけではないことに注意)
* コンテナ間の通信(`ports/expose`)
  * コンテナが公開するポートはportsで指定
  * `ホストマシンのポート番号:コンテナの番号` or コンテナの番号のみ指定
  * ホストマシンへポートは公開せず、リンク機能を使って連携する時は、exposeを使う(ログサーバなど、ホストマシンからアクセスする必要がない場合などに利用)
* コンテナの環境変数の指定(`environment/enf_file`): yamlの配列orハッシュ形式のどちらかで指定。
* コンテナのデータ管理(`volumes/volumes_from`)
  * コンテナにボリュームをマウントする時はvolumesを使う
  * `ホストのディレクトリパス:コンテナのディレクトリパス` なども可能

## docker-composeのコマンド
* コンテナの生成/起動: `docker-compose up`
* コンテナの一覧: `docker-compose ps`
* コンテナのログ出力: `docker-compose logs`
* リソースの削除: `docker-compose down`
* コマンド実行: `docker-compose run`
* 公開用のポート: `docker-compose port`
* 他にも、start/stop/restart/pause/rmなどがある
* 上記はいずれも、`-f`でyamlファイルの指定も可能だし、`-d`でbackground起動、`—build`でコンテナ起動時にビルドの指定ができる
