# 認識 Claude Code：不是補全，是會做事的 AI 工程師
> 從 IDE 補全、聊天機器人，到「自己讀 repo、自己改檔、自己跑測試」的 Agent

## 它到底是什麼？

### 🚀 一句話定義
- Anthropic 推出的 CLI／Agent，直接在終端機跟 Claude 對話
- 它會自己讀檔、改檔、跑 Bash、執行測試，不只是聊天
- 把它當成一位「會白話溝通的資深工程師」就對了

### 🆚 跟其他 AI 工具的差別

[tags]
- [purple] ChatGPT：對話為主，要自己貼程式碼回去
- [blue] Copilot：游標補全，看當下這幾行
- [green] Claude Code：整個 repo 都讀得到，自己動手改完跑測試
[/tags]

### 💡 為什麼值得學
- 一句指令橫跨多檔案，不用自己接力
- Plan Mode、Skills、Commands、Hooks 都能客製，把工作流寫進去
- 直接接 GitHub、IDE、MCP，不是孤立的對話視窗

> **AI 的瓶頸不是 AI，是人**
> 過去限制你的是「Prompt 寫不好」，現在限制你的是「工作流沒設計好」。
>
> Claude Code 讓「設計工作流」這件事變便宜——你寫一次規則，AI 永遠照做。

## 它能幫你做什麼

### 🔧 三種典型場景

[flow]
1. 探索陌生 repo — 「幫我看懂這個專案在做什麼」
2. 落地新功能 — 「實作登入頁，含表單驗證與測試」
3. 維運與修補 — 「跑測試，把失敗的修好」
[/flow]

### 📦 開箱即用的能力

[tags]
- [blue] 檔案：Read / Edit / Write / Glob / Grep
- [green] 執行：Bash 指令、跑測試、開 server
- [purple] 上網：WebSearch、WebFetch 直接抓資料
- [orange] 記憶：跨 session 記住你的偏好與專案脈絡
[/tags]

---

# 安裝與第一次啟動：5 分鐘把環境建起來
> 沒程式背景也能跑起來，只要 Node.js 與一組 API Key

## 安裝、登入、開工

### 💻 系統需求
- macOS / Windows / Linux 三平台都支援
- Node.js 18 以上（建議 20）
- 一組 Anthropic API Key 或 Claude.ai Pro/Max 訂閱

### 📥 一行指令安裝

```prompt [label="Terminal · 全域安裝"]
npm install -g @anthropic-ai/claude-code
```

```prompt [label="Terminal · 確認版本"]
claude --version
```

### 🔑 首次登入流程

[flow]
1. 終端機輸入 claude，第一次會跳出登入引導
2. 選擇用 Claude.ai 訂閱 或 Anthropic API Key
3. 登入完成後，CLI 就停在你的專案目錄等指令
[/flow]

> **付費方式怎麼選？**
> Claude.ai 訂閱：固定月費，額度跟著方案走，個人開發最划算。
>
> Anthropic API Key：用多少付多少，企業導入比較好對帳。
>
> 不確定就先用訂閱，跑得起來再決定要不要切 API。

---

# 第一次對話：把它當「會做事」的同事
> 重點不是會不會打字，是知道怎麼把任務交代清楚

## 起手式：cd 到專案後喊一聲 claude

### 📁 第一個對話

```prompt [label="Terminal · 啟動 Claude Code"]
cd ~/your-project
claude
```

- 啟動時 Claude 會掃描專案結構、讀 git status
- 接下來你只要用白話交代要做什麼
- 它會自己決定該讀哪些檔、跑什麼指令

### 💬 三種常見問句

[tags]
- [blue] 探索類：「幫我看懂這個 repo 在做什麼」
- [green] 動手類：「在 src/auth 加一個 JWT 登入 API」
- [orange] 維運類：「跑測試，把失敗的修好」
[/tags]

## 三種對話模式

### 🛠️ 預設模式：邊問邊做
- 直接交代任務，AI 自己判斷該讀哪些檔
- 危險操作（刪檔、改設定）會跳權限提示
- 適合：日常開發、單一檔案修改

### 🧠 Plan Mode：先規劃、不動手

```prompt [label="按 Shift+Tab 切到 Plan Mode"]
我要把這個專案的認證從 session 改成 JWT
先給我完整計畫，包含要改哪些檔、影響哪些 API
```

- 按 `Shift + Tab` 切換進入
- AI 會先列出完整計畫，等你確認再動手
- 適合：大型重構、跨多檔案改動、不熟悉的 codebase

### ⚡ 自動模式：YOLO 全自動
- 啟動加 `--dangerously-skip-permissions`
- 不跳權限確認，一路跑到底
- 適合：sandbox / Docker 容器內、能信任的批次任務

> **新手先別跳到自動模式**
> 自動模式很爽，但拿掉了警示，誤刪、誤覆蓋的代價很高。
>
> 建議前兩週都用預設模式，摸熟 AI 的行為，再決定要不要授權它自動跑。

---

# 教 AI 認識你的專案：CLAUDE.md 與權限
> 一次性的設定，讓接下來每次對話都少講十句話

## CLAUDE.md：給 AI 看的專案說明書

### 📝 為什麼要寫
- 每次新對話 AI 都從零開始，不寫就要重新交代背景
- 寫一次 → 之後所有對話自動讀到
- 內容像 README，但是寫給 AI 看：技術棧、慣例、禁止事項

```prompt [label="自動初始化 CLAUDE.md"]
/init
```

### 📌 該放什麼

[flow]
1. 技術棧 — 用什麼語言、框架、套件
2. 專案慣例 — 命名、格式、測試怎麼跑
3. 注意事項 — 哪些檔案別碰、哪些行為要先確認
[/flow]

> **CLAUDE.md 是「做事」用，不是「規劃」用**
> 它解決的是「下次對話時 AI 還記得我這個專案的規矩嗎？」
>
> 拿來放架構圖、長篇背景反而塞爆 context。它的精神是「給 AI 的行為準則」，越精簡越好。

## 權限：在自由與安全之間拿捏

### 🔐 三種權限策略

[tags]
- [orange] ask — 每次跳確認（最安全，新手起點）
- [green] allow — 自動放行（信任時用）
- [purple] deny — 永遠拒絕（敏感操作）
[/tags]

### ⚙️ 設定檔位置
- `~/.claude/settings.json` — 全機共用
- `<repo>/.claude/settings.json` — 專案共用（會 commit 進 git）
- `<repo>/.claude/settings.local.json` — 個人覆蓋（不進 git）

```prompt [label="自然語言調權限"]
把所有 npm 與 git 指令加進允許清單
```

- 不用自己背 JSON 語法
- Claude Code 會打開對應的 settings.json 幫你改好

---

# 進階能力：Skills、Commands、Sub-agents
> 從「會用」變「會自己組工作流」，差距就在這

## Skills：把專業技能封裝成 SOP

### 🧩 Skill 是什麼
- 一個資料夾 + 一份 `SKILL.md`
- 描述「什麼時候該啟動 + 怎麼做」
- AI 看到關鍵字就自動載入，不用使用者主動下指令

```prompt [label="範例：講白話就觸發 Skill"]
幫我把 audio/demo.m4a 轉成 SRT 字幕
```

### 📂 Skill 的結構

```prompt [label="標準目錄"]
.claude/skills/<skill-name>/
├── SKILL.md            # 觸發描述 + workflow
├── scripts/            # 可執行腳本
└── reference/          # 範例與模板
```

### 💡 寫 Skill 的訣竅
- `description` 開頭直接寫「什麼時候觸發」，AI 才認得出來
- workflow 用編號步驟列清楚，越像 SOP 越穩定
- 真正的執行邏輯放在 scripts 裡，SKILL.md 只負責引導

> **Skill 不是「教 AI 新技能」，是「給 AI 一份 SOP」**
> AI 本來就會跑 ffmpeg、會寫 Python，但每次重新試錯很慢。
>
> Skill 把「成熟做法」寫下來——下次 AI 再遇到同類問題，直接照 SOP 跑，省下試錯成本。

## Commands：把流程串成 pipeline

### ⌨️ Slash Command 是什麼
- 在對話直接打 `/` 開頭 → 強制觸發
- 適合多步驟的固定流程
- 寫在 `.claude/commands/<name>.md`

### 📋 範例：一條指令跑四步

```prompt [label="一條指令完成「音檔→字幕→字卡→文案」"]
/video-srt-card-des

來源：audio/demo.m4a
原稿：audio/origin.md
```

[flow]
1. audio-to-srt — 音檔轉逐字稿
2. srt-enhancer — 修正錯字、補空格
3. srt-card-annotator — 標註字卡時機
4. srt-social-summary — 產 FB / Threads / YouTube 文案
[/flow]

### 🆚 Skill vs Command

[tags]
- [blue] Skill：AI 自動觸發、單一技能單元
- [green] Command：使用者強制觸發、可串多個 Skill
[/tags]

## Sub-agents：開分身各做各的

### 🤖 為什麼需要 Sub-agent
- 主對話的 context 有上限
- 大型搜尋、code review 會吃掉很多 token
- 解法：交給 sub-agent 跑，只把結論回傳，主對話保持乾淨

### 🎯 內建的幾種代理人

[tags]
- [purple] general-purpose — 通用研究員，幫你跨 repo 搜尋
- [blue] Explore — 唯讀搜尋，最便宜的「找東西」助手
- [green] Plan — 規劃實作步驟
- [orange] 自訂 — 你也可以寫一個自己的
[/tags]

---

# 上手練習：跑一條完整工作流
> 不練不會，照著走一次就懂

## 30 分鐘練習路徑

### ✅ 三個必跑練習

[flow]
1. cd 到一個你熟的 repo，輸入 /init 產出 CLAUDE.md
2. 用 Plan Mode 規劃一個新功能，但先不要實作
3. 寫一個小 Skill：把你常重複的指令封裝起來
[/flow]

### 🎁 本工作坊 repo 內建範例

[tags]
- [blue] audio-to-srt — 音檔轉字幕
- [green] course-page-generator — 主題轉課程網頁（就是你現在看到的這頁）
- [purple] proposal-writer — 課綱轉企業內訓提案
[/tags]

```prompt [label="自己也產一頁試試"]
幫我做一份「<你的主題>」的課程網頁
```

> **從複製到自製，差的是脈絡**
> 看別人的 Skill、抄別人的 Command 都能很快得到結果。
>
> 但要做出真正貼合你工作流的工具，得自己經過一次「為什麼這樣設計」的思考——這也是工作坊的核心：教你「怎麼設計」，而不是「複製哪一個」。

## 下一步

### 🗺️ 進階學習地圖

[tags]
- [purple] MCP — 把 Claude Code 接到 Notion、Slack、Jira
- [blue] Hooks — 在事件發生時自動跑腳本
- [green] CI 整合 — 讓 Claude Code 變成 PR 審查機器人
[/tags]

### 📚 官方資源
- `docs.claude.com/claude-code` — 完整功能文件
- `github.com/anthropics/claude-code` — 原始碼與 issue
- CLI 內建 `/help` 與 `/docs` 隨時查

---

# 總結

[summary]
- 🚀 **Claude Code 是會做事的 AI 工程師** | 能直接讀檔、改檔、跑指令、跑測試，不只補全或聊天
- 🧭 **三種模式覆蓋日常** | 預設邊問邊做、Plan Mode 先規劃、Auto Mode 全自動
- 🧩 **客製化能力是真正差距** | CLAUDE.md / Skills / Commands / Sub-agents，把你的工作流寫進去
- 🎯 **先跑一次再優化** | 找一個熟的 repo /init、用 Plan Mode 規劃、寫第一個 Skill
[/summary]
