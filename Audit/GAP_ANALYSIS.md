# Gap Analysis

缺失的契約、未定義的擁有權、以及「藍圖裡有描述、但任何 repo 都沒有實作」的能力。

## 1. 缺失的跨專案契約（Missing Contracts）

| 缺口 | 說明 | 影響 |
|---|---|---|
| Voice Agent ↔ AIECP 介面契約 | 使用者設想語音請求要進入 AIECP，但目前 Voice Agent 只有 Structured Tool Call 本機執行、以及與 AERIS 的 `ORDER.md` 檔案格式。AIECP 完全沒有語音輸入的 schema。 | 沒有這個契約，語音 → AIECP → 工程領域這條核心流程無法接起來。 |
| AIECP ↔ AERIS/MEGIS 派工介面 | AIECP 的 `aecp.task/v1` Command Card schema 是通用的（`inspect-workspace`、`git-status` 等），沒有針對「聲學工程任務」「機構工程任務」的欄位或路由邏輯；AERIS/MEGIS 也沒有暴露可被 AIECP 呼叫的 API。 | 使用者設想的「AIECP 依領域派工到 AERIS 或 MEGIS」目前無技術路徑。 |
| AIECP ↔ SuperBrain 呼叫介面 | 見 CONFLICT_ANALYSIS C-04；兩者都有自己的 Provider/Worker 概念，但沒有互相呼叫的 API 或協議。 | SPARK-AGAVE-3/4 目前無法被 AIECP 當作 Worker/Provider 派工。 |
| SPARK-AGAVE-4 獨立驗證 SPARK-AGAVE-3 的機制 | 使用者問題23「SPARK-AGAVE-4 如何獨立驗證 SPARK-AGAVE-3」——SuperBrain 藍圖只定義兩者角色分工（FAST vs DEEP），沒有具體定義「DEEP 節點驗證 FAST 節點輸出」的協議、觸發條件或介面。 | 這是使用者原始 25 問裡明確提出、但目前任何 repo 都未回答的問題，屬於待設計項目。 |
| Public Portal 的資料投影契約 | 使用者提及未來的 Public Portal（唯讀投影），但本輪任何 repo 都沒有討論「哪些資料可以投影到公開網站」的規則。AERIS 現有的 `aeris.space653000.workers.dev` 是 AERIS-only 的獨立網站，與此無關，且明確不應被觸碰。 | 若未來要做 Public Portal，需要先定義資料分級與唯讀邊界；目前完全空白。 |

## 2. 未定義的擁有權（Undefined Ownership）

| 項目 | 現況 |
|---|---|
| 「誰擁有 GitHub」（使用者問題12） | 各 repo 各自管理自己的分支保護、CI 與合併規則（AERIS 有自己的 tag 保護規則；AIECP 有自己的 governed delivery）。**沒有一個跨專案的「GitHub 擁有權」角色**——每個 repo 目前都是 `Space653000` 本人作為 owner，沒有委派給任何 AI 角色的「GitHub 管理員」定義。 |
| 「誰擁有 CI」（使用者問題13） | 同上，每個 repo 有自己的 GitHub Actions workflow，彼此獨立，沒有跨專案的 CI 政策或共用 runner 規劃。 |
| 「誰擁有本地運算」（使用者問題10） | SuperBrain 藍圖定義了 Laptop/Spark 的角色分工，但這套分工目前只服務 SuperBrain 自己定義的通用任務類型（`zh_summary`、`code_edit` 等），沒有將 AERIS/MEGIS 的工程運算需求（例如 CadQuery 幾何運算、聲學模擬）納入其資源分配模型。 |
| 「誰擁有人類核准」（使用者問題15） | 每個系統都有自己的 Approval Gate（AIECP 的 RED 操作核准、SuperBrain 的 exact-action digest、AERIS 的 constitution GATE），全部指向同一個人類（Stephen），但沒有一個統一的「核准佇列」讓 Stephen 一個地方看到所有系統的待核准事項。 |

## 3. 描述但未實作的能力（Described but Not Implemented Anywhere）

| 能力 | 描述在哪裡 | 實作狀態 |
|---|---|---|
| AERIS 100 個聲學專業能力席位的逐項驗收 | AERIS `docs/architecture/CANONICAL_ROLE_REGISTRY_V1.json` | 角色 ID 已登錄，但 README/HANDOFF 明確承認「100 席位的逐項專業驗證仍待完成」；`NOT_STARTED`。 |
| MEGIS 的聲學（G7）與機器人（G8）垂直切片 | MEGIS README「僅有 schema-only golden case」 | 尚未施工，maturity 上限受安全政策限制（G8）。 |
| AIECP 的 Official Full MCP 端到端 | `Blueprint/00_MASTER_BLUEPRINT.md` Mode B、`.ai/STATUS.md` ENVIRONMENT gate 清單 | 需要真實 ChatGPT Business/Enterprise/Edu workspace，OWNER-EXTERNAL 等級，尚未完成。 |
| SuperBrain 的多機語音調度全流程（Phase A–C） | `.ai/BLUEPRINT.md` §6 | Phase A（ChatGPT Voice）技術上可行但未部署；Phase C（離線語音）完全未開始；且與 Voice Agent 既有的離線語音成果沒有整合（見 DUPLICATION_ANALYSIS）。 |
| AERIS Supervision 的 SuperSystem-wide 發布監督 | 本 SuperSystem repo 使用者敘述 | `SUPERVISION_CONTRACT.md` 目前明確只服務 AERIS 一個專案；擴展為跨專案監督完全沒有起草。 |
| 跨專案的統一 Evidence 詞彙 | 每個 repo 各自有自己的「證據等級」（見 DUPLICATION_ANALYSIS） | 沒有任何一份跨專案文件試圖統一這些詞彙，直到這次 SuperSystem repo 的 Blueprint 10。 |

## 4. 本次盤點自身的缺口（誠實揭露）

- `0_JN1_AERIS_Local-computer-implementation` 的 `aeris_runtime/` 目錄有 100+ 個 Python 檔案，本輪僅盤點檔名/目錄結構，未逐一閱讀每個工程模組的實作內容，因此「Current Progress」欄位在 REPOSITORY_INVENTORY 中大量標記 `UNKNOWN`。
- `0_JN1_AERIS_Supervision` 的 `automation/*.ps1` 腳本內容未逐行審閱。
- AIECP 的 `Blueprint/01`–`20`、`24` 等文件本輪未逐份精讀，僅精讀 `00_MASTER_BLUEPRINT.md`、`.ai/STATUS.md`、`.ai/ACCEPTANCE.md`、`README.md`、`21_AGENT_ROLES_AND_HANDOFF_PROTOCOL.md` 開頭。若這些未讀文件內含與 AERIS/MEGIS/SuperBrain 的整合線索，本次分析可能有遺漏，需下一輪盤點補上。
