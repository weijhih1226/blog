---
title: Anaconda安裝套件說明
categories: [Anaconda , Python]
tags: [Anaconda , Python]
updated: 2023/03/16 11:00
---

## 基本指令

裝完後預設工作環境為base。

### 版號說明

MAIN.MINOR.PATCH (e.g., Python 3.11.0, conda 22.9.0)

### conda相關
```console
$ conda --version                           # 檢視目前conda版本
$ conda --help                              # 查看conda指令說明文件
$ conda info                                # 查看當下工作環境設定
$ conda config --add channels conda-forge   # 增加套件下載通道至.condarc
```

### 安裝套件

- 透過conda方式：
```console
$ conda install <套件名稱>
$ conda install <套件名稱> -n <指定環境>

$ conda list -e > <套件列表檔案>                            # 輸出套件列表（非階層式）
$ conda env export -n <環境名稱> -f <環境名稱>.yml          # 輸出環境套件階層列表
$ conda env create --name <環境名稱> --file <套件列表檔案>  # 安裝套件列表
```

- 透過pip方式：
```console
$ pip install <套件名稱>

$ pip freeze -l > <套件列表檔案>  # 輸出套件列表
$ pip install -r <套件列表檔案>   # 安裝套件列表
  --force-reinstall
  --ignore-installed
```


### 套件相關

```console
  -e, --export > 檔案名稱   # 輸出檔案
  -n, --name 環境名稱
  -p, --prefix 環境路徑
  --json                    # 輸出成json檔
```

```console
$ conda list                                # 列出當下工作環境下所有套件
$ conda list -n <環境名稱>                  # 列出該工作環境下所有套件
$ conda list [正規表示式]                   # 列出當下工作環境下部分套件
$ conda list -e > <檔案名稱>                # 輸出給conda create --file
$ conda update <套件名稱>                   # 更新指定套件
$ conda update --all                        # 更新全部套件
$ conda remove <套件名稱>                   # 移除當下工作環境下之套件
$ conda remove -n <環境名稱> <套件名稱>     # 移除指定工作環境下之套件
```

### 虛擬工作環境相關
```console
$ conda create -n <環境名稱> <套件名稱=版本>        # 建立新的工作環境
$ conda remove -n <環境名稱> <套件名稱>             # 移除指定工作環境下之套件
$ conda remove -n <環境名稱> --all                  # 移除指定工作環境
$ conda rename -n <原環境名稱> <新環境名稱>         # 更名指定工作環境
$ conda env list                                    # 列出有哪些工作環境
$ conda env export -n <環境名稱> -f <環境名稱>.yml  # 輸出該工作環境套件列表
$ conda activate [環境名稱]                         # 啟動工作環境，未指定預設為base
$ conda deactivate                                  # 停用工作環境
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


## 環境遷移
### 方式一：打包帶走

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

### 方式二：從清單安裝
```console
$ conda env export -n <環境名稱> -f <套件清單檔案(.yml)>    # 輸出環境套件階層清單yaml檔案
$ conda env create -n <環境名稱> -f <套件清單檔案(.yml)>    # 從清單安裝套件
```

## 參考資料

- https://medium.com/datainpoint/python-essentials-conda-quickstart-1f1e9ecd1025