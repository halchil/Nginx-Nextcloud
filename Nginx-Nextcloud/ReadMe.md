# はじめに
NginxとNextcloudを別々で立ち上げることに成功した。
nginx.confにlocationモジュールを追加し、Nextcloud立ち上げ時のポートを閉じることで可能となる。

# nginx.conf編集
`nginx.conf`

# Nextcloud立ち上げ時のポートを閉じる

#
今回は、別々に立ち上げたが、depends on機能を使うことで、1つのdocker-composeにデプロイ順を指定することが可能となる。(俺的には、depends onよりも.shで分けて書いた方がいいかも)

# トラブルシュート

## Nginxには接続できるが、Nextcloudに接続できない

確認方法

### nginxコンテナから、Nextcloudのサーバが認識されているか確認

```
[確認コマンド]
docker exec -it nginx curl http://nextcloud:80

[結果]
<!DOCTYPE html>
<html class="ng-csp" data-placeholder-focus="false" lang="en" data-locale="en" translate="no" >
        <head
 data-requesttoken="B42muiH+kKAoxArh7kY2lj8Mplj8ikuuI70+v/Nv9is=:TvWf1HiTp/RniUewgQNH+w949Wm433vlZdVQ2KoN3Vk=">
                <meta charset="utf-8">
                <title>
                        Nextcloud               </title>
                <meta name="csp-nonce" nonce="TvWf1HiTp/RniUewgQNH+w949Wm433vlZdVQ2KoN3Vk=">
                <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0">
                                <meta name="apple-itunes-app" content="app-id=1125420102">

```
サーバは認識されていることが分かった。


locationにphp追加

```
index index.php;
try_files $uri $uri/ /nextcloud/index.php?$args;
```

### Nextcloudが接続を受け使ていか確認

```
docker exec -it nextcloud curl http://localhost
<!DOCTYPE html>
<html class="ng-csp" data-placeholder-focus="false" lang="en" data-locale="en" translate="no" >
        <head
 data-requesttoken="dcM43KI1l8y+l9YwZI4K3hPTEYabs1LFIdgnleUR3h0=:BowJpJIFzqP/p7pzNcNb9Sa4XsvI9B2wR4AUpINfkW0=">
                <meta charset="utf-8">
                <title>
                        Nextcloud               </title>
```

```
proxy_pass http://nextcloud:80/nextcloud;
```
に変更してみる。

```
docker compose restart nginx
```

