---
title: Linux常用相關指令
categories: [Linux]
tags: [Linux]
updated: 2022/11/30 16:00
---

# Linux 指令功能全集

## 系統啟動、管理、監看
### Systemctl：管理systemd服務

管理Systemd的各種服務，可利用 `systemctl` 來操作。

```bash
$ systemctl 操作指令 服務名稱[.service]
$ systemctl                     # 查詢目前運行中服務
$ systemctl -n --lines=整數     # 顯示列數
$ systemctl status nginx        # 顯示服務狀態
$ systemctl start nginx         # 啟動服務
$ systemctl stop nginx          # 停止服務
$ systemctl restart nginx       # 重新啟動服務
$ systemctl reload nginx        # 重新導入服務
$ systemctl enable nginx        # 開機自動啟動服務
$ systemctl disable nginx       # 開機不自動啟動服務
$ systemctl is-active nginx     # 目前是否執行服務
$ systemctl is-enabled nginx    # 開機是否自動啟動服務
```

```bash
$ systemctl list-unit-files --type service -all
```

### Journalctl：查詢systemd日誌

```bash
$ systemctl status systemd-journald.service   # 顯示journal service狀態
```

```bash
$ journalctl -u 服務名稱[.service]            # 只顯示指定systemd單元的訊息
$ journalctl _SYSTEMD_UNIT=服務名稱[.service] # 等同-u
$ journalctl -f                               # 顯示並更新最近的日誌訊息
$ 
```

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