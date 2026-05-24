# 為什麼自己做：把 Vibe Coding 從免洗變成長期資產
> 與其每次重生成新成品，不如打造一份可重複使用的「自己專屬模板」

## 起心動念

### ✨ 一句話定義這次的嘗試
- 受邀到台大分享 AI 開發工作流，準備過程腦中冒出一個念頭
- 既然在講 AI 寫程式，那講義就不要用傳統 PPT，改用 Vibe Coding 自己做
- 一個晚上做出雛形：電腦／平板／手機都能看，還能匯出 PDF

### 🧩 把成品封裝成 Agent Skill
- 寫一次規則，未來收到新需求只要丟講義初稿，AI 就會自動生成講義網頁
- 不滿意的地方可以直接改 Markdown，不必每次重 prompt
- 這份技能可重複使用，而不是用完就丟

### 🎯 今天會分享的兩件事

[tags]
- [blue] 系統設計思維 — 怎麼把工作流封裝成穩定可用的 Skill
- [orange] 真實踩雷 — 自由度高的背後，會踩到哪些坑
[/tags]

> **Vibe Coding 不該只是免洗餐具**
> 多數人用 AI 寫程式，是「臨時生成、看完即丟」。
>
> 但如果你願意花一點時間設計模板，AI 的產出就能變成可以長期維護、擴增的工具——而不是每次都從零開始。

## 培養多個 AI 工具交替使用的能力

### 🔁 為什麼老是換工具
- 只要某個 AI 被捧上天，過不了多久就會被超越，歷史總是驚人地相似
- 教學重點不是「哪一款最強」，而是用案例示範怎麼解決痛點
- 工具間的差異，沒有想像中那麼大

### 💸 一個血淚提醒

[tags]
- [orange] 不要為了折扣付年費 — 每個時期最好用的 AI 一直在變
- [green] 把錢花在按月訂閱 — 隨時可以跟著生態換手
[/tags]

---

# 安裝 Codex：在終端機把第一版做出來
> Node.js + Codex CLI + 一個空資料夾，就可以開工

## 環境準備

### 💻 編輯器自選
- VSCode、Cursor、Antigravity 都可以
- 這次的示範用 Google 推出的 [Antigravity](https://antigravity.google)，登入 Google 帳號就免費
- 我們不會用編輯器內建 AI，只是借它來檢視程式碼變更

### 📁 給 AI Agent 一個明確工作區
- 開始前先準備一個空資料夾
- 用 `Open Folder` 開啟，或直接把資料夾拖進編輯器
- 這個資料夾就是 AI 的「工作沙盒」

## 安裝 Node.js（透過 NVM）

### 🧪 為什麼用 NVM
- Node.js 有多個版本，不同專案常需要切換
- NVM 可以快速安裝、切換、更新 Node.js 版本
- 比手動下載安裝包輕鬆許多

### 🍎 macOS 一行指令

```prompt [label="Terminal · 安裝 NVM (macOS)"]
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash
```

### 🪟 Windows 走 exe
- 下載 [nvm-windows releases](https://github.com/coreybutler/nvm-windows/releases)
- 選最新版本的 `.exe` 安裝即可

### ✅ 驗證與安裝 Node.js

```prompt [label="Terminal · 驗證 NVM 並安裝 LTS"]
nvm -v
nvm install --lts
node -v
```

## 安裝 Codex

### 📦 全域安裝

```prompt [label="Terminal · 全域安裝 Codex"]
npm i -g @openai/codex
```

```prompt [label="Terminal · 啟動 Codex"]
codex
```

### 🔐 三種登入方式

[tags]
- [green] ChatGPT 帳號 — 最推薦，點開授權網址登入即可
- [blue] Device Code — 適合無瀏覽器的 VM，需先在 ChatGPT 安全性開啟「裝置代碼授權」
- [orange] OpenAI API Key — 按用量計價，記得設用量上限，不然會看帳單想哭
[/tags]

> **第一次啟動會被問是否信任資料夾**
> 選 `Yes` 才能繼續，這是給工作區一個明確授權。
>
> 接著出現的輸入框，就把它當成跟 AI 的對話視窗，操作邏輯跟 ChatGPT 沒太大差別。

## 在終端機生成講義網頁

### 📝 第一段 Prompt：直接點菜

```prompt [label="Prompt · 一句話生成講義網頁"]
請扮演熟悉 AI 主題授課的講師，以「Agent Skills」為主題設計講義
講義用純 html 網頁（CSS/JS 都放裡面）呈現
日系風格的設計感，部分文字與背景可以搭配適當的漸層
開頭會根據內容呈現標題，然後下面有發人深省的標語
會根據不同層級與意圖呈現對應的區塊（ex: 大小標題、陳述、列點、流程）
最後放上社群連結鼓勵大家訂閱（FB/YouTube/Medium）
側邊欄會有對應的目錄，方便跳轉到對應位置
```

### 🧭 這段 Prompt 的三個重點

[flow]
1. 目標明確 — 只生成單純的 HTML 網頁，方便日後部署
2. 風格指定 — 如果沒想法，可以讓 AI 扮演設計師幫你想
3. 架構分層 — 開頭、層級結構、社群連結、側邊欄目錄
[/flow]

### 🎉 結果其實不差
- 等一下下，網頁就生出來了
- 打開資料夾，雙擊 HTML 就能預覽
- 「看起來可以用」的講義網站，這步其實就夠了

> **AI 第一版的甜蜜陷阱**
> 第一版做出來通常很有成就感，可是只要開始想改細節，問題就會浮現。
>
> 與其用「對話 + 改 HTML」的方式跟 AI 拉扯，不如換一個思路：建立穩定的模板。

---

# 從第一版到模板：把控制權拿回來
> 既然生成式 AI 注定有隨機性，那就把可控的部分留給自己

## 痛點：細節調整是真痛

### 🛠️ 改細節的兩條路
- 與 AI 對話調整 — 隨機性太高，還可能動到不該動的地方
- 直接改 HTML — 就算是工程師，改起來都覺得痛苦
- 結論：初版再快、再漂亮，調不動就難以真正用在工作上

### 💡 換個思路：建立模板
- 不要讓 AI 每次都從零生成，而是把「不變的結構」固定下來
- 後續換內容就好，風格與品質自然穩定
- 同樣的概念適用於簡報、文件、網頁等任何重複性工作

## 把 HTML 逆向回 Markdown

### 📝 模板化的 Prompt

```prompt [label="Prompt · 把 HTML 逆向為 Markdown 模板"]
我希望剛剛生成出來的網頁，是可以透過 Markdown 格式編輯內容的
請幫我將網頁結構拆解（列點、步驟、圖片、表格、註解）、分層次（大標、中標、小標），並以常見的 Markdown 語法來設計規則
最後建立一個腳本，我只要編輯 Markdown 文件，網頁的 HTML 就會及時改變
操作方式寫在 README.md 文件中，並建立 package.json
```

### 🧱 這段 Prompt 的四個目標

[tags]
- [blue] 期待目標 — 講義內容透過 Markdown 編輯
- [green] 設計架構 — 拆解區塊與層次，對應到 Markdown 語法
- [purple] 渲染方式 — 寫一支腳本，編輯 Markdown 即時更新 HTML
- [orange] 說明文件 — 操作步驟寫在 README，並建立 package.json
[/tags]

### 🔄 為什麼要這樣設計
- 想要對內容有掌控力，就要把它放在「可編輯、易維護」的格式中
- 最終輸出的 HTML 是結果，不應該是維護介面
- 加上腳本之後，每次只要編輯 Markdown，網頁立刻更新

```prompt [label="Terminal · 啟動開發伺服器"]
npm run dev
```

> **Markdown 是內容、HTML 是輸出**
> 把可變的內容留在容易編輯的層，把輸出交給腳本去生成。
>
> 這也是後面所有設計的根本：先區分「會變的」與「不會變的」，再決定每一塊資訊該放在哪裡。

---

# 把模板封裝成 Agent Skill
> 同樣的工作流，下次只要一句話就能再跑一次

## Skill 的設計目標

### 🎯 為什麼要做成 Skill
- 模板再好用，每次都要手動跑很麻煩
- Agent Skill 可以「依關鍵字觸發」並按照規範跑完整工作流
- 從此每次想做新講義，只要丟主題或草稿就好

### 📝 建立 Skill 的 Prompt

```prompt [label="Prompt · 把工作流封裝成 Skill"]
根據下面需求在「.agents」資料夾建立 Skill：
- 參考模板：根據剛剛腳本設計的規格，建立明確的 Markdown 使用規範
- 觸發時機：使用者提供初步想法想建立網頁時，會觸發這個 Skill
- 執行順序：這個 Skill 會先深入研究輸入的主題，再根據規範設計對應的 Markdown 文件；最後把 Markdown 文件透過腳本生成 html
- 特別提醒：預計會有多個主題，所以觸發時，請先建立新的資料夾，裡面存放新生成的 Markdown 與 HTML 檔案；並且建立獨立的 scripts 與 references 資料夾，確保可單獨執行
```

### 📂 產出的 Skill 結構

[flow]
1. SKILL.md — 名稱、觸發描述、預計執行成果
2. references/ — Markdown 格式的參考文件（給 AI 學語法用）
3. scripts/ — 模板編譯腳本（負責真正執行）
[/flow]

## 用 Codex 外掛驗證 Skill

### 🧩 安裝 Codex IDE 外掛
- 在 Antigravity 左側 `Extensions` 搜尋「chatgpt」
- 找不到時可直接打開 `antigravity:extension/openai.chatgpt`
- 提供「ChatGPT 帳號」與「API Key」兩種模式，登入帳號最方便

### 🖱️ 圖形介面 vs 終端機

[tags]
- [blue] IDE 外掛 — 切換模型、調整思考深度、辨識選取區段，介面友善適合初學者
- [green] 終端機指令 — 速度快、可寫進腳本，需要對 `/` 指令稍有熟悉
[/tags]

### ✅ 驗證 Skill 是否能被觸發

```prompt [label="Prompt · 驗證新 Skill"]
我想要建立深度介紹「ChatGPT 使用技巧」的課程講義網頁。
```

- 觀察 AI 是否正確觸發 `Website Markdown Builder` 這個 Skill
- 確認生成的 Markdown 與 HTML 是否符合預期
- 走到這一步，你就有了一份「屬於自己」的講義生成器

---

# 自由的代價：模板維護要自己扛
> 想要主控權，就得承擔客製化背後的複雜度

## 模板初版常見痛點

### ⚠️ 三個真實踩過的問題

[tags]
- [orange] 側邊欄佔空間 — 講課時希望能自由收合
- [purple] 永遠是淺色 — 關燈授課時觀眾眼睛會累
- [blue] PDF 排版很醜 — 直接用瀏覽器列印，畫面慘不忍睹
[/tags]

### 🧰 模板維護的本質
- 模板的細節要實際使用後才會浮現
- 設計與寫程式可以交給 AI，但「方向與美感」必須自己掌握
- 自由度高 = 你是唯一的維護者

> **市面上已經有很多現成的線上簡報工具**
> 如果只是要「能用」的講義，買現成方案更省力。
>
> 自己設計模板的價值，不是做出更好的方案，而是打造一套「最符合自己工作流」的工具——這需要你願意投入時間打磨。

## 成果展示：實際使用後長出來的功能

### 🌗 收合 / 深淺 / 進度

[tags]
- [blue] 側邊欄可自由收合 — 收合後下方有進度條，掌握授課節奏
- [green] 深淺主題切換 — 配合不同場地的燈光環境
- [purple] 多款配色 — 設定中可挑選主題色
[/tags]

### 📋 講師體感優化
- 單手操作：指令右側有複製按鈕，不用滑鼠選半天
- 中場休息：點「休息一下」可設定倒數計時
- 匯出 PDF：另外設計專屬版面，閱讀體驗才會好
- 簡報模式：右下角一鍵切換，授課切無痛

```prompt [label="Prompt · 給主題自動生成講義"]
扮演一位擅長用實際案例解說的資安專家，設計"生成式 AI 的資訊安全"的講義並生成網頁
完成後直接啟動
```

```prompt [label="Prompt · 給草稿生成講義"]
轉換成網頁，並啟動專案
```

### 🔤 模板專屬語法不必背
- 大部分還是依照常見 Markdown 語法
- 由於模板是自己設計的，可以加入特殊語法
- 你只要把資訊丟給 AI，它會依照規範對應到合適的版型

---

# 部署與系統化：讓模板不再只是 Demo
> 把它放上 GitHub Pages、設好 OG、抽出共用 config，就是一套可分享的工具

## GitHub Pages 部署

### 🚀 三步驟上線

[flow]
1. Init Repository — 在 Source Control 初始化專案，公開的話選 Public
2. Generate Commit + Push Branch — 把內容推上 GitHub
3. Settings → Pages → Deploy from a branch → main — 等 Actions 跑完就有網址
[/flow]

### 🗂️ 資料夾路徑要注意
- 因為網頁檔放在 `example/` 底下，預設網址會 404
- 解決方式：在 GitHub Pages 網址後加上 `example/`
- 其他課程也是一樣，複製對應資料夾路徑就能讀到

## Open Graph：公開分享前的必做

### 🖼️ OG 沒設好，分享預覽會醜
- 缺 `title`、`description`、`image` 時，網址預覽看起來像是壞掉
- 可以到 [opengraph.xyz](https://www.opengraph.xyz/) 貼網址檢查設定
- 雖然是 Vibe Coding 也不能忽略這種專業細節

> **沒技術背景常會忽略「看不到的細節」**
> 像 OG、SEO、無障礙、效能，這些都不是 AI 預設會幫你想到的事。
>
> 模板要長期使用，就要花一點時間補上專業視角，否則品質會慢慢被侵蝕。

## 共用設定：把重複的事一次寫好

### 🌐 兩層 Config 架構

[tags]
- [blue] global.yaml — 講者資訊、社群連結、預設 OG，所有頁面共用
- [green] config.yaml — 各課程獨立覆蓋，例如標題、開場引言
- [purple] assets/ — 共用圖片（avatar、預設 og）統一管理
[/tags]

### 🧠 設計模板的思考方式

[flow]
1. 變動性高的內容 — 放在 Markdown 文件，方便修改
2. 共用的資料 — 抽到 config 集中維護
3. 初始化流程 — 寫成 Agent Skill，AI 幫你跑第一版
[/flow]

> **Vibe Coding 真正脫離 Demo 的轉折點**
> 當你開始用「系統設計」的角度思考，把可變、共用、初始化的責任拆開，
> 模板就會慢慢進入「長期可用」的狀態——而這也是 AI 開始真正幫你省時間的時刻。

---

# 上手練習：把這套工作流跑一遍
> 看 Demo 是看別人的，跑一次才會變成自己的

## 30 分鐘照著做

### ✅ 三個必跑練習

[flow]
1. Fork [agent-skill-lecture-builder](https://github.com/deancourse/agent-skill-lecture-builder) 並 clone 到本機
2. `npm install` 裝好 Puppeteer 等依賴，跑一次 Skill 把資安主題生成出來
3. 拿一份自己的講稿或大綱，再丟一次給 Skill，比較產出差異
[/flow]

### 🧪 兩種觸發方式

[tags]
- [blue] 給主題 — 一句話讓 AI 自動展開大綱與內容
- [green] 給草稿 — 給講稿，內容掌握度更高
[/tags]

---

# 總結

[summary]
- 🚀 **用 Vibe Coding 生第一版** | Codex + 一段 Prompt，30 分鐘做出能跑的講義網頁
- 🧱 **逆向回 Markdown 模板** | 把 HTML 拆解成可編輯的 Markdown 結構，把控制權拿回來
- 🤖 **封裝成 Agent Skill** | 寫一次規範，未來只要丟主題或草稿，AI 自動跑完整工作流
- 🛠️ **自由的代價是維護** | 模板細節得自己長出來，但這也是最符合自己需求的工具
- 🌐 **系統化與部署** | 共用 config、GitHub Pages、OG 設定，讓 Vibe Coding 從 Demo 走向產品
[/summary]
