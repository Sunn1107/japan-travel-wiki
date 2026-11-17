# AGENTS.md - 日本旅遊探索知識庫 (Japan Travel Wiki) 開發與維護規範

## 1. 專案定位與核心理念
本專案是一個基於 Andrej Karpathy「LLM Wiki」模式建構的 **日系手帳風「日本自由行知識庫（Japan Travel Wiki）」**。
全站採用「三層卡片式導覽架構」，告別傳統密密麻麻的純文字列表：
- **第一層（首頁總覽）**：展示 **東京、大阪、名古屋、北海道** 四大城市卡片（附右上角立體紅色大頭圖釘裝飾與分類標籤）。
- **第二層（城市分類頁）**：點進任一城市後，進入該城市的獨立頁面，以三張大卡片劃分 **🏯 必去景點、🍜 在地美食、🎁 必買伴手禮**。
- **第三層（分類清單與實體詳情頁）**：點進「景點 / 美食 / 伴手禮」卡片後，列出該分類下的所有實體小卡，點擊即可閱讀完整圖文攻略（若該分類尚無資料，則顯示虛線外框的「攻略整理中」卡片）。

---

## 2. 視覺風格與設計系統 (Scrapbook / 手帳風 Design System)
所有網頁介面必須嚴格遵守以下視覺規範（定義於 `assets/style.css` 與 `index.html`）：
- **動態雪國背景（`.scenic-backdrop`）**：固定全螢幕 SVG 奶油藍天空、遠景雪山、緩飄雲朵、高空巡航飛機、靜止燕麥色公路與行駛小車，搭配全畫面飄落雪花粒子層（`#snow-layer`）。
- **配色變數規範（`:root`）**：採用日系奶油色系（`--beige-bg: #f4efe6`、`--cream-card: rgba(253, 251, 246, 0.92)`、`--cream-blue-soft: #e6eef5`、`--oatmeal-light: #f1ece2`、`--deep-coffee: #4a3b32`、`--light-coffee: #9b8370`）。
- **手帳卡片規範（`.journal-card`）**：
  - 奶油白半透明毛玻璃底色（`--cream-card`）、細緻暖灰邊框（`1px solid var(--line-hairline)`）、大圓角（`20px`）。
  - 卡片右上角固定配置**立體紅色大頭圖釘（`.card-pushpin`）**裝飾。
  - 卡片內部由上而下依序為：左上角奶油藍膠囊分類標籤（`.pill-badge`）→ 襯線體粗體標題與英文副標 → 細分隔線（`.card-hairline`）→ 簡短摘要 → 底部燕麥色特色小標籤（`.tag-chip`）→ 深咖啡色圓角行動按鈕（`.action-btn`）。
- **待擴充虛線卡（`.dashed-card`）**：
  - 尚未投入資料的城市或分類，使用 `1px dashed var(--light-coffee)` 虛線框搭配 `NEXT CHAPTER` / `COMING SOON` 提示。

---

## 3. 三層式目錄結構與命名規範
所有檔案命名一律採用 **全小寫英文 + 連字號 (`-`)**：

1. **`raw/`（原始情報層 — 嚴格唯讀）**
   - 存放未經修改的部落格食記、旅行社攻略、交通套票說明。
   - **鐵則**：AI 僅能讀取 `raw/` 內的檔案，**絕對禁止**修改、刪除或重新命名 `raw/` 內的任何原始檔案。
2. **`wiki/`（編譯知識庫層 — AI 全權維護）**
   - `wiki/index.md`：全站結構化總目錄（供 AI 檢索與維護使用）。
   - `wiki/log.md`：全站時間軸日誌，採 Append-only（僅附加）模式記錄每次 `ingest`、`query`、`lint`。
   - `wiki/catalog.json`：**前端卡片渲染索引檔**！記錄四大城市、三大分類（`spots` / `food` / `souvenirs`）與所有實體卡片的標題、標籤、摘要與對應 `.md` 路徑，讓前端能動態渲染手帳卡片。
   - `wiki/cities/`：四大城市主頁（`tokyo.md`、`osaka.md`、`nagoya.md`、`hokkaido.md`）。
   - `wiki/entries/`：單一景點、美食或伴手禮實體詳情頁（`.md`）。
   - `wiki/guides/`：跨城市實用攻略頁（如 `convenience-stores.md`、`travel-phrases.md`）。
   - `wiki/sources/`：單篇原始資料的編譯摘要頁。
3. **網頁展示層（`index.html` 與 `assets/style.css`）**
   - 純靜態前端架構，支援 Hash 路由（`#/` 首頁、`#/city/osaka` 城市頁、`#/city/osaka/food` 分類頁、`#/page/entries/onigiri-gorichan.md` 詳情頁），可直接部署於 GitHub Pages。

---

## 4. Markdown 實體頁面格式規範 (`wiki/entries/*.md`)
每個實體詳情頁頂部必須包含 YAML Frontmatter，並包含 5 個標準區塊：
```yaml
---
title: 頁面標題
city: tokyo | osaka | nagoya | hokkaido
category: spots | food | souvenirs
tags: [標籤1, 標籤2, 標籤3]
sources_count: 1
last_updated: YYYY-MM-DD
---
```
內文強制包含：
1. **基本情報表（Markdown Table）**：所屬城市商圈、分類定位、建議停留時間、適合對象。
2. **特色與推薦重點**：條列招牌美食、必看亮點或必買品項（嚴禁捏造未出現在 `raw/` 的資訊，缺漏處標註 `[待實地確認]`）。
3. **排隊與順遊動線攻略**：最佳造訪時段與同城市順遊建議（使用相對路徑連結）。
4. **關聯頁面 (Related Pages)**：連結至所屬城市頁與原始來源摘要頁。
5. **外部參考連結 (External Links)**：提供可點擊開啟的 Google Maps 搜尋連結。

---

## 5. 三大標準操作流程 (Core Workflows)

### 操作一：Ingest（新旅遊情報攝取與知識融合）
當使用者在 `raw/` 放入新筆記並要求「執行 Ingest」時，AI 必須依序完成以下 5 個步驟：
1. **深度閱讀**：完整閱讀 `raw/` 中的目標文件，提取所屬城市（`tokyo` / `osaka` / `nagoya` / `hokkaido`）與分類（`spots` / `food` / `souvenirs`）。
2. **建立來源摘要頁**：在 `wiki/sources/[slug].md` 建立編譯摘要。
3. **建立或更新實體與城市頁**：
   - 依第 4 節規範建立或更新 `wiki/entries/[slug].md`。
   - 同步更新對應的 `wiki/cities/[city].md`。
4. **同步更新前端索引 (`wiki/catalog.json`) 與總目錄 (`wiki/index.md`)**：
   - 在 `wiki/catalog.json` 對應城市的分類陣列中加入新卡片物件（含 `id`、`title`、`badge`、`summary`、`tags`、`mdPath`），確保首頁與城市分類頁立刻出現新的手帳卡片。
5. **寫入操作日誌 (`wiki/log.md`)**：在 `log.md` 最末端附加紀錄：
   `## [YYYY-MM-DD] ingest | [情報標題] -> 新增 X 頁、更新 Y 頁`

### 操作二：Query（跨頁面行程規劃與知識沉澱）
當使用者針對知識庫提問時，先讀取 `wiki/index.md` 鎖定相關頁面並給出附帶引用的回答；若產出有價值的多日行程或比較表，主動詢問是否歸檔至 `wiki/guides/`。

### 操作三：Lint（知識庫健康度檢查）
定期檢查 `wiki/catalog.json` 與 `wiki/entries/` 是否完全對應、有無孤兒頁面或斷裂連結。
