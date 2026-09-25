# 15 — Public Portal Architecture（未來，本輪不施工）

## 範圍聲明

本章只記錄設計方向，**本輪不實作**任何 Public Portal 程式碼或部署。

## 使用者要求

- Public Portal 是唯讀投影（read-only projection），絕不能暴露本機控制或私有資料。
- 必須與既有的 `aeris.space653000.workers.dev`（AERIS-only 網站）明確區隔——那個網站保持不變，不被觸碰。

## 本輪盤點發現

- 沒有任何來源 repo 包含 Public Portal 的設計文件或程式碼。
- `aeris.space653000.workers.dev` 是 AERIS 專案自己的既有資產；本輪僅在 AERIS 相關文件中看到間接提及（`.nojekyll`、`index.html`、`services.html`、`workspace.html` 等靜態頁面存在於 `0_JN1_AERIS` repo 根目錄，暗示這個網站的原始碼可能就在 AERIS repo 本身），但本輪未深入分析其實際部署方式或內容，避免誤觸碰的風險。
- MEGIS 的 `apps/web`（`/progress` 施工進度中心）明確綁定 `127.0.0.1`，README 明文「不應公開至區域網路或網際網路」——這代表 MEGIS 團隊自己已經意識到「本機開發用的儀表板」與「對外公開的 Portal」是兩件事，這個區分原則值得沿用。

## 建議的設計原則（僅供未來參考，非本輪交付）

1. **資料來源**：Public Portal 應該是唯讀查詢既有 Evidence/Status 資料，不應該有寫入路徑，也不應該直接連線到任何機器的 Control Plane（AIECP、SuperBrain）。
2. **資料分級**：只投影明確標記為 `SHAREABLE` 的資料（見 [06](06_CROSS_PROJECT_DATA_FLOW.md) 的隱私分級討論），`LOCAL_ONLY`/`UNTRUSTED_DATA` 一律不投影。
3. **與 AERIS 既有網站的關係**：Public Portal 若要涵蓋跨專案內容，應該是一個獨立的新網站/新部署，不與 `aeris.space653000.workers.dev` 共用網域、程式碼或部署管線；AERIS 網站繼續保持 AERIS-only。
4. **實作時機**：這是使用者明確標示「未來、本輪不建」的項目，本 SuperSystem repo 只保留設計方向，實際實作留給未來單獨的任務。
