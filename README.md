# wp-docker-compose-wtih-http-domain

* Windows の Docker Desktop で 非SSL独自ドメインWP  
* AWS ALBでACMによるSSL終端想定
  * そのため、yml内のネットワークは、backend-app-network(wpとphpmyadmin)、backend-db-network(mariadb)　としている
* hostsファイルには`127.0.0.1 nissy-dev01.com`を`docker-compose up -d`実行前に1行追記
  * [https://hostsfileeditor.com/](https://hostsfileeditor.com/)が便利
* Windowsファイアウォール穴あけ不要
* docker-compose.ymlと同じ階層にhtmlフォルダを作っておいてから`docker-compose up -d`

# ネットワークの前提

以下のコマンドで、下記、1と2が既存ネットワークと重複していないこと（WSL2 の Ubuntu でコマンド実行）

```
docker network inspect $(docker network ls -q) --format '{{.Name}}: {{range .IPAM.Config}}{{.Subnet}}{{end}}'
```

1. 172.20.0.0/28
2. 172.20.0.16/28

# 構成内容
同梱 drawioファイルにてネットワーク参照可能  
  
* mariadb:latest
* phpmyadmin:latest
* wordpress:latest
* 非SSL独自ドメイン
* WP-CLI無し
* MailHog無し

# 置換

以下の命名を(Ctrl+F)で適宜置換してご利用ください
* nissy_dev01.com

# 起動後

* WordPress
  * [http://nissy_dev01.com](http://nissy_dev01.com)
  * [http://127.0.0.1:80](http://127.0.0.1:80)
* phpmyadmin
  * [http://127.0.0.1:8080](http://127.0.0.1:8080)
