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

