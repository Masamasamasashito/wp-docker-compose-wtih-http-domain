# wp-docker-compose-wtih-http-domain

* Windows の Docker Desktop で 非SSL独自ドメインWP  
* AWS ALBでACMによるSSL終端想定
  * そのため、yml内のネットワークは、backend-app-network(wpとphpmyadmin)、backend-db-network(mariadb)　としている
* hostsファイルには`127.0.0.1 nissy-dev01.com`を`docker-compose up -d`実行前に1行追記
  * [https://hostsfileeditor.com/](https://hostsfileeditor.com/)が便利
  * hostsファイル設定抜け漏れしないようにくれぐれも注意
* Windowsファイアウォール穴あけ不要
* docker-compose.ymlと同じ階層にhtmlフォルダを作っておいてから`docker-compose up -d`
* Windows の Docker Desktop で複数環境の並走可能

## 複数環境を同時並走する場合

2環境分のdev01,cev02のdocker-compose.yml

- dev01
  - https://github.com/Masamasamasashito/wp-docker-compose-wtih-http-domain/blob/main/dev01/docker-compose.yml
- dev02
  - https://github.com/Masamasamasashito/wp-docker-compose-wtih-http-domain/blob/main/dev02/docker-compose.yml

### dev01,dev02の差分内容

- 各種 ports ローカルホストIPアドレス
- 各種 networks ipv4_address
- WordPressドメイン名（ローカルで扱うドメイン名）
- DBユーザー名、DBパスワード、DB名
- 各種命名

これらの点を考慮することでローカルホストのリソースに余力が有れば、他のコンテナセットを並走可能。

### hostsファイル

場所:`C:\Windows\System32\drivers\etc\hosts`

以下のように設定。記載する前に重複がないか？要確認。

```
127.0.0.1 nissy-dev01.com
127.0.0.2 nissy-dev02.com
```

# ネットワークの前提

defaultのBridge利用が他用途との干渉原因になりやすいためBridgeをymlの中で指定しています。  
以下のコマンドで、下記、1と2が既存ネットワークと重複していないこと（WSL2 の Ubuntu でコマンド実行）  

```
docker network inspect $(docker network ls -q) --format '{{.Name}}: {{range .IPAM.Config}}{{.Subnet}}{{end}}'
```

1. 172.20.0.0/28
    - backend-app-network(wpとphpmyadmin)
2. 172.20.0.16/28
    - backend-db-network(mariadb)

# 構成内容
下部の図解にて。drawioファイルも有。

* wordpress:latest
    - https://hub.docker.com/_/wordpress
    - apache
* phpmyadmin:latest
    - https://hub.docker.com/_/phpmyadmin
* mariadb:latest
    - https://hub.docker.com/_/mariadb
* 非SSL独自ドメイン
* WP-CLI無し
* MailHog無し

# 置換

以下の命名を(Ctrl+F)で適宜置換してご利用ください
* nissy-dev01.com
* nissy-dev02.com

ローカルホスト 127.0.0.1 が既存の他環境と衝突していないか？要確認。

# 起動後

* WordPress
  * [http://nissy-dev01.com](http://nissy-dev01.com)
  * [http://127.0.0.1:80](http://127.0.0.1:80)
* phpmyadmin
  * [http://127.0.0.1:8080](http://127.0.0.1:8080)

# 図解

[dev01/docker-compose.yml](https://github.com/Masamasamasashito/wp-docker-compose-wtih-http-domain/blob/main/dev01/docker-compose.yml)の場合の母艦Windows OSのブラウザからWordPressまでのhttpリクエストのみ経路を青で記載

![https://github.com/Masamasamasashito/wp-docker-compose-wtih-http-domain/blob/mainnissy-dev01-windows-local-docker-20251106-15.jpg](https://github.com/Masamasamasashito/wp-docker-compose-wtih-http-domain/blob/main/nissy-dev01-windows-local-docker-20251106-15.jpg)

# ⚠️注意事項⚠️

portsで必ずプライベートIPアドレスを明記していることもあり、config関連の設定値の別ファイル化 と 別ファイル用の.gitignore 設定を行っていない。
