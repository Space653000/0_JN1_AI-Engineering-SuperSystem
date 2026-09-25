# 13 — Security, Privacy and Trust

## 各專案的風險分級對照（詞彙不同，精神一致）

| 專案 | 風險分級詞彙 | 高風險動作需要什麼 |
|---|---|---|
| AIECP | GREEN / YELLOW / RED；READ/TEST/WRITE/INSTALL/COMMIT/PUSH/PR/MERGE/DELETE/CREDENTIAL/SYSTEM | RED 需明確人工核准；agent 無法自我核准權限 |
| SuperBrain | GREEN（讀取/測試/健康檢查）/ YELLOW（寫入工作區/commit/安裝依賴）/ RED（刪除/push/發布/對外傳送/改防火牆/改帳號/讓 Spark 連網） | RED 需 exact-action digest 核准，且動作一變核准即失效 |
| Voice Agent | L0～L3 | 語音+文字雙確認 + Emergency Stop |
| AERIS | constitution GATE-01～08 | Human Gate |
| MEGIS | Artifact classification + maturity（`PROTOTYPE` 為自動化上限） | `ENGINEERING_REVIEWED`/`RELEASED` 需合格工程師與法規流程 |

**跨專案對照表（本 repo 整理，非強制統一）**：

| 抽象風險等級 | AIECP | SuperBrain | Voice Agent |
|---|---|---|---|
| 唯讀/低風險 | GREEN | GREEN | L0 |
| 寫入工作區 | YELLOW | YELLOW | L1-L2 |
| 刪除/發布/對外/憑證 | RED | RED | L3 |

## 信任邊界

- **官方 ChatGPT Web 不可被碰觸**：AIECP 的核心不變量之一；不得 DOM 注入、隱藏爬取、回應攔截、流量改寫、cookie/session 擷取、逆向工程或規避配額。
- **Spark 節點完全網路隔離**：SuperBrain 設計無 Default Gateway、無 DNS，防火牆只允許 Laptop 的特定 port（22, 30000-30010）連入。
- **外部資料一律視為 UNTRUSTED_DATA**：Voice Agent（畫面文字）、SuperBrain（外部檔案內容）都明文採用這個原則，防止 prompt injection 取得權限。
- **憑證管理**：AIECP 用 Electron/Windows OS-backed `safeStorage`；SuperBrain 規劃用 PowerShell SecretStore；兩者都要求 DB/audit 只存參照，不存明文，且進入 audit 前先做 redact。

## Public Portal 的隱私邊界（使用者要求，未實作）

使用者提及的 Public Portal 必須是**唯讀投影**，絕不能暴露本機控制或私有資料，且必須與現有的 `aeris.space653000.workers.dev`（AERIS-only 網站）明確區隔，不可觸碰或合併該既有網站。目前本輪盤點沒有找到任何已存在的 Public Portal 實作或設計文件，這是完全空白的未來項目，見 [15_PUBLIC_PORTAL_ARCHITECTURE.md](15_PUBLIC_PORTAL_ARCHITECTURE.md)。

## AERIS 既有網站（`aeris.space653000.workers.dev`）的保護原則

- 這是 AERIS 專案自己的既有資產，本輪未深入盤點其程式碼來源。
- 使用者明確要求本 SuperSystem repo 與任何未來的 Public Portal 設計都不得觸碰或合併此網站。
- 本 repo 對此網站的角色：僅在文件中提及其存在與邊界，不分析其程式碼、不建議修改。
