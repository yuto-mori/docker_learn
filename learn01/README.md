# dockerをコマンドで動かそう

## imageの取得と起動

### 以下コマンドをターミナルに記述してimageを取得
$ docker image コマンドを使っていく
$ docker image pull php:7.4-apache

dockerimage の削除は以下
$ docker image rm {image id}

## コンテナを対話モードで起動する
コンテナのコマンドを使っていく
$ docker container ~

コンテナ内でbashを動かす
下記で動いているか確認
$ docker container run -it --rm php:7.4-apache /bin/bash
root@*****:/# でbash シェルが起動（exit で抜けられる）
・オプション
-it:対話モードで起動
--rm:終了（stop）時にコンテナを削除

すでにdockerが起動している時は以下コマンでbashを動かせるようになる（バックグランドモードでも可）。
$ docker container exec -it learn01 /bin/bash

他のターミナルを立ち上げて
$ docker container ls
で確認。

### コンテナをバックグラウンドモードで起動する

$ docker container run --name learn01 -d -p 8080:80 php:7.4-apache
・オプション
--name 人がわかるようにコンテナの名前を決める
-d バックグランドで実行
-p ホスト側のポートをコンテナ側のポートに割り当てる

http://localhost:8080/
を見るとアクセスできる（コンテナ側にファイルがないので、「Forbidden」と出る）。

以下でコンテナを停止
$ docker container stop learn01

以下でコンテナの再起動
$ docker container restart learn01

以下でコンテナの削除
$ docker container stop learn01
$ docker container rm learn01

#### phpを確認してみよう
$ docker container run --name learn01 -d -p 8080:80 php:7.4-apache
$ docker container exec -it learn01 /bin/bash
echo "Hello" > index.php
http://localhost:8080/

でphoが動いていることが確認できる。


## フォルダのマウント
コンテナ削除とともに消えてしまうコンテナの中で作成したフォルダやファイルが、永続的に残るようにする。

### volumeを使ったフォルダのマウント
$ docker container run -it --rm --mount src=test,dst=/tmp/test php:7.4-apache /bin/bash

他のターミナルを立ち上げて
$ docker volume ls
で確認

コンテナないのbashを使用してファイルを作成
 echo "Hello" > /tmp/test/test.php


ファイルの存在を確認
　ls /tmp/test -l

ボリュームを削除
$ docker volume rm test

## ローカルフォルダのファイル表示してみよう
touch test.php
$ docker container run --name learn01 -d -p 8080:80 php:7.4-apache
$ docker cp test.php learn01:/var/www/html
http://localhost:8080/test.php