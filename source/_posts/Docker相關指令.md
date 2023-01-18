---
title: Docker常用相關指令
categories: [Docker]
tags: [Docker]
updated: 2023/01/18 16:48
---

# Docker 指令功能全集

``` bash
$ docker ps                             # 查看各容器使用狀態（ID、NAME）
$ docker ps -a                          # 查看所有容器使用狀態（包含已停止容器）
$ docker inspect <容器ID|名稱>          # 檢查某Container的狀態（進入點、執行狀態及其他詳細資料）
$ docker stats <容器ID|名稱>            # 查看各Container的CPU、記憶體及網路使用
$ docker exec <容器ID|名稱> [COMMAND]   # 在外部向Container內執行指令
$ docker logs <容器ID|名稱>             # 將Container內的輸出顯示到螢幕上
$ docker service ls                     # 列出Service狀態
$ docker service logs <服務ID|名稱>     # 將Service內的輸出顯示到螢幕上
```

因Docker可能裝在root權限底下，如沒有root使用Docker的權限，請在各指令前加sudo！

## 查看容器使用狀態

``` bash
$ docker ps
$ docker ps -a  # 查看所有容器使用狀態
CONTAINER ID    IMAGE                   COMMAND CREATED     STATUS      PORTS                               NAMES
c7ee35c5de98    127.0.0.1:5000/*:3.3.0  "/…"    7 days ago  Up 7 days   0.0.0.0:8002->8000/tcp, :::8002->8000/tcp  qpesums_web
```

Container的使用狀態會列出Container ID、所使用的IMAGE（映像檔）位置、啟動容器之後會執行的COMMAND（指令）、哪時候被CREATED、目前的STATUS、對應到的PORTS（連接埠），以及Container的NAMES（名稱）。

若後面不加任何options僅會列出正在運行的Container。

## 查看各Container的CPU、記憶體及網路使用

``` bash
$ docker stats <容器ID|名稱>
$ docker stats --no-stream          # 不滾動顯示
```

## 在外部向容器內執行指令

``` bash
$ docker exec
Usage: docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
$ docker exec -it <Container ID|NAME> bash
```

表示進入Container後第一個執行的是bash介面。

## 列出Docker服務並查詢服務狀態
### 列出Docker服務

``` bash
$ docker service ls
ID              NAME        MODE    REPLICAS    IMAGE                   PORTS
zo97osyy73we    qpeplus     global  3/3         127.0.0.1:5000/*:3.3.0  *:8000->8000/tcp
```

將會列出各個服務的ID、NAME（名稱）、MODE（模式）、REPLICAS（複本）、所使用到的IMAGE（映像檔），以及所對應到的PORTS（連接埠）。

### 查詢服務狀態

``` bash
$ docker service logs <ID|NAME>
```

## 查看Swarm Nodes

``` bash
$ docker node ls
```

再利用查到的ID或名稱去查詢該服務狀態。

## 移除多餘容器(container)、網路(network)、映像(image)、數據卷(volume)

```console
$ docker container prune        # 移除未用容器
$ docker network prune          # 移除未用網路
$ docker image prune            # 移除未用映像
$ docker volume prune           # 移除未用數據卷
$ docker system prune           # 移除所有(Docker 17.06.0前包含數據卷)
$ docker system prune --volumes # 移除所有包含數據卷
```

[OPTIONS]有`-a`及`-f`。

## 其他指令

```console
$ docker pull <映像檔名稱>:<版本號>             # 拉取映像檔（未啟動容器）
$ docker run <映像檔名稱>:<版本號>              # 從映像檔啟動容器（若本機無該映像檔，則從遠端拉取）
$ docker rename <舊容器名稱|ID> <新容器名稱>    # 容器更名
$ docker version                                # 顯示詳細版本
```

### DOCKER RUN

- `-d` - 在背景運行容器，並列出容器ID
- `--name <指定容器名稱>` - 指定容器名稱
- `-p <本機通訊埠>:<容器通訊埠>` - 本機的通訊埠對應到容器的通訊埠
- `-t` - 分配pseudo-TTY

```console
$ docker run <映像檔名稱>:<版本號>                      # 從映像檔啟動容器
$ docker run --name <指定容器名稱> <映像檔>             # 指定容器名稱
$ docker run -dp <本機通訊埠>:<容器通訊埠> <映像檔>     # 指定通訊埠並在背景運行
```
