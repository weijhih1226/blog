---
title: Linux常用相關指令
categories: [Linux]
tags: [Linux]
updated: 2023/01/18 16:48
---

# Linux 指令功能全集

## 系統啟動、管理、監看
### Systemctl：管理systemd服務

管理Systemd的各種服務，可利用 `systemctl` 來操作。

```bash
$ systemctl 操作指令 服務名稱[.service]
$ systemctl                           # 查詢目前運行中服務
$ systemctl -n --lines=整數           # 顯示列數
$ systemctl start <服務名稱>          # 啟動服務
$ systemctl stop <服務名稱>           # 停止服務
$ systemctl restart <服務名稱>        # 重新啟動服務
$ systemctl status <服務名稱>         # 顯示服務狀態
$ systemctl reload <服務名稱>         # 重新載入服務配置(.conf)
$ systemctl daemon-reload <服務名稱>  # 重新載入systemd服務配置(.service)
$ systemctl enable <服務名稱>         # 開機自動啟動服務
$ systemctl disable <服務名稱>        # 開機不自動啟動服務
$ systemctl is-active <服務名稱>      # 目前是否執行服務
$ systemctl is-enabled <服務名稱>     # 開機是否自動啟動服務
```

```bash
$ systemctl list-unit-files --type service --all   # 列出所有服務
```

### Journalctl：查詢systemd日誌

```bash
$ systemctl status systemd-journald.service   # 顯示journal service狀態
```

```bash
$ journalctl -u <服務名稱>                      # 只顯示指定systemd單元的訊息
$ journalctl _SYSTEMD_UNIT=<服務名稱>           # 等同-u
$ journalctl -f                                 # 顯示並更新最近的日誌訊息
$ journalctl -p <0~7>                           # 要查看log的最高層級
$ journalctl --since <YYYY-MM-DD[ HH:MM[:SS]]>  # 列出自某時間點之日誌訊息
```

- log層級：
  - 0 - emerg
  - 1 - alert
  - 2 - crit
  - 3 - err
  - 4 - warning
  - 5 - notice
  - 6 - info
  - 7 - debug

### Systemd設定

- 功能：
  - `.service` - 處理服務
  - `.target` - 處理runlevel
  - `.mount` - 處理檔案系統掛載
  - `.timer` - 處理工作排程

- 目錄：
  - /etc/systemd/system - 權限最高
  - /run/systemd/system - 權限中等
  - /lib/systemd/system或/usr/lib/systemd/system - 權限最低

- 範例：
  ```conf
  [Unit]
  Description=A description for this service unit.  # 該systemd服務單元設定檔案的描述
  After=network.target  # 指定systemd目標單元，確保本單元在systemd目標單元運行後運行。本範例指在網路功能啟用後啟用，以讓本服務單元可正常聯網。
  
  [Timer]
  OnStartupSec=5ms
  OnUnitInactiveSec=10s
  OnCalendar=*-*-* *:*:00                 # 每分鐘0秒執行
  Unit=<指定服務>                         # 指定的服務

  [Service]
  User=user                               # 使用者名稱
  Group=group                             # 使用者群組
  WorkingDirectory=/path/to/folder        # 工作目錄
  Environment="VARIABLE_1=VALUE_1" "VARIABLE_2=VALUE_2" # 直接定義多重環境變數
  EnvironmentFile=/path/to/environment    # 環境變數
  ExecStart=/path/to/program [arguments]  # 本服務單元啟動時要執行的應用程式
  Restart=always                          # 設定執行程式在怎樣情況關閉時，再執行相同指令
  RestartSec=3s                           # 關閉後到重啟間隔時間，預設100ms
  
  [Install]
  WantedBy=multi-user.target              # 運行該systemd目標單元時，會同時運行本單元
  ```
  - `Restart`可設定為：
    - `no`：不重啟。(預設值)
    - `always`：不管什麼原因都會重啟。
    - `on-success`：只在成功運行結束後(也就是行程回傳的Exit Status為0的時候)才進行重啟。(這蠻少用)
    - `on-failure`：在運行失敗後(行程回傳的Exit Status非為0、被使用Kill信號強制關閉、逾時或是餵狗沒反應時)才進行重啟。
    - `on-abnormal`：在意外地運行失敗後(被使用Kill信號強制關閉、逾時或是餵狗沒反應時)才進行重啟。
    - `on-abort`：被使用Kill信號強制關閉時才進行重啟。
    - `on-watchdog`：餵狗沒反應時才進行重啟。


## 修改自動登出時間

```bash
$ echo $TMOUT     # 查詢自動登出時間
```

修改`/etc/profile`或`/etc/bashrc`或`~/.bash_profile`或`~/.bashrc`：

```bash
TMOUT=600   # 表示10分鐘之後自動登出
TMOUT=      # 表示關閉自動登出
```

重新啟動bash配置：

```
$ source ~/.bash_profile
```

## 更換Shell

```bash
$ printf "%s\n" $SHELL    # 印出當前shell
$ cat /etc/shells         # 列出所有可用shell
$ yum install zsh
$ chsh -s $(which zsh) $(whoami)

$ git config --global core.autocrlf false  # github儲存庫為Unix格式，若要離線安裝下載至Windows再上傳至Linux server，須取消自動轉換CRLF
$ git clone https://github.com/ohmyzsh/ohmyzsh.git ~/.oh-my-zsh     # 下載Oh-my-zsh獲得更多zsh樣式

cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```

修改主題，例如：

```bash
# ZSH_THEME="robbyrussell"
ZSH_THEME="agnoster"
```

## Linux系統根目錄介紹

- /bin
  - binary程式所在目錄

- /boot
  - 系統啟動核心文件

- /dev
  - 系統設備文件

- /etc
  - 系統及應用軟體的配置文件
  - /etc/passwd - 記錄用戶訊息

- /home
  - 所有使用者的個人文件

- /lib
  - 共享函式庫
  - 類似Windows系統之動態函式庫(.dll)

- /lib64
  - 64位元系統之共享函式庫

- /lost+found
  - 保存遺失文件

- /media
  - 可移動設備的掛載目錄

- /mnt
  - 掛載設備目錄

- /opt
   - option - 目錄是給第三方的軟體放置的目錄，也就是給那些自行額外安裝的軟體放的目錄。

# 參考資料
- https://magiclen.org/linux-init-application-service/