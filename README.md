# 雙北貓咖推薦指南 — LLM Wiki 知識庫 🐾☕

本專案實踐了 **Andrej Karpathy 提出的 [LLM Wiki](https://gist.github.com/karpathy/442a6bf555989ca1873e84742aa569f1)** 模式，專門用於持續累積、編譯並維護「雙北（台北市、新北市）貓咪咖啡廳推薦與評測知識庫」，未來可無縫驅動靜態網站（如 Astro / Next.js）生成精美的推薦前台！

---

## 🌟 核心設計理念 (The Core Idea)

一般 RAG（檢索增強生成）每次提問都是「臨時翻書」，回答完就忘記，沒有知識積累。  
本專案採用 **LLM Wiki 持久化編譯**：
- **人類的角色**：負責挑選高品質來源（收集食記、Google 評論、IG 剪報）、引導探索並提出好問題。
- **Agent 的角色**：全權負責繁瑣的維護工作——從原始文本萃取實體資料、更新店面檔案、標記新舊資訊矛盾、交叉維護捷運與標籤索引、生成綜合評比專題。
- **雙螢幕最佳實踐**：一邊開著 **Obsidian** 瀏覽雙向鏈結與知識圖譜（Graph View），另一邊讓 **Antigravity / LLM Agent** 像專職程式設計師一樣維護 Markdown 知識庫。

---

## 📂 資料夾架構說明

```
雙北貓咖/
├── index.html                # 【網站總入口】日雜洗練冷灰風入口首頁，連結各貓咖原型
├── AGENTS.md                 # 核心規範 (Schema)：規範 Agent 行為、YAML 欄位與操作工作流
├── README.md                 # 專案總覽與使用說明
│
├── site/                     # 【網站前端產出層】
│   ├── specs/                # 網頁內容架構與區塊規格書 (.md)
│   └── prototypes/           # 模組化 HTML 網頁原型 (.html)
│
├── raw/                      # 【第一層：原始資料來源 (唯讀，真實之源)】
│   ├── articles/             # 部落格食記、網路報導文章剪輯（Markdown / 文本）
│   ├── reviews/              # Google Maps 評論精華、社群推薦心得
│   └── assets/               # 照片、菜單圖檔、貓咪貼圖（支援 Obsidian Web Clipper）
│
└── wiki/                     # 【第二層：維基知識庫 (由 Agent 維護)】
    ├── index.md              # 總目錄索引（包含店家名冊、地區、捷運線與標籤）
    ├── log.md                # 異動日誌（時間序列記錄 Ingest / Query / Lint 歷程）
    ├── cafes/                # 貓咪咖啡廳實體頁面（包含標準 Frontmatter）
    ├── areas/                # 行政區與捷運路線頁面（如：大同區、捷運板南線）
    ├── tags/                 # 特色標籤（如：適合工作、巨大品種貓、中途領養）
    └── syntheses/            # 綜合主題與推薦指南（如：紅玥草稿、工作貓咖推薦）
```

---

## 🚀 日常工作流指南 (How to Work With Agent)

### 1. 資料攝入 (Ingest) — 丟入新文章或心得
1. 將你看到的貓咖文章、食記或 Google 評論存入 `raw/articles/` 或直接在對話中貼上文字。
2. 對 Agent 說：
   > 「我剛剛在 `raw/articles/` 新增了一篇關於大安區貓咖的剪報，請幫我執行 Ingest，更新相關實體頁面、標籤與索引。」
3. Agent 會自動：
   - 提取店家基本資訊、規則、貓咪數量與親人程度。
   - 建立或更新 `wiki/cafes/<店家名>.md`。
   - 更新對應的 `wiki/areas/`、`wiki/tags/`。
   - 在 `wiki/index.md` 與 `wiki/log.md` 留下紀錄。

### 2. 深度提問與反哺 (Query & Synthesize)
1. 向 Agent 詢問你想探索的貓咖問題，例如：
   > 「我想在捷運淡水信義線上找一間貓咪很多、親人而且有甜點的貓咖，請幫我分析比較。」
2. Agent 會根據現有 Wiki 給出結構化建議。
3. 若討論出極具價值的推薦，Agent 會將精華沉澱至 `wiki/syntheses/`（例如：`wiki/syntheses/捷運紅線療癒貓咖精選.md`），讓知識持續複利成長。

### 3. 健康檢查 (Lint)
每當累積數篇店家後，可對 Agent 說：
> 「請對 wiki 執行一次 Lint 檢查，看看有沒有孤兒頁面、缺漏的資訊（如未確認低消）或需要補充的項目。」

---

## 🌐 未來展望：轉為前端推薦網站

每個貓咖實體頁面均包含標準的 YAML Frontmatter：
```yaml
city: "台北市"
district: "大安區"
mrt_line: "板南線"
mrt_station: "忠孝敦化站"
min_consumption: "低消一杯飲品"
time_limit: "平日無客滿不限時"
work_friendly_score: 4
cat_breeds: ["米克斯", "緬因貓"]
tags: ["適合工作", "中途領養"]
```

這套資料模型可直接與現代靜態網站生成器整合：
- **Astro / Next.js (Content Collections)**：直接讀取 `wiki/cafes/*.md` 生成卡片式前端，具備捷運線、行政區、有無插座、適合工作評分等多重即時篩選。
- **Leaflet / Google Maps API**：依據地址或經緯度直接在地圖上標記出雙北貓咖地圖。
