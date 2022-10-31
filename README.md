独自の設定を入れたmy.cnfを作成して、config/配下に保存する。

config/
   mymy.cnf ←　作成したy.cnf設定ファイル


**中身**
```
$vi config/mymy.cnf

[mysql]
default-character-set=utf8mb4

[mysqld]
collation-server = utf8mb4_unicode_ci
init-connect='SET NAMES utf8mb4'
character-set-server = utf8mb4
```

作成した設定ファイルを読み込んでmariadbをdockerで起動する。

```
$ docker run --name utf8-mariadb -p 3306:3306 -v /mnt/c/Users/james/development/docker_image/mariadb/config:/etc/mysql/conf.d -e MARIADB_ROOT_PASSWORD=password -d mariadb:latest
```

起動後にocker cliでデータベースに入って、接続用のユーザを作成する。

docker cliで入った後に以下を実行してip を確認する。
```
$hostname -i
172.17.0.3
```

mysqlコマンドでログインして、ユーザ追加を実行する。

```
$ mysql -u mysql -p
grant all on *.* to 'root'@'172.17.0.3' identified by '1111';
FLUSH PRIVILEGES;
```

ローカル環境から以下を実行して、接続できるか確認する。

```
$ mysql -u root -p -h 127.0.0.1 -P3306
```