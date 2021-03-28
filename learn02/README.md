# dockerfile を使おう
Docker Image は Dockerfile というファイルを記述し、そのファイルを元にビルドすることでスナップショットの作成ができます。
Dockerfile を記述することによって Docker Image を作成できる。

## dockerfile に記述する
### 基本の7つのコマンド
FROM:ベースとなるDocker Image を指定。
ベースとなるDocker Image は公式で提供されているImageを使用するのが一般的。
https://hub.docker.com/

ENV:Docker内で使用する環境変数を定義。

WORKDIR:Dockerfileでコマンドを実行する際に基準となるディレクトリを設定する。
ここでは /var/www/html

COPY:Docker内へホストのファイル/ディレクトリをコピーする。
COPY は基本的に2つの引数を設定する。1つ目はホスト側のディレクトリ、2つ目はDocker側のディレクトリ。
ホスト側のディレクトリは docker build . で指定したディレクトリ。この場合 . を指定しており、カレントディレクトリが参照される。
Docker側は WORKDIR で定義されたディレクトリを参照する。

USER:作成したDocker Image を起動時にログインするユーザーを指定。

RUN:Docker内でコマンドを実行。
node をインストールするとか。

CMD:Docker内でコマンドを実行。
NPMのコマンドとか。

## docker imageを作る
$ docker image build --build-arg wdir=/var/www/html -t learn02/php:1.0 .

-t は必ず必要。「-t 名前:タグ」でイメージの名前とタグを指定（--name を使って使用するのは、docker container の時）。
最後の「.」は dockerfile の場所。dockerfile の名前が dockerfile であれば「.」。
そうでない場合は「-f」オプションを使う。

## docker containerを起動
dockerfile に書いたことは、docker imageを作るところまで。
docker container の使用はlearn01と同じ
docker container run --name learn02 -d --rm -p 8080:80 --mount type=bind,src=$(pwd)/src,dst=/var/www/html learn02/php:1.0

pwd はカレントディレクトリを示している

COPY を記述する（COPY src/index.php /var/www/html/）と以下のコマンドでもブラウザで確認できる。
docker run --name learn02 -d --rm -p 8080:80 learn02/php:1.0
