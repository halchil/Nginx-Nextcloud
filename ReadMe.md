# はじめに

# 参考文献
[Next Cloud Document](https://docs.nextcloud.com/)

# ネットワーク作成

```
[実行コマンド]
docker network create --subnet=172.10.10.0/24 --gateway=172.10.10.1 ecosystem

[結果]
d7178ec128c06b2ca63be8bcf778d58c2514f84623fbe2ed03162a77acfedd3a

[確認コマンド]
docker network inspect ecosystem

[結果]
[
    {
        "Name": "ecosystem",
        "Id": "d7178ec128c06b2ca63be8bcf778d58c2514f84623fbe2ed03162a77acfedd3a",
        "Created": "2024-09-22T10:36:58.129701695Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.10.10.0/24",
                    "Gateway": "172.10.10.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

# docker compose 実行

```
[実行コマンド]
docker compose up -d

[結果]
WARN[0000] /home/mainte/Nginx-Nextcloud/Nginx/docker-compose.yaml: `version` is obsolete 
[+] Running 1/1
 ✔ Container nginx  Started
 ```

Nginxへのアクセスは確認できた


# NextCloud イメージ取得

```
[実行コマンド]
docker pull nextcloud

[結果]
Using default tag: latest
latest: Pulling from library/nextcloud
a2318d6c47ec: Already exists 
c335a1cecf20: Pull complete 
9f1356d24f26: Downloading [=====>                                             ]  11.31MB/104.3MB
93a67f8d1dfc: Download complete 
773d8e2be6c6: Downloading [==================>                                ]  7.633MB/20.33MB
b874a8bbb522: Download complete 
...
```

イメージを確認する。

```
[実行コマンド]
docker image list

[結果]
REPOSITORY            TAG       IMAGE ID       CREATED         SIZE
nextcloud             latest    2706aa40a539   4 days ago      1.24GB
...
```

# Nextcloud単体でデプロイ

[Nextcloud](https://github.com/halchil/Nginx-Nextcloud/blob/main/NextCloud/docker-compose.yaml)

```
[実行コマンド]
docker compose up -d

[結果]
WARN[0000] /home/mainte/Nginx-Nextcloud/NextCloud/docker-compose.yaml: `version` is obsolete 
[+] Running 1/1
 ✔ Container nextcloud  Started
 ```

以下のアドレスで、アクセスしログイン画面の表示に成功。
```
 http://192.168.56.102:8080
 ```

![fig](/Img/img1.png)

ここでは、データが保持されないので、一度コンテナをリムーブすると、データが消えてしまう。

# データ保持の仕組みを作る

データ保持に関しては、以下のボリュームセクション
```
volumes:
      - ./nextcloud-data:/var/www/html
```

```
ll nextcloud-data/
total 1328
...
drwxr-xr-x 53 www-data www-data    4096 Sep 23 07:13 apps/
...
drwxr-xr-x  3 www-data www-data    4096 Sep 23 07:13 data/
drwxr-xr-x  2 www-data www-data   20480 Sep 23 07:13 dist/
...

```

この中の、`data`ディレクトリにアップロードファイルが保管される。

試しにブラウザから`Haru`という名前のフォルダを作成する。
そしてその中に`nginx.conf`を作成する。

```
[確認コマンド]
sudo ls -l nextcloud-data/data/test/files

[結果]
...
drwxr-xr-x 2 www-data www-data     4096 Sep 23 07:17  Haru
...

[確認コマンド]
sudo ls -l nextcloud-data/data/test/files/Haru

[結果]
-rw-r--r-- 1 www-data www-data 913 Sep 23 05:46 nginx.conf
```
となり、ローカル側でもファイルが連携されていることが分かった。


# Nginxと接続する

