## Git 與 GitHub 的區別

首先初學者很容易會搞混的是 **Git 和 GitHub 是不同的東西**
- **Git** 是一個**版本控制工具**，就像玩遊戲時儲存進度一樣，可以保存專案的歷史紀錄。當出現問題時，透過 Git 可以「回到」先前的版本，防止因錯誤導致資料遺失或破壞。
- **GitHub** 則是一個**雲端儲存平台**，用來存放 Git 儲存庫（repository, repo），也方便與其他人協作開發專案。GitHub 提供遠端版本控制的功能，讓多個人可以在不同的設備上共同工作，並且同步專案進度。

#### 安裝 Git
在 Windows 上使用 Git 需要先進行安裝。可以由 [git for windows ](https://git-scm.com/download/win)下載。安裝完成後，即可使用 Git 進行版本控制。
![git-flow](https://hackmd.io/_uploads/B193fBl2C.png)

接下來，將說明簡單的 git 概念以及演示如何使用 Git 以及如何將本地的儲存庫（local repo）上傳到 GitHub。


## 創建 repository

在開始進行版本控制前，首先需要在 GitHub 上創建一個新的儲存庫（repo）。在創建過程中，GitHub 會要求你選擇儲存庫的可見性：

- **Private（私人）**：僅限擁有權限的用戶存取。使用 private repo 時，不需要特別設置 SSH key 就可以進行上傳，因為只有授權用戶可以操作。
- **Public（公開）**：任何人都可以查看這個儲存庫的內容，但為了防止不當使用，公開的儲存庫上傳需要進行身份驗證。因此，需要創建並配置 SSH key 來確認上傳者的身份，以避免任意用戶向你的遠端儲存庫（remote repo）上傳資料。

## 創建 ssh key

SSH key 是由 **公鑰 (public key)** 和 **私鑰 (private key)** 組成的一對密鑰。

當使用 SSH 與網站（例如 GitHub、Netlify 等）或伺服器進行安全連線時，會將公鑰放到這些服務上，來與本地端的私鑰配對使用。私鑰則必須妥善保管，不能外洩，否則他人就有了未經授權存取或盜用本地資料的權限。


#### 創建 SSH Key

以下是創建 SSH key 的步驟：
1. 在檔案管理器中，在任意資料夾內右鍵選擇「Open Git Bash here」，然後輸入以下指令生成一對 SSH key：
    ```
    ssh-keygen
    ```
2. 接著終端機會詢問 3 個問題：
    1. 金鑰要存放在哪裡？
        建議使用預設路徑，直接按下 `Enter` 即可。
    2. 是否要設定 passphrase？
        若設定了 passphrase，每次使用 SSH key 時都需要額外輸入密碼。若不需要，直接按 `Enter` 跳過。
    3. 再輸入一次 passphrase
        若上一個步驟沒設置密碼，這裡也可直接按 Enter 跳過。
3. 完成後，系統將生成一對 SSH 金鑰，終端機會顯示金鑰存放的路徑：
    - 私鑰路徑：`C:/Users/ming/.ssh/id_rsa`
    - 公鑰路徑：`C:/Users/ming/.ssh/id_rsa.pub`


#### 將公鑰上傳至 GitHub

生成的公鑰需要上傳到 GitHub 以進行匹配，成功匹配後便可安全推送專案。
根據生效範圍，有兩種常見的上傳方式：
1. **放到 repository 專案**，只針對特定專案生效 : 
    - 在 GitHub 專案頁面點選「Settings」→「Deploy keys」→「Add deploy key」。
    - 輸入 title 和 key，並勾選「Allow write access」以允許寫入操作。
2. **放到 github 帳戶**，所有的專案都會被作用 : 
    - 點選「個人頭像」→「Settings」→「SSH and GPG keys」→「New SSH key」。
    - 輸入 title 和 key，並選擇「Authentication key」[^註腳]。

#### 完成 SSH Key 設定
當 ssh key 正確上傳，就可以使用 SSH 進行本地專案(local repo)的推送或拉取操作。
SSH 連線會自動使用您配置的私鑰來匹配遠端儲存庫(remote repo)的公鑰，從而建立安全連線。


## git 指令操作

#### git init
在資料夾中初始化一個 Git 儲存庫，讓 Git 開始對該資料夾進行版本控制。通常只需要執行一次。如果是使用 `git clone` 複製遠端儲存庫，已經包含初始化步驟，因此不需要再執行 `git init`。
```
git init
```

#### git status
查看工作目錄和暫存區的狀態。紅字表示未追蹤的檔案或已修改但未暫存的檔案，綠字表示已加入暫存區的檔案。
```
git status
```

#### git add
將檔案加入暫存區，為下一次提交做準備。可以指定單個檔案或所有檔案。
```
// 加入單個檔案
git add 123.txt
// 加入當前目錄下的所有檔案
git add .
```

#### git commit
將暫存區的變更提交到 local repo 。每次提交都會建立一個新的版本。
```
// 會打開文字編輯器，讓使用者撰寫提交訊息
git commit
// 直接以 "message" 提交，不會打開編輯器
git commit -m "message"
```

#### git log
查看提交的歷史紀錄，按時間順序顯示提交訊息。
```
git log
```

### git branch
建立或管理分支。這裡可以重命名分支為 main（常見的主分支名稱）。
```
git branch -M main
```

#### git remote
將 local repo 連結到 remote repo，通常使用 SSH 或 HTTPS 進行連結。url 應該是 remote repo 的 URL，例如：`git@github.com:your_account/your_repository.git`。
```
// 創建名為 origin 的遠端檔案庫 
git remote add origin url
```

#### git push
將 local repo 的變更推送到 remote repo 。
```
// 推送本地 main 分支到遠端名為 origin 的儲存庫
git push -u origin main
```

#### git pull
從 remote repo 拉取最新的變更，並將其整合到 local repo。
當 remote repo 的變更比 local 新時，推送操作會被拒絕，此時需要先進行 pull，再 push。
```
$ git pull
```

#### git colone
從 remote repo 複製整個專案（包括檔案、歷史紀錄和分支）到 local repo。
```
$ git colone url
```

## 常用指令

#### 創立一個新的 repository 到 github。
```
// 建立一個 README.md 檔案，初始化資料夾並加入該檔案：
echo "# your_repository" >> README.md
git init
git add README.md

// 設定作者身份，使接下來的 commit git 會附加作者資訊
git config --global user.email "you@example.com"
git config --global user.name "Your Name"

// 進行提交，並將該提交與 main 分支關聯
git commit -m "first commit"
git branch -M main

// 連接 remote repo 並推送到 GitHub：
git remote add origin git@github.com:your_account/your_repository.git
git push -u origin main
```
#### 對資料夾內的已更新檔案上傳到 github 上。
```
git add .
git commit -m "message"

// 將提交推送到遠端儲存庫的 main 分支
git push -u origin main
```

以上是常用的 git 基礎指令介紹，後續的分支可能就另外再搞一篇吧。



## Reference

- [【GIT 小教室】SSH Key 的建立與設定](https://www.youtube.com/watch?v=CeC_qyQHiCE)
- [【git教學 #1】15分鐘學會git & github（附實例）](https://www.youtube.com/watch?v=Zd5jSDRjWfA)
- [【Day10】Git 版本控制 - 將檔案 push 到 GitHub 的懶人包](https://ithelp.ithome.com.tw/articles/10271811)
- [30 天精通 Git 版本控管 (01)：認識 Git 版本控管](https://ithelp.ithome.com.tw/articles/10132053)


[^註腳]: [兩個鑰匙之間的差異](https://stackoverflow.com/questions/73673920/do-i-need-authentication-as-well-as-signing-keys-on-github)
Authentication keys 是用於驗證 GitHub 上特定帳戶的憑證，當作 git 指令操作時可以證明是帳戶的擁有者。
Signing keys 是用於證明操作是由本人進行操作的。