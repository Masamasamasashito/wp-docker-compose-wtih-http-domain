# wp-docker-compose-wtih-http-domain

* DockerDesktopで非SSL独自ドメインWP(flywheeelが起動している場合、ネットワーク競合に注意)。  
* AWS ALBでACMによるSSL終端想定  
* docker-compose.ymlと同じ階層にhtmlフォルダを作っておいてから`docker-compose up -d`
* hostsファイルには`127.0.0.1 nissy-dev01.com`を1行追記
* Windowsファイアウォール穴あけ不要

# 構成内容
同梱 drawioファイルにてネットワーク参照可能  
  
* mariadb:11.8.2
* phpmyadmin:5.2.2
* wordpress:latest
* 非SSL独自ドメイン(hostsファイルでFlywheel競合に注意)
* WP-CLI無し
* MailHog無し
