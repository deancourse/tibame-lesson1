# Commit Message Conventions

本檔案是 commit-message-writer Skill 的硬規範表。SKILL.md 描述「怎麼判斷」；本檔描述「結果長怎樣才合格」。任何格式 / 用字 / 結構爭議一律以本檔為準。

## 1. Subject 結構

```
type(scope): 中文描述
└┬┘└──┬──┘  └────┬────┘
 │    │          └─ 動詞開頭、現在式、不加句尾標點
 │    └─ 選填，kebab-case，≤ 20 字
 └─ 必填，小寫
```

驗證正則：`^[a-z]+(\([a-z0-9-]+\))?!?: .+$`

## 2. Type（必填）

只用下表，不自創：

| type | 用途 |
|------|------|
| `feat` | 新增使用者感知得到的功能、Skill、頁面、CLI flag |
| `fix` | 修正壞掉的行為（bug、錯誤訊息、邏輯錯誤） |
| `perf` | 為效能而改（更快、更省記憶體） |
| `refactor` | 改寫內部結構，對外行為不變（重命名、抽函式、調目錄） |
| `test` | 新增或修改測試 |
| `docs` | 只改文件、README、註解、提案內容 |
| `style` | 只改排版、格式、空白、引號（**不改邏輯**） |
| `build` | 改 build 流程、依賴、bundler 設定 |
| `ci` | 改 CI/CD pipeline、GitHub Actions |
| `chore` | 雜事：升級依賴、清理檔案、初始化、調設定 |
| `revert` | 撤回某個 commit |

**規則**：

- type 一律小寫。
- 不可自創新 type（`hotfix`、`wip`、`update` 等不在表上的一律不用）。
- 進度提交（WIP）統一寫 `chore: WIP - 描述`，並提醒「PR 前要 squash 或 reword」。

## 3. Scope（選填）

- 一律小寫 **kebab-case**：`audio-to-srt`、`pdf-builder`、`config`。
- 長度 ≤ 20 字元（含連字號）。
- 用單數：`commit-message-writer`，不是 `commit-message-writers`。
- 來源優先級：
  1. 全部變更在單一 skill → 用 skill 名。
  2. 全部變更在單一頂層目錄 → 用該目錄名。
  3. 全部變更在單一檔案類別 → 用類別名（例 `config`、`deps`、`assets`）。
  4. 跨多個無關區域 → **不寫 scope**。不要湊 `misc` / `multi` / `general` / `all`。

## 4. Subject 文字

### 語言

- 預設中文。
- 若近 5 個 commit 全為英文（無 CJK 字元）→ 改用英文，沿用近期風格。

### 字數

- 中文 ≤ 30 字（含 `type(scope): ` 前綴）。
- 英文 ≤ 50 字（含 `type(scope): ` 前綴）。
- 中英混用以中文上限為準。

### 動詞詞彙表（中文）

第一個字必須是動詞，**同義詞統一**：

| 偏好 | 不要用 |
|------|--------|
| 新增 | 增加、加入、添加、創建 |
| 修正 | 修補、修復、改好、處理 |
| 移除 | 刪除、拿掉、去除 |
| 重構 | 重寫、改寫（後者保留給結構性大改） |
| 調整 | 改、變更、修改 |
| 優化 | 改善、改進、提升 |
| 補上 | 補充、加上（用於遺漏修補） |
| 升級 | 更新版本（用於依賴升級） |
| 撤回 | 還原、回滾、revert |
| 重命名 | 改名、rename |
| 抽出 | 拆出、抽取、extract |

### 動詞詞彙表（英文）

`add` / `fix` / `remove` / `refactor` / `tweak` / `optimize` / `bump` / `revert` / `rename` / `extract`。

### 禁則

- 帶語氣的代名詞與時態：「我」「已」「了」「的」開頭（例：「我新增了…」、「已修正…」）。
- 句尾標點：句號、句點、驚嘆號、問號。
- Emoji（含 gitmoji）。
- 在 subject 寫原因（原因放 body）。
- 一個 subject 塞多件事（用逗號或頓號連接多個動作 → 拆 commit 或寫 body）。

## 5. Body（選填）

### 何時加

1. 改動橫跨多個面向（需列點交代「同時改了 A、B、C」）。
2. Breaking change 或非顯而易見的副作用。
3. 變更動機在 diff 裡看不出來。
4. 依賴升級且有行為變化。

### 格式

- subject 與 body 中間空 **一** 行。
- 每行 ≤ 72 字元（中文一字算 2）。
- 用 `-` 開頭條列，一行一個事實，不換行延續。
- 條列各點不重複 subject 已說過的內容。
- body 內部段落間也用空行分隔。

## 6. Footer（選填）

順序固定，由上到下：

```
BREAKING CHANGE: <說明影響範圍與遷移方式>

Refs #123
Closes #456

Co-authored-by: Name <name@example.com>
```

- `Refs` 用於相關但未關閉；`Closes` 用於 commit 一進 main 就關閉的 issue。
- Co-authored-by 一人一行，email 用 `<>` 包住。

## 7. Breaking Change 標記（擇一，不混用）

| 場景 | 標記方式 |
|------|----------|
| 影響輕、subject 就讀得懂 | `type(scope)!: 描述` |
| 影響重、需說明遷移 | `type(scope)!: 描述` **加** `BREAKING CHANGE:` footer 寫完整說明 |
| 無 breaking change | 兩者都不寫 |

不要只寫 `BREAKING CHANGE:` footer 而 subject 沒有 `!`——`git log --oneline` 會漏看。

## 8. 反規範清單

- `feat: 新增了功能` — 「了」、subject 空泛
- `修正 bug` — 缺 type
- `feat(misc): 一些更新` — 湊 scope、subject 空泛
- `Feat(API): ...` — type / scope 大寫
- `feat: 新增功能。` — 句尾句號
- `feat: 🎉 新增功能` — emoji
- `feat(audio-to-srt): 新增轉換、修正對齊、更新 README` — 三件事塞同 subject
- `fix: 修正 SRT 字幕在某些情況下會出現時間軸不對齊的問題，這個問題的根本原因…` — subject 寫成摘要
- `chore: update deps`（中文專案突然冒英文）— 不沿用近期語言風格
- `feat(skill): ...`（整個 repo 都是 skill）— scope 太空泛
