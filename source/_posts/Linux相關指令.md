---
title: Linux常用相關指令
categories: [Linux]
tags: [Linux]
---

# Linux 指令功能全集

## 系統啟動、管理、監看

管理Systemd的各種服務，可利用 `systemctl` 來操作。

```bash
$ systemctl 操作指令 服務名稱[.service]
$ systemctl                     # 查詢目前運行中服務
$ systemctl status nginx        # 顯示服務狀態
$ systemctl start nginx         # 啟動服務
$ systemctl stop nginx          # 停止服務
$ systemctl restart nginx       # 重新啟動服務
$ systemctl reload nginx        # 重新導入服務
$ systemctl enable nginx        # 開機自動啟動服務
$ systemctl disable nginx       # 開機不自動啟動服務
$ systemctl is-active nginx     # 目前是否執行服務
$ systemctl is-enabled nginx    # 開機是否自動啟動服務
$ journalctl 
```

```bash
$ systemctl list-unit-files --type service -all
```

## 更換Shell

```bash
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