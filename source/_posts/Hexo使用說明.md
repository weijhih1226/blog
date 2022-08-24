---
title: Hexo使用說明
categories: [GitHub, Hexo]
tags: [hexo, git]
date: 2022/08/23 12:10
---

## Hexo初始化

使用hexo init初始化Hexo的目錄架構。
```bash
D:\>hexo init [自訂目錄名稱]
```
或是到自訂名稱目錄內使用hexo init指令。
```bash
D:\[自訂目錄名稱]>hexo init
```
後面[自訂目錄名稱]以blog為例。

## 更換主題

1. 到 https://hexo.io/themes/ 選擇想要的主題。
2. 複製該主題的Github網址到本地端。

   ```bash
   D:\blog\themes>git clone [該主題的Github網址]
   ```

3. 修改themes目錄上一層的_config.yml：

   修改_config.yml中themes項目的主題名稱為剛下載下來的目錄名稱。

   ```yaml
   # Extensions
   themes: fluid

   # Deployment
   deploy:
     type: git
     repo: https://github.com/weijhih1226/blog.git
     branch: gh-pages
   ```

4. 接著執行hexo clean、generate及server，即可看到更換後的主題了。

   ```bash
   > hexo clean
   > hexo generate  # hexo g
   > hexo server    # hexo s
   ```
  