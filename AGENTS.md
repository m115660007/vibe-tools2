# AGENTS.md — 雙北貓咖知識庫維護指南 (LLM Wiki Schema)

本檔案為本專案的核心指令與架構規範（Schema），定義 LLM Agent 如何維護、擴充與查詢「雙北貓咖（台北市/新北市貓咪咖啡廳）推薦知識庫」。

> **核心理念（Karpathy LLM Wiki Pattern）**：  
> 我們不採用「每次查詢才臨時切塊檢索」的一次性 RAG 模式，而是由 Agent 持續建構並維護一個**具備複利效應、彼此鏈結的結構化 Markdown Wiki**。  
> 每次新增原始來源（部落格、Google 評論、IG 剪報），Agent 會萃取資訊並整合進現有實體頁面、標記矛盾、補齊交叉參照，讓知識庫隨著時間與對話越來越豐富。  
> **人類負責挑選資料來源、提出好問題；Agent 負責閱讀、編譯、維護交叉鏈結、建立索引與除錯。**

---

## 1. 四層架構 (Four Layers)

```
雙北貓咖專案/
├── raw/                      # 【第一層：外部原始來源 (Immutable)】
│   ├── articles/             # 網路食記、部落格剪報、官方公告（唯讀真實來源）
│   ├── reviews/              # Google Maps 評論精華、社群心得
│   └── assets/               # 原始照片、菜單圖片（依店名歸檔）
├── wiki/                     # 【第二層：維基知識層 (Persistent Compiled Wiki)】
│   ├── index.md              # 總目錄索引（分類目錄與一覽）
│   ├── log.md                # 時間序列操作日誌（Ingest, Query, Lint）
│   ├── cafes/                # 貓咪咖啡廳實體頁面（每一家店一個獨立檔案）
│   ├── areas/                # 地區與捷運線頁面（大安區、板南線等）
│   ├── tags/                 # 屬性與主題標籤頁面（適合工作、中途領養、巨大品種等）
│   └── syntheses/            # 網頁介紹草稿、綜合評比與主題專題（支援 LaTeX）
├── site/                     # 【第三層：網站產出與原型層 (Web Presentation Layer)】
│   ├── specs/                # 網頁內容架構與區塊規劃規格書 (.md)
│   └── prototypes/           # 模組化 HTML 網頁原型與樣式 (.html, 內嵌 KaTeX)
└── AGENTS.md                 # 【第四層：架構與行為規範 (The Schema)】
```

1. **外部原始來源 (`raw/`)**：唯一真實來源（Source of Truth）。Agent 僅能讀取，絕不可將產出物或原型存入此層。
2. **維基知識層 (`wiki/`)**：由 Agent 維護之持久化 Markdown。其中 `wiki/syntheses/` 存放專案生成的網頁介紹草稿。
3. **網站產出層 (`site/`)**：存放依據草稿轉化之網頁規格架構（`site/specs/`）與模組化 HTML 原型（`site/prototypes/`）。
4. **規範架構 (`AGENTS.md`)**：定義資料結構、Frontmatter、LaTeX 支援規範與維護作業流程。

---

## 2. 貓咖實體頁面資料模型 (Cafe Schema)

每個咖啡廳實體必須建立在 `wiki/cafes/<店家標準名稱>.md`，並且**必須包含統一的 YAML Frontmatter**，以利後續透過 Obsidian Dataview 查詢或轉換為靜態網頁（如 Astro / Next.js）：

```markdown
---
name: "店家名稱"
aliases: ["別名或英文名稱"]
city: "台北市" # 台北市 | 新北市
district: "大安區"
mrt_line: "板南線"
mrt_station: "忠孝敦化站"
walk_minutes: 5 # 捷運站步行約幾分鐘
address: "台北市大安區XX街XX號"
phone: "02-XXXX-XXXX"
business_hours: "週一至週日 12:00-20:00（週二公休）"
google_rating: 4.6
google_reviews_count: 850

# 消費與訂位規則
min_consumption: "低消一杯飲品" # 或門票制、特定金額
time_limit: "平日無限時，假日限 2 小時" # 無限時 | 限時 X 小時
reservation: "可預約" # 預約制 | 僅現場候位 | 可預約
service_fee: "無" # 無 | 10%

# 貓咪資訊
cat_count: 12
cat_breeds: ["米克斯", "緬因貓", "布偶貓"]
rescue_cat: true # 是否為中途/浪浪/領養背景 (true | false)
cat_interaction: "親人溫馴，可撫摸，不可強行抱貓" # 親人/高冷/互動良好/可餵肉泥

# 辦公與休閒設施
work_friendly_score: 4 # 1-5 分 (5最高)
wifi: true # true | false | 未知
power_outlets: "部分座位有插座" # 充足 | 部分座位 | 無
noise_level: "安靜舒適" # 極安靜 | 安靜舒適 | 偏熱鬧

# 規範限制
child_friendly: false # 12歲以下禁止入內 | 友善 | 需家長陪同

# 關聯標籤
tags:
  - "適合工作"
  - "捷運步行5分鐘內"
  - "無限時"
  - "中途領養"
last_updated: "2026-11-05"
sources:
  - "[[raw/articles/2026-taipei-cat-cafes.md]]"
---

# 店家名稱

## 快速摘要
（用 2-3 句話總結這家店的最大特色與最適合的造訪族群，例如：「大安區極少見的安靜無客滿不限時貓咖，擁有超過 10 隻溫馴親人的浪浪，插座充足，極度適合攜帶筆電工作或需要療癒心靈的人。」）

## 貓咪特色與互動體驗
- **店貓陣容**：介紹明星貓咪、品種或救援故事。
- **互動感受**：親人程度、店家提供的互動工具（逗貓棒、肉泥凍乾規定等）。

## 環境與辦公友善度
- **空間與座位**：桌椅高度適中程度、採光、座位寬敞度。
- **辦公配備**：WiFi 穩定度、插座數量、背景音樂音量、安靜程度。

## 餐飲與消費評價
- **招牌推薦**：評價好的甜點、飲品或鹹食。
- **消費心得**：CP 值評語與低消注意事項。

## 造訪須知與店家規範
- 換室內拖鞋、消毒洗手規定。
- 能否攜帶自身寵物、兒童年齡限制、摸貓抱貓規範。

## 關聯實體與反向鏈結
- **所屬區域**：[[wiki/areas/大安區|大安區]]、[[wiki/areas/捷運板南線|捷運板南線]]
- **特色標籤**：[[wiki/tags/適合工作|適合工作]]、[[wiki/tags/中途領養|中途領養]]
- **原始來源**：[[raw/articles/2026-taipei-cat-cafes.md]]
```

---

## 3. 三大核心維護流程 (Core Workflows)

### 流程一：資料攝入 (Ingest)
當使用者在 `raw/` 放入新文件（或在對話中貼入心得網址/文字）並要求 Agent 進行處理時，Agent 必須執行以下步驟：

1. **詳讀原始資料**：讀取 `raw/` 中的新文件，提煉店家基本資料、亮點、爭議點（如服務態度或漲價限時）。
2. **比對現有 Wiki**：
   - 檢查 `wiki/cafes/` 是否已有該店家。
   - 若為**新店家**：建立全新實體頁面，補齊 Frontmatter。
   - 若為**既有店家**：增補新事實。**若新資料與舊資料矛盾（例如：原本不限時改為限時 90 分鐘、低消漲價、搬遷），必須在頁面上明確註明並更新**。
3. **維護關聯實體**：
   - 同步更新對應的行政區或捷運線頁面（`wiki/areas/`），加入該店家雙向連結。
   - 同步更新特色標籤頁面（`wiki/tags/`）。
4. **更新總目錄與歷史日誌**：
   - 在 `wiki/index.md` 加入該店或更新狀態。
   - 在 `wiki/log.md` 尾端追加一筆日誌：
     `## [YYYY-MM-DD] ingest | <來源名稱> -> 異動了哪些頁面`

### 流程二：深度查詢、合成與網頁生成 (Query, Synthesize & Web Gen)
當使用者指定特定貓咖或提出主題時，執行以下四部曲：

1. **草稿生成 (`wiki/syntheses/<主題或店名>-草稿.md`)**：
   - 區分版型：
     - **單店詳細頁**：店名、行政區/捷運、快速指標、店貓陣容、環境設施、手作餐飲、造訪守則。
     - **主題集合頁**：客群意圖導向（如中途領養導向 vs 咖啡廳導向、特定區域/捷運線、辦公友善），包含橫向對比表與精選卡片。
   - **嚴格事實邊界**：正文僅能採用 `wiki/` 內已驗證資料。支援 LaTeX 語法（例如：低消/計時公式 \( C(t) = \min(550, 55 \times \lceil t/10 \rceil) \)）。
   - **外部最新動態整理**：搜尋網路最新評價/動態，尚未入庫者一律列於文末「建議擴充參考文獻」區塊，採用待辦 Checkbox（`- [ ]`）格式以便未來一鍵入庫。

2. **內容架構規格書 (`site/specs/<名稱>-架構.md`)**：
   - 將草稿梳理為網站 Component 區塊階層（Hero, Filter, Badges, CatCards, SpecTable, Map）。

3. **模組化 HTML 原型生成 (`site/prototypes/<名稱>.html`)**：
   - 採用語意化 HTML5 + CSS 變數系統，完全映射 Frontmatter 欄位。
   - 內建 KaTeX CDN，原生支援繁體中文排版與 LaTeX 渲染。

4. **更新日誌與索引**：
   - 記錄於 `wiki/log.md`。

### 流程三：健康度巡檢 (Lint)
定期或應使用者要求，對整個知識庫進行健康檢查：

1. **孤兒頁面檢查**：尋找未被 `wiki/index.md` 或其他頁面引用的檔案。
2. **資訊缺漏標記**：列出尚未確認低消、營業時間或捷運站的店家名單，提示使用者下次可補充。
3. **矛盾與過期檢視**：檢查是否有歇業、裝修暫停營業或規則變更的舊資料。
4. **追加巡檢日誌**：在 `wiki/log.md` 記錄 `## [YYYY-MM-DD] lint | 檢查結果與待修項目`。

---

## 4. 命名與雙向鏈結規範 (Conventions)

- **檔案命名**：
  - 咖啡廳：使用繁體中文標準店名（例如：`wiki/cafes/貓藝家.md`）。若有分店則加上地區，如 `wiki/cafes/疵疵咖啡-公館店.md`。
  - 捷運線與區域：`wiki/areas/捷運板南線.md`、`wiki/areas/板橋區.md`。
  - 標籤分類：`wiki/tags/適合工作.md`、`wiki/tags/巨大品種貓.md`。
- **雙向鏈結（Obsidian 雙括號風格）**：
  - 一律使用 `[[路徑|顯示名稱]]` 或 `[[檔案名稱]]`，例如 `[[wiki/cafes/貓藝家|貓藝家]]` 或 `[[wiki/tags/適合工作|適合工作]]`。
  - 確保所有實體頁面至少連結回一個區域 (`wiki/areas/`) 與至少一個標籤 (`wiki/tags/`)。

---

## 5. 日誌格式標準 (`wiki/log.md`)

日誌必須維持嚴格的格式，方便以腳本或搜尋工具解析：

```markdown
## [2026-11-05] ingest | 雙北10間人氣貓咖評比.md
- 新增店家：[[wiki/cafes/咪可思啾啾堡]]、[[wiki/cafes/貓藝家]]
- 更新店家：[[wiki/cafes/浪浪別哭]]（確認平日不限時）
- 更新標籤：[[wiki/tags/中途領養]]、[[wiki/tags/適合工作]]
- 更新索引：[[wiki/index.md]]
```

---

## 6. 未來網頁版導向 (Roadmap to Website)

本知識庫的結構可無縫橋接到靜態網站產生器（SSG，例如 Astro、Next.js Contentlayer、Hugo 或 VitePress）：
- **Frontmatter 欄位**可直接作為網站的篩選器（Filter）：依捷運線、行政區、有無插座、是否適合工作、是否限時進行多重篩選。
- **Markdown 內容**直接作為個別店家的詳細評測介紹頁。
- **Syntheses 頁面**可直接作為網站的精選文章、部落格指南或專題推薦。
