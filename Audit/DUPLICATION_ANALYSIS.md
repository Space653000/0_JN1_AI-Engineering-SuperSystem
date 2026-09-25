# Duplication Analysis

跨專案比較同一種能力被重複實作的情況。「合理重複」指本地實作（local implementation）本來就該獨立於控制平面之外；「風險重複」指兩套本該共用的控制邏輯各自演化，未來會分裂成不相容的兩套真相。

| 能力 | AERIS / AERIS Impl | AIECP | SuperBrain (2AGAVE) | Voice Agent | MEGIS | 評估 | 建議（僅供本 SuperSystem 文件參考，不得回頭修改來源 repo） |
|---|---|---|---|---|---|---|---|
| **Queue（任務佇列）** | `UNKNOWN`（本輪未見獨立 queue 實作） | ✅ 持久 Mission/Task 狀態、有界排程器（`.ai/STATUS.md`） | ✅ 規劃中：SQLite-based queue（尚未實作，P4 骨架階段） | `UNKNOWN`（PlanRunner 多步驟規劃，非跨機 queue） | ✅ `execution/WORK_QUEUE.yaml`（Gate 工作圖佇列，非執行期任務佇列） | **風險重複**：AIECP 與 SuperBrain 都要做「跨機任務佇列」，服務對象雖不同，但概念（lease、heartbeat、retry）幾乎一致 | 兩者若真的要整合（AIECP 派工到 SuperBrain 的 Spark），應該由 AIECP 的 Queue 呼叫 SuperBrain 的 `sb submit`，而不是兩套佇列各自維護狀態機 |
| **Scheduler** | — | ✅ 有界並行度、任務相依檢查、租約與心跳 | ✅ 規劃中：heartbeat 10秒、30秒判定 OFFLINE | — | — | **風險重複**：兩套排程器的心跳/租約邏輯幾乎一模一樣的設計哲學 | 同上 |
| **Agent（Planner/Builder/Reviewer）** | `role_specs.py`、`role_acceptance.py`（100 席位角色） | ✅ Supervisor/Planner/Researcher/Builder/Reviewer/Verifier/Maintainer/Local Compute/Harness 九角色 | ✅ FAST/DEEP + 雲端工人（Codex/Claude/Gemini） | `.ai/CLAUDE_REVIEWER.md` + `.ai/CODEX_WORKER.md` | Gate `*-REV-*`/`*-ACC-*` 審查流程 | **合理重複**：每個專案都需要「誰規劃、誰動手、誰審查」這組角色，但**具體角色抽象層**應該收斂到一份 vocabulary——目前五個 repo 各自定義了外觀相似但不互通的角色詞彙 | 見 [Blueprint/11_AI_AGENT_ROLE_ARCHITECTURE.md](../Blueprint/11_AI_AGENT_ROLE_ARCHITECTURE.md) 提出的抽象角色對照表 |
| **Router（供應商/任務路由）** | — | ✅ Provider Router（Claude/Codex/Gemini/Ollama/OpenCode，角色↔供應商解耦） | ✅ 靜態分流表 `config/routing.yaml`（本地 vs 雲端，閾值 0.85） | — | — | **風險重複**：AIECP 的 Provider Router 與 SuperBrain 的 Router 概念完全一致（都是「角色 + 供應商健康度 → 選 Worker」），卻是獨立實作，未來供應商健康狀態、API 契約可能互相矛盾 | 若 AIECP 與 SuperBrain 要共存於同一個 ULTRA-MAERA-2，建議只保留一套 Router 抽象，另一個改為呼叫它 |
| **Evidence（證據）** | `aeris.traceability.json`、`aeris.review.json` | ✅ 五級證據分級（STATIC/TESTED/CI/ENVIRONMENT/OWNER-EXTERNAL），每次執行的證據根目錄 + 雜湊/清單 | ✅ SQLite state + JSONL audit；「NO EVIDENCE = NOT DONE」精神與 AERIS Supervision 一致 | ✅ `progress/p*/REPORT.md` + JSON 量測結果 | ✅ artifact classification + maturity scanner，L1/L2 fingerprint | **合理重複，但詞彙不統一**：每個專案都各自發明了一套「證據等級」，精神高度一致（都拒絕自我宣稱），但欄位/等級名稱互不相同 | 這是最值得在 SuperSystem 層級收斂成單一 Evidence 詞彙的地方（見 Blueprint 10） |
| **Approval（人類核准）** | GATE-01～08、Human Gate | ✅ 高風險合併需明確人工核准；agent 無法自我核准權限 | ✅ 三色風險分級，RED 需 exact-action digest 核准 | ✅ L0-L3 + 語音/文字雙確認 | Gate acceptance 需人工簽核 | **合理重複**：每個系統都需要自己的 approval gate，本質上是同一個「人類是最終權威」原則的不同實作 | 概念統一即可，不必合併程式碼 |
| **Dashboard** | AERIS `index.html`/`workspace.html`（靜態網頁，非即時 dashboard） | ✅ Harness Command Center（missions/tasks/approvals/CI/PR/事件流） | ⬜ 規劃中，最小化 dashboard（尚未實作） | `console/index.html`、`console/companion.html`（本機 console） | `apps/web`（`/progress` 施工進度中心，UI-0） | **合理重複**：每個專案都需要自己領域的儀表板，這是預期行為，不需要合併 | — |
| **Memory** | `docs/architecture` 相關文件（角色記憶概念，`UNKNOWN` 實作） | Context Capsule（最小必要上下文） | 規劃中（尚未實作） | `memory_op` 工具（P3，超出 35 工具表的額外項目） | `UNKNOWN` | **合理重複** | — |
| **Provider（供應商健康狀態）** | — | ✅ 五狀態（NOT_CONFIGURED/READY/DEGRADED/UNAVAILABLE/AUTH_REQUIRED） | 規劃中的 worker 健康狀態（heartbeat-based） | — | — | **風險重複**：與上方 Router 同源問題 | 同 Router |
| **Worker（隔離執行單元）** | — | ✅ Codex OFFICIAL / Codex PEGA，各自獨立 `CODEX_HOME`，隔離 worktree | ✅ 規劃中：Spark1 FAST / Spark2 DEEP，隔離網路 + 各自 worktree | — | — | **風險重複**：AIECP 的 Worker 隔離（同倉庫並行任務用不同 Git worktree）與 SuperBrain 的 Worker 隔離（不同機器 + 不同 worktree）解決的是同一類問題（避免兩個 agent 同時寫入同一份東西），概念可以共用 | 若整合，建議 SuperBrain 的跨機 Worker 沿用 AIECP 既有的 worktree 鎖定機制作為參考模式 |
| **Review** | ASTRA/SOL 獨立審查 Gate（`docs/governance/ASTRA_EXECUTION_GATE_V3.md`、`SOL_INDEPENDENT_REVIEW_GATE_V2.md`） | ✅ Reviewer 角色，Claude Code 為 Reviewer adapter | Verifier 角色（Spark2 DEEP） | `.ai/CLAUDE_REVIEWER.md` | Gate `*-REV-*` | **合理重複**：獨立審查是每個治理體系都該有的，precise mechanics 各自不同是合理的 | — |
| **Git / CI** | GitHub Actions（governance/CI 腳本存在，細節 `NOT VERIFIED`） | ✅ 完整：gh CLI gateway、`agent/<task-id>` 分支慣例、exact-HEAD CI、簽章 webhook | 規劃中：`git push origin feature/T-101` 慣例 | `UNKNOWN` | ✅ `scripts/run-baseline-ci.ps1`，本機 baseline CI | **合理重複**：每個 repo 都該有自己的 CI，這是正常的 per-repo 職責 | AIECP 的 GitHub delivery 機制已經最成熟，可作為其他專案未來參考的模式（非強制） |
| **Voice** | — | Agent Switcher（偵測 ChatGPT Web/Codex/Claude/Gemini/Ollama，非語音） | ✅ Phase A（ChatGPT Voice）/ B / C（XVF3800 離線） 三階段語音規劃 | ✅ 已驗證：VAD+喚醒詞+ASR（97.9% 中文辨識率），**目前唯一有實測語音數據的專案** | — | **風險重複／缺整合**：SuperBrain 規劃了自己的離線語音方案（Phase C：XVF3800+ASR+TTS），但 Voice Agent 專案已經在真實硬體上做出一套完整、已驗證的離線語音管線。**兩者互不知道對方存在** | 這是最直接可行的「避免重造輪子」機會：SuperBrain Phase C 不必從零建語音管線，可以評估重用 Voice Agent 現有的 ASR/喚醒詞/安全分級成果（僅供本文件建議，實際整合需雙方專案自行決定） |
| **Security（安全分級）** | GATE-01～08 | GREEN/YELLOW/RED + READ/TEST/WRITE/…/SYSTEM | GREEN/YELLOW/RED | L0–L3 | Artifact classification + maturity | **合理重複，詞彙不同**：概念收斂度高（都是分級 + 高風險需人工核准），但四套詞彙互不對應 | 見 Blueprint 13，提出一份跨專案風險等級對照表（僅供理解，不要求各專案改名） |
| **Logging** | `aeris_runtime/audit.py` | JSONL 事件日誌 + 事件回放 API | JSONL audit | Memory/Logging 資料表（P4） | `UNKNOWN` | **合理重複** | — |

## 總結

最值得注意的兩個模式：

1. **AIECP 與 SuperBrain 幾乎是同一種東西的兩份獨立藍圖**——都是「通用控制平面：Queue + Scheduler + Router + Worker + Evidence + Approval」，只是 AIECP 面向「ChatGPT Web + Git 工程任務」，SuperBrain 面向「語音指令 + 多機（含雲端多供應商）調度」。**2026-09-25 Stephen 已裁決分工邊界**：這組控制平面原語唯一歸 AIECP，SuperBrain 不重建，改往上提升為跨機統籌規劃層（見 [Blueprint/18](../Blueprint/18_DECISION_LOG.md)、[Blueprint/09](../Blueprint/09_SUPERBRAIN_COMPUTE_FABRIC.md)）。
2. **Voice Agent 已經做出來的離線語音能力，與 SuperBrain 規劃中的 Phase C 語音能力高度重疊**，但 SuperBrain 的藍圖完全沒有提到 Voice Agent 這個既有專案。這是本次盤點發現的最大「重複造輪子」風險，也是最容易在文件層面先做整合建議的地方。
