# Git 分支、合併、Fork 與 Pull Request 實作

帳號：`over-over-STEAK`　操作日期：2026-10-21

## 專案與分支

| 項目 | 連結 |
| --- | --- |
| 母專案（原始儲存庫） | https://github.com/se-test-examples/git-examples |
| 我的 Fork（子專案） | https://github.com/over-over-STEAK/git-examples |
| 我的開發分支 | https://github.com/over-over-STEAK/git-examples/tree/developGitBranch |

母專案與 Fork 是兩個儲存庫；`main` 與 `developGitBranch` 則是同一個儲存庫中的不同分支。

## 操作方式與紀錄原則

Fork 由使用者在 GitHub 網頁完成；後續分支、檔案提交與 PR 由使用者授權 ChatGPT 使用 GitHub 連接工具協助操作。下列 Git 指令是對應的本機操作方法，並非宣稱本次在終端機逐條執行。實際結果以文末的 GitHub 紀錄為準。

## 1. 分支（Branch）

從自己的 Fork 的 `main` 建立 `developGitBranch`，在此分支新增這份 `examples/over-over-STEAK/README.md`，並於根目錄 README 加入入口。

若用 Git Bash 或 VS Code 終端機，對應流程為：

```bash
git clone https://github.com/over-over-STEAK/git-examples.git
cd git-examples
git checkout main
git checkout -b developGitBranch
```

編輯、儲存檔案後執行：

```bash
git add README.md examples/over-over-STEAK/README.md
git commit -m "docs: 新增 Git 四項操作說明"
git push -u origin developGitBranch
```

- `checkout -b`：建立分支並切換到該分支。
- `add`：將指定檔案的變更加入暫存區。
- `commit`：建立本機版本紀錄。
- `push -u`：推送到 GitHub，並建立遠端追蹤關係。

本次透過 GitHub 連接工具建立分支與提交，因此不需額外執行本機 `push`，變更已直接儲存在遠端。

## 2. 合併（Merge）

這次的合併示範在自己的 Fork 內完成，方向為 `developGitBranch` → `main`。採用 PR 的一般 merge 方式，保留合併提交，方便查看歷史。

在 GitHub 網頁操作時：

1. 進入自己的 Fork → **Pull requests → New pull request**。
2. 確認接收儲存庫是自己的 Fork，base 選 `main`，compare 選 `developGitBranch`。
3. 檢查 Files changed，確認只有預期的文件變更。
4. 建立 PR，再使用 **Merge pull request → Confirm merge**。

若不透過 PR，等效的本機一般合併方式如下；已完成 PR 合併時不需再做一次：

```bash
git checkout main
git pull --ff-only origin main
git merge --no-ff developGitBranch -m "merge: 合併 Git 操作說明"
git push origin main
```

`--no-ff` 會在需要合併時保留合併提交。若有衝突，需先修正內容、加入暫存區並完成提交，再推送；若主分支有保護規則，應遵守規則使用 PR。

## 3. Fork

使用者在母專案頁面點選 **Fork**，Owner 選 `over-over-STEAK`，儲存庫名稱保留 `git-examples`，再點 **Create fork**。GitHub 的儲存庫資料已確認此專案的 parent 是 `se-test-examples/git-examples`。

Fork 建立的是自己帳號下的遠端副本；`git clone` 則將遠端專案下載到本機，兩者用途不同。

若需要在本機追蹤母專案，可於專案目錄執行：

```bash
git remote add upstream https://github.com/se-test-examples/git-examples.git
git remote -v
```

`origin` 指向自己的 Fork，`upstream` 指向母專案。設定 upstream 不代表取得母專案的寫入權限，也不會自動同步。

## 4. Pull Request（PR）

完成自己 Fork 內的合併後，可依以下步驟向母專案提出跨儲存庫 PR。此次連接工具建立跨儲存庫 PR 時收到 403，尚未成功送出；需由使用者在 GitHub 網頁完成。

在 GitHub 網頁操作時：

1. 母專案 → **Pull requests → New pull request → compare across forks**。
2. 依下表設定來源與目標。
3. 確認差異、填入修改原因與內容，點選 **Create pull request**。

| 欄位 | 設定 |
| --- | --- |
| base repository | `se-test-examples/git-examples` |
| base | `main` |
| head repository | `over-over-STEAK/git-examples` |
| compare | `main` |

兩邊都叫 `main`，但屬於不同儲存庫。建立 PR 是提出合併請求，不是自動完成合併；母專案須由有權限的維護者處理。`git pull` 是取得並整合遠端更新的指令，並不會建立 Pull Request。

## 四項操作比較

| 操作 | 作用 | 本例 |
| --- | --- | --- |
| Branch | 建立獨立開發路線 | `developGitBranch` |
| Merge | 整合分支變更 | 自己 Fork 的開發分支合併到 `main` |
| Fork | 建立另一個儲存庫副本 | `over-over-STEAK/git-examples` |
| Pull Request | 請求審查並合併變更 | 自己的 `main` 提交至母專案的 `main` |

## 實際結果

| 項目 | 實際結果與證據 |
| --- | --- |
| Fork | 已確認來源為 `se-test-examples/git-examples` |
| 分支 | 已建立 [developGitBranch](https://github.com/over-over-STEAK/git-examples/tree/developGitBranch) |
| 文件提交 | [8a643e0](https://github.com/over-over-STEAK/git-examples/commit/8a643e09cfc0572d4f3256b1201271f8444bc973)：新增本文件 |
| 首頁入口提交 | [152e51a](https://github.com/over-over-STEAK/git-examples/commit/152e51a662e1a3943d4530213a3014ce7ab5360d)：加入 README 連結 |
| Fork 內的 Pull Request | [PR #1](https://github.com/over-over-STEAK/git-examples/pull/1)，已建立並合併 |
| 合併提交 | [4af66ae](https://github.com/over-over-STEAK/git-examples/commit/4af66ae7cb8c62fd045a8a3be2b0beb78e0d334d) |
| 向母專案的 Pull Request | 尚未建立：GitHub 回傳 `403 Resource not accessible by integration` |

此次已完成 Branch、Merge、Fork 與自己儲存庫內的 PR 示範。跨 Fork 向母專案送出 PR 尚待完成，不能將它記成已送出或已合併。

### 完成跨 Fork PR 的最後步驟

在自己的 Fork 頁面點選 **Contribute → Open pull request**，或依第四節從母專案建立。確認 base 是母專案的 `main`、head 是 `over-over-STEAK:main`，再送出。

建議標題：`docs: 新增 over-over-STEAK 的 Git 四項操作實作`

建議說明：新增個人 Git 分支、合併、Fork 與 PR 的中文說明，並於根 README 加入入口。已在自己的 Fork 透過 PR #1 完成分支合併，這次提出修改供母專案維護者審查。

結果補記是在合併後更新自己的 `main`；保留開發分支作為原始提交紀錄。
