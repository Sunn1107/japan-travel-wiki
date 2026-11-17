# Wiki 操作日誌 (Operation Log)

本檔案採 Append-only 模式，依時間順序記錄每次旅遊情報攝取（Ingest）、介面重構與知識庫健康檢查（Lint）。

## [2026-11-17] init | 初始化 Japan Travel Wiki 專案
- 建立三層式目錄架構 (`raw/`, `wiki/`, `AGENTS.md`)。

## [2026-11-17] ingest | 攝取大阪梅田飯糰、超商攻略與大阪京都長文攻略 -> 共建立 7 個實體頁
- 讀取 `raw/osaka-food-notes.md`、`raw/japan-konbini-guide.md`、`raw/pacific-osaka-kyoto-guide.md`，建立 `entries/` 下的大阪美食、景點與伴手禮實體頁。

## [2026-11-17] ui-redesign | 升級為日系手帳風三層卡片導覽介面
- 參考手帳卡片設計系統（暖米點點背景、和紙膠帶裝飾條、圓角粗框卡片、待擴充虛線卡），將全站改版為「首頁四大城市卡片 -> 城市內景點/美食/伴手禮三張分類卡片 -> 實體清單卡片 -> 詳情頁」的層級式瀏覽體驗，並新增 `wiki/catalog.json` 驅動卡片渲染。

## [2026-11-17] ingest | 遊日必備52句常用日文會話指南 -> 新增 guides/travel-phrases.md 與頂部「常用日文」導覽頁

## [2026-11-17] ingest | 東京自由行攻略【2026】必去景點、5天4夜行程規劃、交通美食 -> 新增 18 頁（東京 16 個實體頁、來源摘要與城市主頁）
- 讀取 `raw/tokyo-travel-guide-2026.md`，建置東京「必去景點（9）、在地美食（5）、必買伴手禮（4）」共 16 個實體頁與 `cities/tokyo.md` 行程指南，同步更新 `wiki/catalog.json` 與 `index.html` 內嵌離線資料。
