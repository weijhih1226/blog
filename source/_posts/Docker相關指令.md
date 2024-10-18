---
title: Docker常用相關指令
categories: [Docker, Docker Compose]
tags: [Docker, Docker Compose]
date: 2023/03/31 17:31
updated: 2024/10/18 11:30
---

## Docker

### Docker 常用指令

```bash
docker inspect <容器ID|名稱>          # 檢查某Container的狀態（進入點、執行狀態及其他詳細資料）
docker stats <容器ID|名稱>            # 查看各Container的CPU、記憶體及網路使用
docker exec <容器ID|名稱> [COMMAND]   # 在外部向Container內執行指令
docker logs <容器ID|名稱>             # 將Container內的輸出顯示到螢幕上
docker service ls                     # 列出Service狀態
docker service logs <服務ID|名稱>     # 將Service內的輸出顯示到螢幕上
```

- 容器相關
  
  ```bash
  docker ps                                       # 查看狀態
  docker ps -a                                    # 查看所有狀態
  docker run -it --name <name> <image> <command>  # 新建、啟動並執行指令
  docker start <container>                        # 啟動
  docker stop <container>                         # 停止
  docker restart <container>                      # 重啟
  docker rm <container>                           # 移除
  ```

- 映像檔相關

  ```bash
  docker build -t <image>[:tag] <path>            # 從dockerfile建立
  docker images                                   # 列出
  docker image ls                                 # 列出
  docker image rm                                 # 移除
  ```

因Docker可能裝在root權限底下，如沒有root使用Docker的權限，請在各指令前加sudo！

### 查看容器使用狀態

```bash
$ docker ps
$ docker ps -a  # 查看所有容器使用狀態
CONTAINER ID    IMAGE                   COMMAND CREATED     STATUS      PORTS                               NAMES
c7ee35c5de98    127.0.0.1:5000/*:3.3.0  "/…"    7 days ago  Up 7 days   0.0.0.0:8002->8000/tcp, :::8002->8000/tcp  qpesums_web
```

Container的使用狀態會列出Container ID、所使用的IMAGE（映像檔）位置、啟動容器之後會執行的COMMAND（指令）、哪時候被CREATED、目前的STATUS、對應到的PORTS（連接埠），以及Container的NAMES（名稱）。

若後面不加任何options僅會列出正在運行的Container。

### 查看各Container的CPU、記憶體及網路使用

```bash
docker stats <容器ID|名稱>
docker stats --no-stream          # 不滾動顯示
```

### 在外部向容器內執行指令

```bash
$ docker exec
Usage: docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
$ docker exec -it <Container ID|NAME> bash
```

表示進入Container後第一個執行的是bash介面。

### 列出Docker服務並查詢服務狀態

#### 列出Docker服務

```bash
$ docker service ls
ID              NAME        MODE    REPLICAS    IMAGE                   PORTS
zo97osyy73we    qpeplus     global  3/3         127.0.0.1:5000/*:3.3.0  *:8000->8000/tcp
```

將會列出各個服務的ID、NAME（名稱）、MODE（模式）、REPLICAS（複本）、所使用到的IMAGE（映像檔），以及所對應到的PORTS（連接埠）。

#### 查詢服務狀態

```bash
docker service logs <ID|NAME>
```

### 查看Swarm Nodes

```bash
docker node ls
```

再利用查到的ID或名稱去查詢該服務狀態。

### 移除多餘容器(container)、網路(network)、映像(image)、數據卷(volume)

```console
docker container prune        # 移除未用容器
docker network prune          # 移除未用網路
docker image prune            # 移除未用映像
docker volume prune           # 移除未用數據卷
docker system prune           # 移除所有(Docker 17.06.0前包含數據卷)
docker system prune --volumes # 移除所有包含數據卷
```

[OPTIONS]有`-a`及`-f`。

### 其他指令

```console
docker pull <映像檔名稱>:<版本號>             # 拉取映像檔（未啟動容器）
docker run <映像檔名稱>:<版本號>              # 從映像檔啟動容器（若本機無該映像檔，則從遠端拉取）
docker rename <舊容器名稱|ID> <新容器名稱>    # 容器更名
docker version                                # 顯示詳細版本
```

#### DOCKER RUN

功能：從映像檔啟動容器。

- `-d` - 在背景運行容器，並列出容器ID
- `-i` - 保持stdin
- `-t` - 分配pseudo-TTY
- `-p <host_port>:<container_port>` - 本機的通訊埠對應到容器的通訊埠
- `-v, --volume <host_dir>:<container_dir>` - 掛載本機目錄至容器(自動建立)
- `--mount <type=bind,source=host_dir,target=container_dir>` - 掛載本機目錄至容器
- `-w, --workdir <container_dir>` - 指定工作目錄
- `--name <name>` - 指定容器名稱

```console
docker run <image>[:<tag>]                            # 從映像檔啟動容器
docker run --name <name> <image>                      # 指定容器名稱
docker run -dp <host_port>:<container_port> <image>   # 指定通訊埠並在背景運行
docker run -it --name <name> -v <host_dir>:<container_dir> <image> <command> # 從映像檔啟動、掛載目錄並進入容器維持輸入
```

### DOCKERFILE

若現有的image儲存庫無法滿足功能需求，可以透過dockerfile，在現有的image儲存庫上，構建自己的image。

- Dockerfile關鍵字使用說明

  ```dockerfile
  FROM <image>[:<tag>]                          # 映像檔:版號
  ADD [--chown=<user>:<group>] <src>... <dest>  # 從<src>複製至image的<dest>處(支援從遠端下載，tar.gz檔自動解壓縮)
  COPY [--chown=<user>:<group>] <src>... <dest> # 從<src>複製至image的<dest>處(僅支援本地複製)
  WORKDIR <path>                                # 設定工作目錄
  VOLUME <path>...                              # 設定要開放對接的目錄
  RUN <command>                                 # 在當前image上再執行指令並提交結果
  RUN ["executable", "param1", "param2"]        # 在當前image上再執行指令並提交結果
  CMD <command>                                 # 執行中容器提供初始指令
  CMD ["executable", "param1", "param2"]        # 2個以上CMD指令只會運行最後1個
  EXPOSE <port>...                              # 開通容器的port
  USER <user>[:<group>]                         # 設定使用者及群組
  USER <uid>[:<gid>]                            # 設定使用者及群組
  ```

- Dockerfile其他說明

  ```dockerfile
  FROM <base_image> AS <stage_name>             # 命名提高可讀性
  COPY --from=<stage_name>                      # 可複製image
  ```

寫完dockerfile之後，再利用`docker build`建立自己的映像檔。

```console
docker build -t <映像檔名稱>[:<tag>] . --no-cache # 沒有tag時預設為latest，確定Dockerfile有放在同目錄下，--no-cache是避免在build的時候被cache住
docker images                                     # 查看是否有build成功
```

```dockerfile
FROM centos:8                   # 使用到image名稱及版本號
LABEL maintainer=satoshi        # 維護者資訊
# MAINTAINER satoshi            # 已棄用寫法(維護者資訊)

ARG PROXY                       # 只在build時存在的變數(可從內部或外部設值，例如：docker build --build-arg PROXY=http://proxy.xxx.com.tw/:1234 .)
ENV http_proxy=${PROXY}         # 設定環境變數(在build及run時存在的變數)
ENV https_proxy=${PROXY}

ADD run.py test.txt /           # 將local檔案複製到image裡(tar.gz檔會自動解壓縮)

RUN yum install -y wget         # 安裝這個image需要的套件
RUN cd /                        # 切換到根目錄

EXPOSE 5000                     # 開通容器的port

CMD ["/run.py" , "test.txt"]    # docker run時做的指令
```

## Docker Compose

### docker-compose.yaml 格式

該檔案應放置在專案目錄內，可命名為docker-compose.yml、docker-compose.yaml、compose.yml或compose.yaml。

以建立一個api-service為範例：

```yaml
version: "3.8"
services:
  api-service:                  # service名稱
    build: https://github.com/user/api.git  # 建立image的儲存庫或目錄(需有Dockerfile)
    image: api-image            # 建立image的名稱
    restart: always             # 重啟策略
    ports:                      # port對應
      - 5000:5000               # 容器外:內
    volumes:                    # volume路徑對應
      - ${LOG_PATH}:/logs       # 容器外:內
    environment:                # 容器內環境變數設定
      DEBUG: ${DEBUG_MODE}
    container_name: api-service # container名稱
```

當中LOG_PATH及DEBUG_MODE的環境變數，[預設會讀取專案目錄的 `.env` 檔案裡的環境變數][1]：

```conf
LOG_PATH=/home/user/api/logs/
DEBUG_MODE=false
```

搭配的Dockerfile格式說明：

```dockerfile
FROM python:3.12

WORKDIR /api                    # 工作目錄
ADD . /api                      # 複製目前目錄下檔案至/api
VOLUME /logs                    # 開放對接的目錄

RUN pip install --no-cache-dir -r requirements.txt  # 安裝程式所需python套件

EXPOSE 5000                     # 開通port

CMD python api_server.py        # 啟動server
```

### Docker Compose 常用指令

必須在有compose yaml file的目錄下才能使用以下指令：

```bash
docker compose up -d            # 運行container在背景
docker compose up -d --env-file <PATH>  # 指定環境變數檔案跑script並運行container
docker compose ps               # 列出active的container
docker compose down --rmi all   # 停止目前的container並移除相關image
docker compose config           # 列出docker compose的組態(yaml)
```

對應Docker指令：

- `docker compose down --rmi all`

  ```bash
  docker stop <container>
  docker rmi <image>
  ```

- 以上述範例說明 `docker compose up -d`

  ```bash
  docker build -t api-image https://github.com/user/api.git
  docker run --rm -itd \
  --name api-service \
  --restart always \
  -v $LOG_PATH:/logs \
  -p 5000:5000 api-image
  ```

  如有多個volume可以增加tag `-v` ，同理有多個port也可以增加tag `-p`。

---

## 參考資料

1. [Set, use, and manage variables in a Compose file with interpolation | Docker Docs][1]

[1]: https://docs.docker.com/compose/environment-variables/variable-interpolation/
