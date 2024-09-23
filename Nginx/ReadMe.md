# はじめに

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
