# CMD 與 VS Code：Git 實作練習

日期：2026-10-22  
本次練習的接收專案：https://github.com/over-over-STEAK/git-examples  
新開發分支：`practice-branch`

## 本次如何製作

使用者授權 ChatGPT 透過 GitHub 連接工具，從 `main` 建立 `practice-branch`、提交這份文件及測試檔，再建立 PR 並以一般 merge 方式合併。下列命令為供使用者在本機重做的對應指令，並非本次實際執行的終端機紀錄。

分支和提交可以在 GitHub 網頁查看；子專案 Fork 是另一個儲存庫，不能把分支或 clone 資料夾當作 Fork。

## 1. 下載與觀察

在 CMD 或 VS Code 的終端機執行：

```cmd
git --version
git clone https://github.com/over-over-STEAK/git-examples.git git-practice
cd git-practice
git branch -a
git remote -v
```

已有本機專案時直接進入原目錄，不必重複 clone。Git 若尚未安裝，需先安裝 Git for Windows，再重新開啟終端機。

- clone：取得專案檔案與歷史。
- branch -a：查看本機與遠端追蹤分支。
- remote -v：確認 origin 指向哪個 GitHub 儲存庫。
- VS Code 可用「檔案 → 開啟資料夾」打開專案，再從「終端機 → 新增終端機」輸入指令。

## 2. 建立自己的練習分支

本次已建立 `practice-branch`。你要親自重做時改用 `my-practice-branch`，避免與既有分支重名：

```cmd
git switch main
git pull --ff-only origin main
git switch -c my-practice-branch
echo My own branch practice. > my-branch-practice.txt
git status
git add my-branch-practice.txt
git commit -m "Add my branch practice"
git push -u origin my-practice-branch
```

`switch -c` 是建立並切換分支，也可寫成 `git checkout -b my-practice-branch`。

`add` 選取要提交的變更；`commit` 建立本機版本；`push` 才上傳 GitHub。`-u` 設定本機分支與遠端分支的追蹤關係。

若出現 Author identity unknown，先設定以下資料，將信箱換成自己的提交信箱，再重新 commit：

```cmd
git config user.name "over-over-STEAK"
git config user.email "YOUR-EMAIL"
```

## 3. 分支差異與合併

完成上一節提交後，執行：

```cmd
git switch main
```

觀察工作目錄，`my-branch-practice.txt` 應消失，因為 main 還沒有這筆提交。再執行：

```cmd
git merge --no-ff my-practice-branch -m "Merge my branch practice"
git push origin main
git log --oneline --graph --all -15
```

合併方向是「指定分支 → 目前分支」，所以必須先切到 main。`--no-ff` 保留合併提交，便於觀察。合併後檔案應重新出現。

若使用 GitHub 網頁合併，就在自己的儲存庫選 Pull requests → New pull request，base 選 main，compare 選你的開發分支，檢查 Files changed，再建立 PR 並按 Merge pull request → Confirm merge。本機合併與網頁合併擇一執行即可。

若出現衝突，先在編輯器處理衝突內容，完成 git add 與 git commit 後再推送。

## 4. 建立子專案 Fork

Git 本身沒有 git fork 命令；Fork 是 GitHub 功能。

若以 `over-over-STEAK/git-examples` 作為接收端，請讓另一個帳號或有權限的組織作為 Fork 的 Owner：

1. 使用要擁有子專案的帳號登入 GitHub。
2. 開啟 https://github.com/over-over-STEAK/git-examples 。
3. 點 Fork，選擇 Owner，再點 Create fork。
4. 記下真正產生的儲存庫網址，作為子專案證據。

本文件沒有假設第二個帳號是誰，也沒有宣稱已建立這個新的 Fork。目前此步需由使用者選擇帳號並完成網頁建立。

注意：`over-over-STEAK/git-examples` 本身是從 `se-test-examples/git-examples` Fork 出來的。它可作為這次練習的接收端，仍保有原本 Fork 網路關係。

## 5. 子專案修改與 PR

下列 CHILD-ACCOUNT 必須換成真正的子專案帳號；在另一個資料夾執行：

```cmd
git clone https://github.com/CHILD-ACCOUNT/git-examples.git git-child-practice
cd git-child-practice
git switch -c child-change
echo A change from the fork. > fork-practice.txt
git add fork-practice.txt
git commit -m "Add fork practice"
git push -u origin child-change
```

推送時應登入擁有子專案寫入權限的帳號。接著在接收專案的 GitHub 網頁進入 Pull requests → New pull request → compare across forks：

| 欄位 | 選擇 |
| --- | --- |
| base repository | over-over-STEAK/git-examples |
| base | main |
| head repository | CHILD-ACCOUNT/git-examples |
| compare | child-change |

檢查差異後按 Create pull request；再由 over-over-STEAK 帳號審查並合併。

建立 PR 是提出審查與合併請求，不會自動完成合併。`git pull` 是將遠端更新取回並整合到本機，與建立 PR 不同。

## 教授希望理解的重點

| 操作 | 改變什麼 |
| --- | --- |
| branch / switch -c | 建立新的分支指標與開發路線 |
| add | 決定下一筆提交包含的變更 |
| commit | 將變更記成版本 |
| push | 更新遠端儲存庫 |
| merge | 將另一分支的歷史整合到目前分支 |
| Fork | 建立獨立但有來源關係的 GitHub 儲存庫 |
| Pull Request | 提出變更供審查與合併 |

## 官方參考

- https://git-scm.com/docs/git-switch
- https://git-scm.com/docs/git-merge
- https://docs.github.com/en/pull-requests/reference/forks
- https://docs.github.com/articles/creating-a-pull-request-from-a-fork
