---
layout: post
title: "[Golang] 安裝流程 CoreOS/DEX : OAuth Server"
description: ""
category: 
- Golang
tags: ["docker", "oauth", "kubernetes"]

---

## 挑選 DEX

挑選 [CoreOS 的 Dex](https://github.com/coreos/dex) ，因為它具有以下特點:

- 支援 OpenID
- 支援 Kubernetes Authenication (via OpenID)
- 支援 OAuth
- CoreOS 開發 (XD)

以下就是基本建置方式

### 建立 Google API Console 憑證

- 進入 [Google API Console](https://console.developers.google.com/project) 選擇已經有的專案（或是建立一個新的）
- 進入"[憑證中心](https://console.developers.google.com/apis/credentials)"，建立一個新的憑證．
	- 選取 "OAuth 用戶端 ID "
	- 選取 "網路應用程式"
	- 在 "已授權的重新導向 URI" 輸入 "http://127.0.0.1:5556/dex/auth/google/callback"
- 這邊會取得 "ID" 跟 "Secret" 記得存下來．

### 建立資料庫 (PostgresQL)

#### 1. 設定 postgres docker


```
docker run --name postgres -e POSTGRES_PASSWORD=YOURPASSWORD -d postgres
```

#### 2. 登入建立相關 schema

```
docker run -it --rm --link postgres:postgres postgres psql -h postgres -U postgres

>Password for user postgres: 
>psql (9.5.4)
>Type "help" for help.


postgres=# CREATE DATABASE dex_db;
> CREATE DATABASE

postgres=# CREATE USER dex WITH PASSWORD 'dex_pass';
> CREATE ROLE

postgres=# GRANT ALL PRIVILEGES ON DATABASE dex_db TO dex;
> GRANT

postgres=# \q //離開 postgre console
```

#### 3. 編譯 DEX

先設定 postgres 環境變數，將剛剛資料庫密碼帶入

```
export DEX_DB_URL=postgres://dex: YOURPASSWORD@localhost/dex_db?sslmode=disable
```

P.S.: 如果你跟我一樣 postgres 使用 docker ，記得將 `localhost` 改成該 container 的 IP.


下載並且編譯 DEX 

```
git clone https://github.com/coreos/dex.git
cd dex
./build
```

#### 4. 設定環境變數與啟動 DEX 服務

準備一段密碼（可能需要在其他電腦，如果你機器是乾淨的）：
(我是在 Mac 下跑這段:)


- 產生 Secret Symmetric Key

```
export DEX_KEY_SECRET=$(dd if=/dev/random bs=1 count=32 2>/dev/null | base64 | tr -d '\n')

//這是 Mac 產生的
> dd if=/dev/random bs=1 count=32 2>/dev/null | base64 | tr -d '\n'
hwMSvt8Fr39WN2tN1ydyPlD02szBhhL6REjGgCIhn3o=

```

- 產生 Admin API Secret

```
DEX_OVERLORD_ADMIN_API_SECRET=$(dd if=/dev/random bs=1 count=128 2>/dev/null | base64 | tr -d '\n')

//這是 Mac 產生的
> dd if=/dev/random bs=1 count=128 2>/dev/null | base64 | tr -d '\n'

B02cILOvy6o7DNU/zH7umCNkWr+E2MSkFsV3+nj5uKNaqVK7T33OLdN1ou38Rid6Swy/ZL4GljqeGOFhDgHJTkjA1so2HYr8Uda2FYHRuz2/AMSamwjLCOANl+3i9WOGduTDc8BtksN+fXB5xaJYpKDxWbcZoAaC1rU3VZyajDM=
```

設定環境變數，啟動伺服器．

```
export DEX_OVERLORD_ADMIN_API_SECRET=$DEX_OVERLORD_ADMIN_API_SECRET
export DEX_OVERLORD_DB_URL=$DEX_DB_URL
export DEX_OVERLORD_KEY_SECRETS=$DEX_KEY_SECRET
export DEX_OVERLORD_LOG_DEBUG=true
./bin/dex-overlord &
```

#### 5. 執行 worker

這裡要修改 `static/fixtures/emailer.json` 不過可以先依照原本範例使用 fake email 

```
./bin/dex-worker --db-url=$DEX_DB_URL --key-secrets=$DEX_KEY_SECRET --email-cfg=static/fixtures/emailer.json --enable-registration=true --log-debug=true &
```

#### 6. 執行 Connector

記得要先將 `$DEX_GOOGLE_CLIENT_ID` 與 `$DEX_GOOGLE_CLIENT_SECRET` 換成你在 Google API Console 拿來的資料．

```
cat << EOF > /tmp/dex_connectors.json
[
    {
        "type": "local",
        "id": "local"
    },
    {
        "type": "oidc",
        "id": "google",
        "issuerURL": "https://accounts.google.com",
        "clientID": "$DEX_GOOGLE_CLIENT_ID",
        "clientSecret": "$DEX_GOOGLE_CLIENT_SECRET",
        "trustedEmailProvider": true
    }
]
EOF
./bin/dexctl --db-url=$DEX_DB_URL set-connector-configs /tmp/dex_connectors.json
```

#### 7. 啟動 Client

```
eval "$(./bin/dexctl --db-url=$DEX_DB_URL new-client http://127.0.0.1:5555/callback)"
```

記得要把 `127.0.0.1` 換成你的 public IP ．


#### 8. 啟動 Web Server

```
./bin/example-app --client-id=$DEX_APP_CLIENT_ID --client-secret=$DEX_APP_CLIENT_SECRET --discovery=http://127.0.0.1:5556/dex &
```
記得要把 `127.0.0.1` 換成你的 public IP ．


### Authenitication for Kubernetes

- [Simple walkthrough](https://github.com/aledbf/contrib/blob/6d61ea81bb0bdbbc115cd6a6e9c59ef653afb213/ingress/controllers/nginx/examples/auth/README.md) 
- [Kubernetes Authentication plugins and kubeconfig](http://www.dasblinkenlichten.com/kubernetes-authentication-plugins-and-kubeconfig/)
- [Slide: Secret in Kubernetes](http://www.slideshare.net/jerryjalava/secrets-in-kubernetes)
- [Kubernetes の認証と dex](http://qiita.com/superbrothers/items/1822dbc5fc94e1ab5295)
- [Deploying dex on Kubernetes](https://github.com/coreos/dex/tree/master/contrib/k8s)


### Seems no OAuth support

- [Allow actors against the apiserver to be authenticated and actions to be authorized](https://github.com/kubernetes/kubernetes/issues/443)
- [kubernetes/issue: Improve documentation on how authentication is handled in Kubernetes](https://github.com/kubernetes/kubernetes/issues/11000)


## 參考鏈結 OAuth Server

- [Go: OpenID Connect Identity (OIDC) and OAuth 2.0 Provider with Pluggable Connectors](https://github.com/coreos/dex)
- [Go: Auth Boss](https://github.com/go-authboss/authboss)
- [Go: OAuth2](https://github.com/go-oauth2/oauth2)
- [Go: Docker registry oauth server](https://github.com/cesanta/docker_auth)
- [Ruby: OAuth server with UI management system](https://github.com/SUSE/Portus)

## 其他參考鏈結

- [Docker Registry v2 authentication using OAuth2](https://docs.docker.com/registry/spec/auth/oauth/)
- [Docker Registry v2 authentication via central service](https://docs.docker.com/v1.7/registry/spec/auth/token/)
- [Kubernetes: Authenticating](http://kubernetes.io/docs/admin/authentication/)
- [Kubernetes: Using Authorization Plugins](http://kubernetes.io/docs/admin/authorization/)
- [Kubernetes技术分析之安全](http://dockone.io/article/599)