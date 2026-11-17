# 🗾 Japan Travel Wiki（日系手帳風・日本旅遊探索知識庫）

基於 Andrej Karpathy「LLM Wiki」理念打造，採用 **三層卡片式手帳風介面**：
1. **首頁**：🗼 東京、🐙 大阪、🏯 名古屋、❄️ 北海道 四大城市卡片入口。
2. **城市頁**：點擊城市進入下一頁，以三張主題卡片區分 **🏯 景點、🍜 美食、🎁 伴手禮**。
3. **詳情頁**：點擊任一卡片查看完整圖文攻略、基本情報表與 Google Maps 連結。

## 本機預覽方式
在專案根目錄開啟終端機執行：
```bash
python -m http.server 8000
```
接著開啟瀏覽器前往 `http://localhost:8000` 即可體驗完整互動切換效果（或直接使用 VS Code Live Server 開啟 `index.html`）。
