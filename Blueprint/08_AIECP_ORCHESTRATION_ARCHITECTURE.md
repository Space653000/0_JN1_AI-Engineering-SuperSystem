# 08 — AIECP Orchestration Architecture

> 本章整理自 `0_JN1_AIECP` 的既有藍圖（`Blueprint/00_MASTER_BLUEPRINT.md`），並標註使用者目標架構下 AIECP 應扮演的角色與現況落差。

## AIECP 現有架構（來源真實內容）

- **產品定位**：Windows-first 本機工程控制平面。使用者持續用官方 ChatGPT Web 作為主要對話介面，AIECP 提供本機執行環境。
- **架構平面**：Presentation / Control / Local Agent Harness / Adapter（Tool + Provider）/ Data-Evidence 五層。
- **連線模式**：Mode A Web Safe Bridge（預設，剪貼簿交接 Command Card/Result Capsule）、Mode B Official MCP Bridge（選用）、Mode C External API Provider（選用）、Mode D Local Provider（選用）。
- **任務生命週期**：`INBOX → READY → DISPATCHED → RUNNING → VERIFYING → PASS → READY_TO_COMMIT → DONE`，write 任務不可跳過驗證。
- **Multi-Worker**：Provider 與 Worker 分離；目前有 Codex OFFICIAL 與 Codex PEGA 兩個隔離 Worker，各自獨立 `CODEX_HOME`；Dashboard 對每個 Worker 投影 canonical runtime state。
- **產品不變量**：官方 ChatGPT 不可被碰觸、不規避供應商配額、本地優先資料、最小必要雲端上下文、人類權威、證據優於自我宣稱、可復原性、單一狀態來源。

## 使用者目標架構下 AIECP 應扮演的角色

「AIECP = 控制平面（mission/task/workspace/queue/policy/git/ci/approval），不吸收 SuperBrain 的 compute fabric 職責」。

## 現況落差

1. **AIECP 目前是通用的，未綁定任何工程領域**。其 Command Card schema（`aecp.task/v1`）只支援 `inspect-workspace`、`git-status` 這類通用動作，沒有聲學/機構工程專屬的任務類型或路由邏輯。要讓 AIECP 承擔「依領域派工到 AERIS/MEGIS」的職責，需要新增領域路由層——目前不存在。
2. **AIECP 目前不知道 SPARK-AGAVE-3/4 的存在**。其 Provider Router 支援 Ollama/OpenCode 等本地供應商的抽象概念，理論上可以擴展成把 SPARK 節點註冊為一種 Local Provider，但這需要 AIECP 一側新增對應的 Provider Adapter，目前未實作。
3. **AIECP 的輸入來源設計預設是 ChatGPT Web（人類手動複製貼上 Command Card）**，尚未考慮語音直接輸入的場景（見 [07](07_CROSS_PROJECT_HANDOFF_PROTOCOL.md)）。
4. **AIECP 自己的證據分級（STATIC/TESTED/CI/ENVIRONMENT/OWNER-EXTERNAL）是目前七個來源 repo 裡最嚴謹、文件化最完整的一套**，值得作為整個 SuperSystem 的 Evidence 詞彙參考基礎（見 [10](10_SUPERVISION_AND_EVIDENCE.md)）。

## 建議（僅供參考）

若 Stephen 決定推進「AIECP 作為跨領域 Control Plane」這個方向，第一步應該是在 AIECP 自己的 Blueprint 裡新增一份「Domain Routing」規格（例如新的 `Blueprint/25_DOMAIN_ROUTING.md`），定義任務如何依領域分類、以及呼叫 AERIS/MEGIS 的介面契約長什麼樣子。這個決策與實作屬於 AIECP repo 自己的治理範圍，本 SuperSystem repo 不代為執行。
