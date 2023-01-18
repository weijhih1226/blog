---
title: Django3架站實戰筆記
categories: [Django , Python]
tags: [Django , Python]
---

# 常用指令

```console
> python manage.py startapp <應用程式名稱>  # 新增應用程式
> python manage.py makemigrations           # 建立資料檔
> python manage.py migrate                  # 同步模型與資料庫
> python manage.py runserver                # 啟動server
> python manage.py createsuperuser
```

# 安裝與建立專案、應用程式

## 安裝Django套件

```console
> conda install django
```

## 建立Django專案

```console
> django-admin startproject <專案名稱>
```

建立專案之後，會在該目錄下建立兩層<專案名稱>的目錄，<專案名稱>以project1為例：

- \project1 - 專案主目錄
  - manage.py - 專案管理（建立app、啟動server和Shell）
  - \project1 - 專案設定
    - \_\_init\_\_.py
    - asgi.py - asgi伺服器和Django介面設定檔
    - settings.py - 專案設定檔
    - urls.py - url配置檔
    - wsgi.py - 網頁伺服器和Django介面設定檔

## 建立Application應用程式

```console
> python manage.py startapp <應用程式名稱>
```

建立應用程式之後，會在第一層project1目錄下建立<應用程式名稱>目錄，<應用程式名稱>以app1為例：

- \project1
  - manage.py
  - \project1
  - \app1 - 應用程式
    - \migrations - 資料表架構、版本紀錄
    - admin.py
    - apps.py
    - models.py
    - tests.py
    - views.py

### 建立必要目錄

- \project1
  - manage.py
  - \project1
  - \app1
  - \templates - 放置模板(.html檔)
  - \static - 放置靜態檔案
    - \css
    - \js
    - \img

### 建立migration資料檔

```console
> python manage.py makemigrations
```

### 模型與資料庫同步

```console
> python manage.py migrate
```

會在專案目錄下建立`db.sqlite3`檔。

### 啟動server

```console
> python manage.py runserver
```

## 環境設定

```python
BASE_DIR = Path(__file__).resolve().parent.parent
DEBUG = True
ALLOWED_HOSTS = []  # 若關閉除錯模式，則需指定該參數
INSTALLED_APPS = [
  ...
  'app1'            # 新增建立的應用程式
]
TEMPLATES = [
  {
    "BACKEND": "django.template.backends.django.DjangoTemplates",
    "DIRS": [BASE_DIR / 'templates'],  # 指定模板路徑
        "APP_DIRS": True,
        ...
  },
]
LANGUAGE_CODE = "zh-hant"   # 改為繁體中文
TIME_ZONE = "Asia/Taipei"   # 改為臺北時區
STATICFILES_DIRS = [        # 加入static路徑
    BASE_DIR / "static",
]
```

### 更改資料庫

可使用PostgreSQL、MariaDB、MySQL或Oracle。

以MySQL為例：

1. [安裝MySQL Server](https://dev.mysql.com/downloads/mysql/)。
2. [安裝步驟說明](https://blog.hungwin.com.tw/windows-server-mysql-community-install/)。

安裝完請先確定MySQL Server是否有新增至環境變數。

- 登入MySQL：

  ```console
  > mysql -u 使用者帳號 -p
  Enter password: ********
  ```

- 常用SQL指令：

  ```sql
  mysql> SELECT VERSION();  /* 檢查資料庫版本 */
  mysql> SELECT USER();     /* 檢查目前登入使用者 */
  mysql> SHOW ENGINES \G    /* 顯示目前資料庫支援的儲存引擎 */

  mysql> CREATE DATABASE 資料庫名稱   /* 建立資料庫 */
  mysql> USE 資料庫名稱               /* 進入資料庫 */
  mysql> DROP DATABASE 資料庫名稱     /* 移除資料庫 */
  ```

原有環境資料庫設定為SQLite3：

```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.sqlite3", # 預設
        "NAME": BASE_DIR / "db.sqlite3",
    }
}
```

改為MySQL的設定：

```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.mysql",
        "NAME": "資料庫名稱",
        "USER": "使用者名稱",
        "PASSWORD": "密碼",
        "HOST": "localhost",
        "PORT": "3306",
    }
}
```

或使用配置檔：

```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.mysql",
        "OPTIONS": {
            "read_default_file": "配置檔路徑(my.cnf)",
        },
    }
}
```

```my.cnf
[client]
host = localhost
database = 資料庫名稱
user = 使用者名稱
password = 密碼
default-character-set = utf8
port = 3306
```

# 參考資料

1. https://blog.hungwin.com.tw/windows-server-mysql-community-install/