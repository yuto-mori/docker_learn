# dockercompose を使おう
dockercompose　は複数のコンテナで構成されるアプリケーションのDockerイメージのビルドやコンテナの起動を簡単に行えるようにするツール。

## 記述の意味
services の一段下の「db」と「web」はサービス名。

image: dockerHub の名前の指定。build でDockerfile を使用している場合は image名になる。「-t」と同じ。

container_name:コンテナの名前。コマンドで打った時の「--name」と同じ。

expose:コンテナで公開するポート番号

environment:環境変数を指定する。Dockerfile のENVと同じ。

build:Dockerfile が格納されたフォルダ名の指定。buildを指定したときは imageの値はimage名になる。

ports:コンテナのポートをホストのポートに割り当てる。「ホスト側のポート:コンテナのポート」。コマンド「-p」と同じ。

working_dir:コンテナ起動時のコマンドを実行するディレクトリの指定。
Dockerfile に書いていた 「WORKDIR」は必要なくなる。実行するコマンドは、Dockerfileに書く。

volumes:ボリュームのマウントの指定。DockerfileのCOPYのような書き方と、コマンドの「--mount」のような書き方2つある。「-」でわけている。

depends_on:サービスの依存関係を指定。


複数コンテナを扱うためにDBの設定をしているが、特にDBは使用していない。


## dockercomposeのコマンド

・一部抜粋
docker-compose up サービスの起動/再起動
docker-compose exec コマンドの実行
docker-compose stop　サービスの停止
docker-compose start サービスの開始
docker-compose down サービスの停止とコンテナの削除

docker-compose up -d 
「-d」とつけてバックグランドでサービスを起動するのが一般的。

docker-compose up -d　--build 
Dockerfile を変更した場合、イメージからのビルドを行う。

docker-compose exec web /bin/bash
シェルを起動。webはディレクトリの場所。

docker-compose down -rmi all
「-rmi all」オプションでイメージを削除。「-volumes」オプションでボリュームも削除。