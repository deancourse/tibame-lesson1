---
name: commit-message-writer
description: 根據 staged 變更（git diff --cached）產生符合 Conventional Commits 規範的中文 commit 訊息，並詢問是否直接 commit。當使用者說「幫我寫 commit message」「設計 commit 訊息」「產生 commit 描述」「commit 變更」「寫一個 commit」「幫我 commit」「整理 commit 訊息」「help me write a commit message」「generate commit message」時使用。Skill 流程：preflight 檢查（分支安全、secrets 偵測、stage 完整性）→ 從近期 commit 學風格 → 套用情境模板 → 提出 2–3 個候選方案 → 確認後 commit → 輸出摘要。只分析已 git add 的內容，不會自動 stage；訊息採用 `type(scope): 中文描述` 格式。完整規範見同目錄 CONVENTIONS.md，情境模板見 SCENARIOS.md。
version: 0.2.0
---

# Commit Message Writer

把 `git diff --cached` 濃縮成一則合規的 commit 訊息，使用者確認後 `git commit`。

## 流程概覽

```
preflight 檢查（分支、secrets、stage 完整性）
   ↓
取得 diff + 近期 commit 風格
   ↓
判讀 type / scope / 是否拆 commit
   ↓
對照 SCENARIOS.md 套用情境模板（若命中）
   ↓
產生 2–3 個候選訊息（附判斷理由）
   ↓
使用者選定或調整
   ↓
git commit（HEREDOC）
   ↓
摘要：hash、檔案數、hook 狀態
```

## 用途

寫 commit message 是 daily ritual：要決定 type、決定 scope、決定要不要寫 body、還要顧及中文用詞統一。這個 skill 把這些判斷集中起來，讓使用者只需要 stage、呼叫 skill、確認，就完成 commit。

## 何時使用

- 使用者已 `git add` 並準備 commit。
- 使用者明說要產生 / 設計 / 寫一則 commit message。
- 使用者貼 diff 並問「這個怎麼 commit 比較好」。

**不要主動觸發**：

- 使用者只想 review diff、討論變更內容、看程式碼。
- 使用者要寫 PR 描述（PR 描述不是 commit message，應走 `gh pr create` 流程）。

## 工作流程

### 1. Preflight 檢查（必跑）

並行執行：

- **分支安全**：`git symbolic-ref --short HEAD`
  - 若為 `main` / `master` / `release/*` → 警告「正在主幹分支直接 commit，確定要繼續嗎？」
  - 若為 detached HEAD（指令回傳非零）→ 警告「目前在 detached HEAD，commit 不會留在任何分支上」
- **Secrets 偵測**：對 `git diff --cached` 內容做模式檢查
  - 檔名 pattern：`.env`、`.env.*`、`*.pem`、`*.key`、`id_rsa*`、`credentials.json`、`*.p12`
  - 字串 pattern：
    - `-----BEGIN [A-Z ]+-----`（PEM、SSH key、PGP）
    - `AKIA[0-9A-Z]{16}`（AWS Access Key）
    - `sk-[A-Za-z0-9]{20,}`（OpenAI/Anthropic 風格 token）
    - `(?i)(api[_-]?key|password|secret|token)\s*[:=]\s*['"][A-Za-z0-9+/]{16,}`
  - 命中 → **停下**，告訴使用者命中哪個 pattern、在哪個檔案，請使用者確認是否要 `git restore --staged` 並清除後再來。
- **Stage 完整性**：`git status --short`
  - 若有同檔的 unstaged 變更（前綴 `MM` / `AM`）→ 提醒一次：「`<檔>` 有未 staged 的修改，是否也要 add？」由使用者決定，**不自動 add**。

`git diff --cached` 若為空 → 列出 `git status` 結果問使用者要先 add 哪些，不自作主張。

### 2. 取得變更與風格

並行：

- `git diff --cached --stat` — 改動量級。
- `git diff --cached` — 實際內容。**若超過 1000 行**，改抓 `--stat` 加上每個檔的前 50 行 hunks，避免 context 爆掉。
- `git log -5 --pretty=format:"%s"` — 近期 subject。從中偵測：
  - **語言**：是否全 ASCII（→ 採英文）vs 含 CJK（→ 採中文）。
  - **scope 使用率**：含 `(scope)` 比例 > 60% → 鼓勵這次也寫 scope。
  - **scope 命名風格**：kebab / snake / 駝峰 → 套用相同風格。

### 3. 判讀 type / scope / 拆 commit

依 `CONVENTIONS.md` §2 的 type 表決定 type；依 §3 決定 scope。

**判斷常見陷阱**：

- **混合變更**：feat + 同 feat 的 docs / test → 主 type 為 `feat`，docs 放 body 條列；無關的 feat + 無關 fix → **建議拆 commit**（見 §4 情境模板 #8）。
- **改測試碰實作**：為了讓新功能可測而新增測試，type 仍是 `feat`；只是補測試覆蓋率，才是 `test`。
- **重命名檔案**：算 `refactor`，不是 `chore`。
- **新增整個 Skill**：算 `feat`（對外行為有增加）。

### 4. 套用情境模板

對照 `SCENARIOS.md` 的 14 種情境清單；命中任一就直接套用該情境的訊息模板，未命中走預設規則。常見情境：

- WIP / 純 rename / 純刪除 / 批次依賴升級 / DB migration / 二進位資產 / revert / 混合變更 / 初始 commit / CI / 文件 / 設定檔 / 拼字修正 / 測試。

### 5. 產生候選訊息

**一次給 2–3 個候選**，每個附「判斷理由」一行。呈現格式：

```
─── 候選 A（精簡版）────────────────
feat(audio-to-srt): 新增 GPU 自動偵測

判斷：使用者可見的新行為，變更集中於 audio-to-srt。

─── 候選 B（含 body）──────────────
feat(audio-to-srt): 新增 GPU 自動偵測

- 偵測 CUDA / MPS / CPU 並切換 whisper 後端
- 偵測失敗時 fallback 到 CPU

判斷：偵測有層次，body 條列方便讀者掌握。

─── 候選 C（拆 commit 建議）────────
建議拆為兩個 commit：
1. feat(audio-to-srt): 新增 GPU 自動偵測
2. docs(audio-to-srt): 補上 GPU 設定說明
```

呈現後問：「要選哪一個？或想自己改？」

候選之間應有差異化（精簡 vs 詳細 vs 拆分），不要三個都長得像。

### 6. 確認並 commit

使用者選定後：

```bash
git commit -m "$(cat <<'EOF'
<完整訊息>
EOF
)"
```

只有 subject 也用 HEREDOC（保持一致）。

**禁止**：

- `--no-verify`（pre-commit hook 失敗 → 告知使用者，不要繞過）
- `--amend`（除非使用者明說要 amend）
- 任何修改 `git config` 的指令
- `git add -A` / `git add .` 在本流程中（會誤帶 secrets）

### 7. 後置摘要

commit 完成後，跑 `git log -1 --stat --format=%h%n%s` 取資訊，輸出：

```
✓ committed 7a3f2c1
  3 files changed, 42 insertions(+), 12 deletions(-)
  pre-commit hook: passed
```

**Hook 失敗時**：清楚告訴使用者錯誤訊息、建議修法，**不要重試或 amend**。修好後讓使用者重新 stage 並重 call skill。

## 與其他 Skill / 系統 prompt 的關係

- 系統 prompt 的 "Committing changes with git" 章節之安全規範（不 amend、不 force push、HEREDOC、不加 `--no-verify`）**全部優先適用**。本 skill 補上中文 + Conventional Commits + 自動判讀的具體邏輯。
- 不取代 PR 描述產生（那是 `gh pr create` 的事）。
- 不與 `code-review` / `verify` 衝突；若使用者先 review 才想 commit，先讓 review 跑完。

## 規範與情境檔

- `CONVENTIONS.md` — 完整硬規範（type 表、scope 規則、字數、動詞詞彙、breaking change、反規範清單）。
- `SCENARIOS.md` — 14 種情境模板。

任何「格式 / 用字 / 動詞」判斷一律以 `CONVENTIONS.md` 為準。本檔只負責「怎麼跑流程」。

## 如何驗證 Skill

1. **觸發測試**：用各種說法試（「幫我寫 commit message」「整理 commit」「commit 變更」「help me write a commit message」），看 Skill 是否被正確選用，且不在 review-only 場景被誤觸發。
2. **規則對照**：每次產出的訊息逐條對照 `CONVENTIONS.md` §1–§8，特別檢查：subject 字數、動詞、有無句尾標點、scope 是否 kebab-case。
3. **情境覆蓋**：用 14 種情境各構造一個 staged diff，驗證 Skill 是否正確套用模板。
4. **Preflight 真實性**：刻意把 `.env`、含 `sk-xxxxx...` 字串、或 main 分支 commit 丟進 staged 區，確認 Skill 會擋住或警告。
5. **正則驗證**：對 `git log -20 --pretty=format:"%s"` 套用 §1 正則，未通過的 subject 就是不合規。
