# wp-docker-compose-wtih-http-domain
DockerDesktopで非SSL独自ドメインWPとしてdocker-compose up -d出来る(flywheeel競合に注意)。AWS ALBでACMによるSSL終端想定  
docker-compose.ymlと同じ階層にhtmlフォルダを作っておいてから

`docker-compose up -d`

# メール用ソフト MailHog 無し
同梱 drawioファイルにてネットワーク参照可能 Windowsファイアウォール穴あけ不要

* mariadb:11.8.2
* phpmyadmin:5.2.2
* wordpress:latest
* 非SSL独自ドメイン(hostsファイルでFlywheel競合に注意)
* WP-CLI無し
* MailHog無し
