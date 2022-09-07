---
title: Anaconda安裝套件說明
categories: [Anaconda , Python]
tags: [Anaconda , Python]
---

## 基本指令

裝完後預設工作環境為base。

### conda相關
```console
$ conda --version   # 檢視目前conda版本
$ conda --help      # 查看conda指令說明文件
```

### 套件相關
```console
$ conda list                            # 列出該工作環境下所有套件
$ conda update --all                    # 更新全部套件
$ conda remove -n <環境名稱> <套件名稱> # 移除指定工作環境下之套件
```

### 虛擬工作環境相關
```console
$ conda create -n <環境名稱> <套件名稱=版本>    # 建立新的工作環境
$ conda remove -n <環境名稱> --all              # 移除指定工作環境
$ conda env list                                # 列出有哪些工作環境
$ conda activate [環境名稱]                     # 啟動工作環境，未指定預設為base
$ conda deactivate                              # 停用工作環境
```

建立新的工作環境，可自行指定安裝某Python版本，例如：

```console
$ conda create -n my_env python=3.9
```

## 設定proxy

由於氣象局內有專用Proxy設定，如要順利連網下載安裝、更新，必須使用以下設定：

### Linux

```console
$ export http_proxy=http://proxy.cwb.gov.tw:8888
$ export https_proxy=http://proxy.cwb.gov.tw:8888
```

### Windows

```console
> set http_proxy=http://proxy.cwb.gov.tw:8888
> set https_proxy=http://proxy.cwb.gov.tw:8888
```

以上設定僅在該次登入有效，如要在下次登入也能沿用此次設定，必須寫入家目錄下的.bashrc設定（Linux）／環境變數（Windows）。

以Windows 10為例，環境變數設定在：

**控制台>系統及安全性>系統>進階系統設定>進階>環境變數>系統變數>新增**

| 變數          | 值                           |
| :------------ | :--------------------------- |
| HTTP_PROXY    | http://proxy.cwb.gov.tw:8888 |
| HTTPS_PROXY   | http://proxy.cwb.gov.tw:8888 |

Windows系統變數不區分大小寫。


## 打包環境遷移

首先須先安裝conda-pack套件，以使用`conda pack`及`conda unpack`指令來打包及解包。

```console
$ conda install -c conda-forge conda-pack   # 安裝conda-pack套件
```

接著選擇要打包的工作環境，打包產生`<環境名稱>.tar.gz`檔。再利用tar在欲放置工作環境之目錄解包。

```console
$ conda pack -n <環境名稱> --ignore-editable-packages   # 打包／壓縮
$ mkdir -p <目錄名稱>                           # 新增欲放置工作環境之目錄
$ tar -xzf <環境名稱>.tar.gz -C <目錄名稱>  # 解壓縮／解包
```

解包後便可以啟動工作環境了，啟動後還需要再一步`conda unpack`。

```console
$ conda activate <環境名稱>
(<環境名稱>) $ conda unpack # 或conda-unpack
```

這一步非常關鍵，否則會導致遷移失敗。至此，conda環境遷移結束。