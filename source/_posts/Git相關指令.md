---
title: Git一二三，同步真簡單
categories: [GitHub , Git]
tags: [GitHub , Git]
date: 2022/08/24 14:07
updated: 2023/11/15 11:52
---

Git是一個多工團隊協作版控的好用工具，雖然一開始的概念有點小複雜，我也是花了好一陣子才漸漸搞懂它，因此。對於初學者而言，在了解Git指令之前，先要有local（本地）與remote（遠端）repository（儲存庫）的概念，以及各自的branch（分支）之後，或許會比較好入手。

## Git指令全集

### 概念

0. 初始化git工作目錄

   狀態：未初始化

   ```bash
   git init
   ```

   初始化後，目錄下新增.git目錄。

1. 工作區（Working Directory）

   狀態：未追蹤（Untracked files，對於新加入檔案或提交後新建立檔案）、已更改（Changes not staged for commit，對於提交後再被修改之檔案）

   直接編輯的地方，在桌機上可直接操作檔案。

   ```bash
   # 工作區 -> 暫存區
   $ git add .    # 將檔案從工作目錄加入至暫存區
   ```

2. 暫存區（Staging Area）

   狀態：等待提交（Changes to be committed）

   數據暫時存放的區域，介於工作區與儲存庫之間。

   ```bash
   # 暫存區 -> 儲存庫
   $ git commit                   # 新開編輯器編輯提交訊息
   $ git commit -m "提交訊息"     # 將暫存區內容移至儲存區
   $ git commit -am "提交訊息"    # 從工作區移至儲存庫，對已在儲存庫的檔案有效
   ```

3. 儲存庫／數據庫（Repository）

   狀態：已提交（Committed）

   存放已經提交的數據。

   1) 本地端儲存庫（Local）

      為了方便自己使用的儲存庫，通常是個人開發的電腦或機台。

      ```bash
      # 本地端 -> 遠端儲存庫
      $ git push        # 推送到遠端儲存庫

      # 本地端複製
      $ git clone <本地目錄名稱> <新本地目錄名稱|路徑>  # 複製本地儲存庫至指定目錄
      ```

   2) 遠端儲存庫（Remote）

      為了讓多人共享而建立的儲存庫，通常是一個共用的伺服器。

      ```bash
      # 遠端 -> 本地端儲存庫
      $ git fetch <遠端名稱> <遠端分支名稱>     # 下載遠端指定分支
      $ git fetch --all                         # 下載遠端所有分支
      $ git clone <遠端名稱>                    # 下載遠端預設分支
      $ git clone <遠端名稱> -b <本地分支名稱>  # 下載遠端預設分支至指定本地分支
      $ git clone <遠端名稱> <目錄名稱|路徑>    # 下載遠端預設分支至指定目錄

      # 遠端儲存庫 -> 工作區
      $ git pull <遠端名稱> <遠端分支名稱>:<本地分支名稱>   # git fetch + git merge
      
      ```

### 常用指令

```bash
git init                  # 進行版本控制（此時就會有.git目錄產生）
git status                # git狀態
git add .                 # 將檔案加至暫存區
git commit -m "提交訊息"  # 提交檔案至儲存庫
git filter-branch --tree-filter "rm -f config/database.yml"   # 針對過去的提交批次修改紀錄並提交
```

### 將存放庫初始化

首先必須要先將Git本地的存放庫初始化啦！

到了想要透過Git監控的目錄git init，此時在目錄下會多出.git的隱藏目錄，這就相當於在VS code下在原始檔控制中的「將存放庫初始化」的功能。

```bash
git init  # 在當前目錄下創建.git以監控版更
```

### 查看當前git狀態

先透過git status來查看當前git狀態。

```bash
$ git status  # 查看當前git狀態
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
```

從輸出的內容，可以看到目前本地的分支名稱為master、沒有任何commit（提交），以及哪些檔案還沒被add（加入）追蹤（untracked）。

目錄下可加入.gitignore檔案，裡面的內容可以加入不想同步追蹤的檔案名稱，以將這些檔案排除在git的檢查之外。

目錄下加入.gitkeep則是強制git在檢查時，將空目錄也涵蓋進來。

### 一、add（加入）untracked（未追蹤）檔案至暫存的變更

當在目錄當中新增的檔案，只要是非空目錄、未被.gitignore排除檢查的檔案，或者並非是另一個含有.git的目錄，都會被列為檢查對象。

利用VS code的原始檔控制，會自動偵測已變更的檔案（新增的檔案右方會顯示A(Add)、新增後變更的檔案會顯示M(Modified)），在「變更」欄的右方「+」號則可以將新增或變更的檔案加入到暫存的變更；在「暫存的變更」右方「-」號則是可以選擇取消變更的檔案，等修改好再加進去。

如果利用指令列，則須透過git status來看各個檔案的情況。

假設我們先新增一個test.md檔，這時候git status會列在untracked，於是我們git add：

```bash
git add test.md
```

這時候再git status，會看到檔案已被加入：

```bash
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   test.md
```

如果再修改test.md檔案中的文字，再次git status則會看到：

```bash
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   test.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   "test.md"
```

代表有一個版本已經加入暫存的變更等待提交，而另一個則是因為我們後來的修改而產生的版本，尚未加入提交的行列。

> 若是目錄中含有其他git監測的目錄，則無法加入追蹤。

```bash
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        themes/fluid/
```

像是Hexo部分目錄本身即含有git版控功能，因此部分目錄無法同步加入追蹤。

### 二、新增訊息並commit（提交）

當我們新增檔案或修改某些檔案裡的文字時，可以利用git commit分批提交更新訊息，把每一次的更新訊息記錄下來：

```bash
$ git commit -m "add test.md"
[github 1da2962] revise Git相關指令.md
 0 file changed, 1 insertions(+)
 create mode 100644 "test.md"
```

提交之後會顯示提交的分支名稱、提交ID、提交訊息、幾個檔案新增或者被改變。

每次修改檔案之後，都要記得將檔案再度git add到要提交的行列當中。

在提交之後，會發現本地端也新增了master分支，代表我們提交的內容會在我們本地的master分支裡。每修改某個檔案都可以在訊息欄做備註。但我們要推送的是遠端分支，而不是本地分支，要確定要提交上去的是哪一個分支。

### 三、push（推送）至remote repository（遠端儲存庫）

緊接著，你在工作目錄中陸陸續續新增的檔案，要放到遠端儲存庫供他人使用，就必須先將遠端儲存庫加入路徑之中。Github儲存庫URL在頁面右上角的「code」當中，使用HTTPS或SSH都可以，將想要放置檔案的Github儲存庫URL透過git remote add加入即可。

```bash
git remote add <遠端名稱> <遠端儲存庫URL>   # 將該遠端加入fetch跟push的路徑
git remote -v                               # 查看遠端分支名稱及路徑
git remote remove <遠端名稱>                # 對應之移除指令
git remote rename <原遠端名稱> <新遠端名稱> # 更改遠端名稱
```

以遠端名稱為github為例：

```bash
$ git remote add github https://github.com/weijhih1226/blog.git
$ git remote -v
github    https://github.com/weijhih1226/blog.git (fetch)
github    https://github.com/weijhih1226/blog.git (push)

$ git branch -v               # 查看branch名稱、commit訊息
$ git branch -vv              # 查看branch名稱（有upstream則會顯示）、commit訊息
$ git branch -l               # 查看分支名稱（本地）
$ git branch -a               # 查看所有分支名稱（本地及遠端）
* master                      # 本地分支（目前的分支）
  remotes/github/gh-pages     # 遠端分支
  remotes/github/main         # 遠端分支
```

從上面git branch -a當中，可以看到因為目前本地端是在master分支底下，而遠端則在github底下有自動抓到2個分支。

利用git checkout則可以切換分支。

```bash
git checkout <分支名稱>
git checkout -b <新的分支名稱>
```

例如我們如果想將本地端的檔案同步到遠端的main分支上：

```bash
git checkout main
```

```bash
git push <遠端名稱> <分支名稱>
```

例如：

```bash
git push github main
```

### 其他常用指令

```bash
git branch <分支名稱>                               # 建立本地分支
git branch -d <分支名稱>                            # 刪除本地分支
git branch <-m|--move> [<舊分支>] <新分支>          # 移動／更名分支
git branch <-M|--move --force> [<舊分支>] <新分支>  # 強制移動／更名分支
git push <遠端名稱> --delete <遠端分支名稱>         # 刪除遠端分支
git push <遠端名稱> :<遠端分支名稱>                 # 刪除遠端分支
git clone <遠端專案URL> -b <分支名稱>               # 複製遠端分支
git pull <遠端名稱> <遠端分支名稱>:<本地分支名稱>   # git fetch + git merge
```

#### 關於git組態

```bash
git config --global core.autocrlf true      # 自動轉換CRLF
git config --global user.name "xxx"         # 使用者名稱
git config --global user.email "xxx@xx.com" # 使用者電子郵件
```

#### git忽略追蹤

- 狀況一：新增過濾條件**後新增的檔案**
  - 符合規則 Git 就不會去追蹤。
- 狀況二：新增過濾條件**前新增的檔案**
  - 沒有額外處理還是會被追蹤。
  1. 於專案根目錄新增 `.gitignore` 檔案。
  2. 於檔案內新增需要忽略的檔案、目錄等，例如：

     ```yaml
     *.conf          # 檔案
     __pycache__/    # 目錄
     ```

  3. 若以 git status 查看，會發現記錄只有新增一個檔案而已，而這就是因為先有 Git 記錄才新增 .gitignore 檔案，因此進行以下步驟清除快取並重新加入追蹤，才會套用新的 .gitignore 設定：

     ```bash
     # 清除本機 Git 的快取，就是將所有檔案移除 Git 的追蹤，但沒有刪除檔案
     $ git rm -r --cached .
     # 重新加入 Git 追蹤，這時就會重新套入 .gitignore 設定
     $ git add .
     # 重新 commit ，並會忽略設定在 .gitignore 的檔案
     $ git commit -m 'update .gitignore'
     ```

---

## 參考資料

- <https://shichia.medium.com/gitignore-忽略那些不該上傳的-git-檔案-2031ac4dc679>
