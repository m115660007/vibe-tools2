# 雙北貓咖知識庫異動日誌 (Activity Log)

本檔案記錄知識庫的所有流水歷程（Ingest, Query, Lint）。每一筆紀錄以 `## [YYYY-MM-DD] <操作類型> | <事件描述>` 作為開頭，以確保可被工具自動檢索與回溯。

---

## [2026-11-05] polish | 網站文案去工程化修飾
- 刪除首頁 `index.html` 中的「以 Karpathy LLM Wiki 結構化編譯，」字眼，讓前台文案回歸純粹的讀者與消費者視角。
- 將前台的所有「專案」、「報告」替換為「店家」與「店家介紹」（如「查看專案」改為「查看店家」）。

---

## [2026-11-05] web-complete | 全站 HTML 原型與首頁動態搜尋篩選完善
- 完成全量 HTML 單店原型：共 11 間活躍實體貓咖（紅玥、FUFU、小貓花園、Nekocoffee貓珈、朵朵嚐嚐、貓食光、肉球森林、擼LALA松菸、光嶼、NUKU NUKU、転運棧、咪可思啾啾堡）全數上線於 `site/prototypes/`。
- 完成主題特案原型：[site/prototypes/雙北工作與安靜放鬆貓咖精選.html](file:///c:/Users/user/Desktop/雙北貓咖/site/prototypes/雙北工作與安靜放鬆貓咖精選.html)（含工作指數量化模型與橫向對比表）。
- 升級首頁 [index.html](file:///c:/Users/user/Desktop/雙北貓咖/index.html)：
  - 新增即時前端 JavaScript 關鍵字即時搜尋與標籤快篩（全部/巨大品種貓/中途/捷運5分/工作/餐點）。
  - 新增主題特企橫幅引導。
  - 將 12 間卡片全數掛載對應之真實 HTML 原型連結。
  - 整合賓士貓貼圖全家族（包含 `image3.png` 瞇瞇眼與 `image7.png` 冬日毛衣助手）。

---

## [2026-11-05] portal | 建立網站首頁 index.html 與樣式微調
- 建立專案根目錄首頁：[index.html](file:///c:/Users/user/Desktop/雙北貓咖/index.html)（同款日雜洗練冷灰風格，展示整體統計指標、主題快篩、實體卡片目錄，並直連已發布之各貓咖原型）。
- 微調單店原型樣式：修改 [site/prototypes/紅玥緬因貓藝廊.html](file:///c:/Users/user/Desktop/雙北貓咖/site/prototypes/紅玥緬因貓藝廊.html) 中的 `image5.png`，由最右側絕對定位改為互動區頂部置中，避免遮擋文字，並支援點擊導航回首頁。

---

## [2026-11-05] web-gen | 紅玥緬因貓藝廊 網頁草稿與日雜冷灰風原型產出
- 產生介紹草稿：[[wiki/syntheses/紅玥緬因貓藝廊-草稿]]（含 LaTeX 費率公式與外部待入庫參考文獻）。
- 產生架構規格書：`site/specs/紅玥緬因貓藝廊-架構.md`。
- 產生 HTML 原型：`site/prototypes/紅玥緬因貓藝廊.html`（日雜編輯・洗練冷灰風，整合 KaTeX 與賓士貓貼圖）。

---

## [2026-11-05] update | 貓藝家搬遷新址與暫停營業資訊同步
- 更新實體頁面：[[wiki/cafes/貓藝家]]（註明遷至台北市中山區新生北路二段127巷7-2號，裝修暫停營業中）。
- 撤除活躍推薦：從 [[wiki/tags/適合工作]]、[[wiki/tags/中途領養]] 及專題 [[wiki/syntheses/雙北適合筆電工作與安靜放鬆的貓咖推薦]] 中移出活躍名單。
- 更新捷運與區域：[[wiki/areas/捷運中和新蘆線]]、[[wiki/areas/永和區]]。
- 更新總目錄索引：[[wiki/index.md]]。

---

## [2026-11-05] ingest | https://rosy-arts.com/life/cat-cafe/
- 原始檔案：已抓取並轉譯儲存至 [[raw/articles/rosy-arts-cat-cafe.html]] 與 [[raw/articles/rosy-arts-cat-cafe.md]]。
- 新增實體頁面（共 11 間）：
  - [[wiki/cafes/小貓花園]]（士林區 / 金氏紀錄最老貓咖）
  - [[wiki/cafes/紅玥緬因貓藝廊]]（大同赤峰街 / 緬因貓純白藝廊風）
  - [[wiki/cafes/肉球森林]]（中正東門 / 貓咪、狐獴、巨嘴鳥）
  - [[wiki/cafes/FUFU_Cat_Cafe]]（萬華西門 / 20多隻品種貓、計時暢飲）
  - [[wiki/cafes/NUKU_NUKU_Cat_Cafe]]（萬華西門 / 2025新址三層樓空間、中途貓）
  - [[wiki/cafes/Nekocoffee_貓珈]]（中正北車 / 複合放鬆休閒、按摩椅與床鋪）
  - [[wiki/cafes/擼LALA_Sweets_松菸店]]（信義松菸 / 挪威森林貓、貓掌馬卡龍）
  - [[wiki/cafes/朵朵嚐嚐貓咪中途咖啡廳]]（松山 / 25隻親人米克斯、泰式打拋豬與牛丼定食）
  - [[wiki/cafes/転運棧貓咪中途咖啡廳]]（大同天水路 / 20多隻中途、多樓層、完整燉飯正餐）
  - [[wiki/cafes/貓食光]]（大安公館 / 貓砂盆巧克力蛋糕、中途愛心領養）
  - [[wiki/cafes/光嶼]]（大同圓山 / 十年街貓救援主理人、極度安靜老派庇護所）
- 新增/更新區域頁面：
  - 捷運線：[[wiki/areas/捷運淡水信義線]]、[[wiki/areas/捷運板南線]]、[[wiki/areas/捷運松山新店線]]、[[wiki/areas/捷運文湖線]]
  - 行政區：[[wiki/areas/大同區]]、[[wiki/areas/中正區]]、[[wiki/areas/萬華區]]、[[wiki/areas/信義區]]、[[wiki/areas/松山區]]、[[wiki/areas/士林區]]、[[wiki/areas/大安區]]
- 新增/更新特色標籤：
  - [[wiki/tags/巨大品種貓]]、[[wiki/tags/計時制貓咖]]、[[wiki/tags/捷運步行5分鐘內]]、[[wiki/tags/特色餐點]]、[[wiki/tags/中途領養]]
- 更新總目錄索引：[[wiki/index.md]]

---

## [2026-11-05] init | 專案初始化與架構建立
- 依據 Karpathy LLM Wiki 模式建立 `AGENTS.md`。
- 建立資料夾架構：`raw/articles/`, `raw/reviews/`, `raw/assets/`, `wiki/cafes/`, `wiki/areas/`, `wiki/tags/`, `wiki/syntheses/`。
- 建立初始索引 [[wiki/index.md]] 與異動日誌 [[wiki/log.md]]。
- 建立示範實體頁面：[[wiki/cafes/貓藝家]]、[[wiki/cafes/咪可思啾啾堡]]。
- 建立示範標籤與區域：[[wiki/tags/適合工作]]、[[wiki/tags/中途領養]]、[[wiki/tags/無限時]]、[[wiki/areas/永和區]]。
- 建立示範專題：[[wiki/syntheses/雙北適合筆電工作與安靜放鬆的貓咖推薦]]。
