# Scenarios — 情境模板

針對 `git diff --cached` 反映的不同情境，給出建議的 commit 訊息模板。SKILL.md 流程的「套用情境模板」步驟會對照本檔；若任一情境命中就直接套，未命中就走預設規則。

## 1. WIP / 進度提交

**徵兆**：使用者明說「先存個進度」「還沒寫完」「等下繼續」。

**模板**：
```
chore: WIP - 簡述未完成的事項
```

**附加處理**：告訴使用者「PR 前需 squash 或 reword 成正式訊息」。

## 2. 純 rename / 純移動

**徵兆**：`git diff --cached --stat` 顯示 `=>` 或 `rename ...`，無實質內容變動。

**模板**：
```
refactor(scope): 將 <舊名> 重命名為 <新名>
```

或：
```
refactor(scope): 將 <檔/模組> 移至 <新位置>
```

## 3. 純刪除

**徵兆**：diff 全是 `-`，無新增檔案。

**模板**（移除無人使用的內容）：
```
chore: 移除已不再使用的 <被刪內容>
```

**模板**（刪除對外功能）：
```
feat!: 移除 <功能名>

BREAKING CHANGE: <對外影響與遷移方式>
```

## 4. 批次依賴升級

**徵兆**：只動 `package.json` / `package-lock.json` / `requirements.txt` / `Cargo.lock` / `go.mod` 等。

**單一套件**：
```
chore: 升級 <package> 至 v<X.Y>
```

**多套件（≥ 3 個）**：
```
chore: 升級多個依賴版本

- <pkg-a> v1.2 → v1.5
- <pkg-b> v2.0 → v2.1
- <pkg-c> v0.9 → v1.0
```

**含 breaking**：
```
chore: 升級 <package> 至 v<X.Y>

- v<X.Y> 移除 <API>，呼叫端改用 <新 API>
- 同步調整 <受影響檔案>
```

## 5. DB Migration / Schema 變動

**徵兆**：路徑含 `migrations/`、新增 `*.sql`、或 ORM 的 schema 檔。

**模板**（必寫 body，必交代是否可逆）：
```
feat(db): 新增 <表/欄位/索引>

- 變更：<具體 schema 變動>
- 可逆：是 / 否（若否，列回滾策略）
```

## 6. 二進位 / 圖片資產

**徵兆**：`git diff --cached` 顯示 `Binary files differ`。

**策略**：改看 `git diff --cached --stat` 與檔名／路徑推測用途。

**模板**：
```
chore(assets): 新增 <用途> 圖庫（<張數>）
```

或：
```
chore(assets): 替換 <用途> 的視覺資產
```

## 7. revert / cherry-pick

**revert**：
```
revert: <被 revert 的原 subject>

This reverts commit <hash>.
理由：<為何 revert>
```

**cherry-pick**：保留原訊息，並在 footer 加：
```
(cherry picked from commit <hash>)
```

## 8. 混合變更（拆 commit 建議）

**徵兆**：同 commit 含 ≥ 2 個無關區域，或 ≥ 2 個無關 type。

**處理流程**：

- 若兩者強相關（feat + 同 feat 的 docs / test）→ 寫一個 commit，body 條列。
- 若無關 → **必列出建議拆法**：

```
建議拆為 N 個 commit：
1. <type(scope)>: <subject>（檔案：A, B）
2. <type(scope)>: <subject>（檔案：C, D）

要照拆嗎？還是合併成一個？（合併會以主 type 為主，body 列出所有改動）
```

- 使用者選「合併」→ 用主 type 寫單一 commit；選「拆」→ 提示使用者重新 stage 後重 call skill。

## 9. 初始 commit / 專案啟動

**徵兆**：repo 沒有任何 commit；或新增的是專案骨架（README、LICENSE、package.json、初始目錄）。

**簡短版**：
```
chore: 初始化專案
```

**完整版**：
```
feat: 初始化 <專案名> 專案骨架

- 建立目錄結構與基本設定
- 加入 README、LICENSE、package.json
```

## 10. CI / Build 設定

**徵兆**：只動 `.github/workflows/`、`Dockerfile`、`Makefile`、`tsconfig.json`、bundler 設定。

**ci pipeline 變更**：
```
ci: <調整內容>
```

**build 工具 / bundler 變更**：
```
build: <調整內容>
```

別搞混：`build` 是 build 流程與 bundler；`ci` 是 pipeline。

## 11. 文件 / 提案 / 簡報

**徵兆**：只動 `*.md`、`*.pdf`、`*.pptx`、`docs/`。

**模板**：
```
docs: <調整內容>
```

提案 / 簡報資料夾常用 scope：`proposal`、`slides`。

```
docs(proposal): 補上 <章節> 內容
```

## 12. 設定檔調整

**徵兆**：只動 `.gitignore`、`.editorconfig`、`.prettierrc`、`tsconfig.json` 等開發環境設定。

**模板**：
```
chore: <調整內容>
```

例：
```
chore: 將 .claude/ 與 .agents/backup/ 加入 .gitignore
```

## 13. 修正既有 commit 的拼字 / 標點

**徵兆**：跨多個檔案的拼字、標點、註解修正，無邏輯變更。

**模板**：
```
fix: 修正多個檔案的拼字與標點錯誤
```

不寫 scope（跨多個區域），不寫 body（內容過於瑣碎）。

## 14. 第一次寫測試 / 補測試覆蓋率

**徵兆**：新增 `*.test.*`、`*.spec.*`、`__tests__/` 內容，無實作變動。

**模板**：
```
test(scope): 補上 <受測對象> 的單元測試
```

注意：若測試是為了讓新功能可測而新增 → type 仍是 `feat`，不是 `test`。
