---
title: 利用「Hexo」架設個人部落格
categories: [GitHub , Hexo]
tags: [GitHub , Hexo , Node.js]
date: 2022/07/21 14:25
updated: 2022/07/25 14:52
---

**自**從進入職場、學習不同的程式語言或工具以來，發現累積的說明文件愈來愈多，但卻又沒有一套系統性的整理，遇到問題一時要找就開始有點麻煩了😓另外，很久也沒藉由寫點東西，來反思自己的生活了。希望可以經營一個可以累積過往的程式經驗，又可順便記錄生活種種點點滴滴的部落格，而且重點是要方便記錄跟找尋！於是找到了Hexo。「[Hexo](https://hexo.io/zh-tw/)」是一個讓你很容易上手部署自己個人部落格，以及發佈程式類說明文章的套件工具。而且[主題](https://hexo.io/themes/)及[外掛](https://hexo.io/plugins/)超多任你選擇，只需要懂得一些語法架構，即可做出美美的、與眾不同的網頁！

## 安裝及設定 Hexo

### 利用 npm 安裝 Hexo

在安裝Hexo之前，須先安裝[Node.js](https://nodejs.org/zh-tw/)，以利用npm來安裝Hexo。

另外也須安裝[Git](https://git-scm.com/)。

``` bash
npm install hexo-cli -g
npm list # 檢查是否成功安裝套件
```

### 建立並複製公開儲存庫（repository；repo）至本地端

在欲放置儲存庫的專案目錄下，複製GitHub上的公開儲存庫至此。

``` bash
git clone https://github.com/使用者名稱/使用者名稱.github.io
```

### 設定 Hexo

1. 從[Hexo的GitHub上](https://github.com/hexojs/hexo-starter)下載所需設定文件：

    ``` bash
    hexo init blog # 在當前目錄建立blog資料夾（可自訂名稱）
    cp blog/* <本地儲存庫> # 範例：D:\xxx.github.io
    ```

2. 根據複製檔案中的「package.json」自動安裝需要的套件：

    ``` bash
    cd <本地儲存庫>
    npm install # 依據當前目錄package.json
    ```

3. 安裝hexo-deployer-git，以一鍵部署：

    ``` bash
    npm install hexo-deployer-git --save # 無此套件後續deploy可能出錯
    ```

4. 修改設定檔_config.yml：

    主要設定網站內容的檔案。

    設定網站標題、作者、以及網站的URL等基本設定。

    加入以下程式碼，以自動部署到GitHub Pages：

    ``` yaml
    deploy:
      type: git
      repo: <儲存庫URL>
      branch: [branch]
      message: [message]
    ```

### 部署網頁

- 將目前資料夾中所有靜態網頁清除：

    ``` bash
    hexo clean
    ```

- 按照當前設定檔，產生對應的靜態網頁：（可省略）

    ``` bash
    hexo generate
    ```

- 在本地端產生網頁，確認當前設計（預設127.0.0.1:4000）：（可省略）

    ``` bash
    hexo server
    ```

- 將目前本地端靜態網頁自動部署至GitHub Pages：

    ``` bash
    hexo deploy
    ```

### Hexo 目錄架構

1. node_module：

   這個專案所需要的套件（剛剛透過npm install所安裝）。

2. public：

   存放網頁中的文章、圖片、標籤等資料。

3. scaffolds：

   存放網站的內容模板。

4. themes：

   存放網站的主題。

---

## 參考資料

1. [文件 | Hexo](https://hexo.io/zh-tw/docs/)
  
2. [建立一個屬於自己的(程式)部落格！. 透過 Github Page + Hexo來免費建立自己的Bl | by 吳明倫 MingLun Wu | Medium](https://minglun-wu.medium.com/%E5%BB%BA%E7%AB%8B%E4%B8%80%E5%80%8B%E5%B1%AC%E6%96%BC%E8%87%AA%E5%B7%B1%E7%9A%84-%E7%A8%8B%E5%BC%8F-%E9%83%A8%E8%90%BD%E6%A0%BC-4d295ed96236)
  
3. [nodejs-tw/nodejs-wiki-book/zh-tw/node_npm.rst:Node.js Taiwan 社群協作中文電子書](https://github.com/nodejs-tw/nodejs-wiki-book/blob/master/zh-tw/node_npm.rst)
