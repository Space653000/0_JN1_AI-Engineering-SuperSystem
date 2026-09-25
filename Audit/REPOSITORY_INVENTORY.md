# Repository Inventory

盤點時間：2026-09-25。每張卡片的欄位都盡量附上來源檔案；查無來源或來源矛盾的欄位標記 `UNKNOWN`。

---

## 1. 0_JN1_AERIS

- **Project Name**：0_JN1_AERIS（AERIS Core / Blueprint repo）
- **Purpose**：聲學工程領域的「一位人類主管 + 100 個聲學專業能力席位」AI 工程師系統的**設計藍圖與治理**倉庫。（`README.md` 第1行）
- **Domain**：Acoustic engineering（聲學工程）
- **Canonical Blueprint**：`docs/AERIS_BLUEPRINT_ZH_TW.md`（繁中總藍圖，README 稱為「產品入口」）；治理鐵律在 `constitution.md`（GATE-01～08）。
- **Current Version**：Architecture revision `v0.7.0-governance.4`；已建立的發布 tag 為 `v0.7.0-blueprint.1`（`BLUEPRINT_BASELINE.md`）。
- **Source of Truth**：`BLUEPRINT_BASELINE.md` 明文：「定稿以實際 tag 與驗證紀錄為準，產品能力仍須驗收」；`HANDOFF.md` 要求每次接手都要重新核對 Blueprint / Implementation / Local checkout / Running service 四方版本。
- **Acceptance**：三輪發布檢查（盤點候選 commit → 隔離審查 → CI + 治理檢查）；三輪檢查只證明「該批文件審查通過」，不是產品完成證明（`BLUEPRINT_BASELINE.md`）。
- **Current Status**：`HANDOFF.md` 明文「A–D 整體 NOT VERIFIED」「E 全面本機驗收 NOT_STARTED」。**`v0.7.0-blueprint.1` 這個 tag 本身是「設計凍結」的證據，不是「產品已完成驗收」的證據**——這點在 `BLUEPRINT_BASELINE.md` 講得很清楚：「定稿只固定這一版設計與驗收要求」「100 席位的逐項專業驗證仍待完成」。
- **Runtime**：唯一正式產品與本機寫入根目錄是 `C:\0_JN1_AERIS`（`README.md`、`HANDOFF.md`）。是純文件/治理 repo，本身不含執行期程式（工程邏輯在姊妹倉庫 Local-computer-implementation）。
- **Main Machine**：`UNKNOWN`——repo 內文件從未提及 ULTRA-MAERA-2 / SPARK-AGAVE 等機器名稱（已用 grep 全文搜尋確認零命中）。只確定「唯一正式位置 `C:\0_JN1_AERIS`」，沒有指定是哪一台實體機器。
- **Agent Roles**：`docs/architecture/CANONICAL_ROLE_REGISTRY_V1.json` 定義 100 個聲學專業角色（Role ID），但 README 明文強調「100 席位是責任/能力邊界，不是 100 個常駐代理或 100 名已驗證真人工程師」。
- **External Dependencies**：UX/UI 方向參考 Kairos（雷小蒙）、Agent Zero（代理/工具/記憶呈現）、氛圍學院／哈利說的（技能教學呈現）——三者都是「設計參考方向」，README 明文「是否採用其程式碼或執行框架，須另行評估授權、安全性、相容性及維護成本」。
- **Inputs**：聲學工程問題（人類主管提出）。
- **Outputs**：聲學工程分析/決策 + 執行證據（Evidence）。
- **Security Boundary**：`docs/governance/GITHUB_ACCESS_BOUNDARY.md`（存在，內容 `NOT VERIFIED` 本輪未深讀）；`constitution.md` 定義 GATE-01～08 強制治理。
- **Evidence Model**：`aeris.traceability.json`、`aeris.review.json`——PASS 僅適用列出的 commit 與文件範圍，不代表產品驗收（`HANDOFF.md`）。
- **Integration Points**：與 Implementation repo 之間透過「Blueprint 是 WHAT，Implementation 是 HOW」分工（`README.md`）；與 Offline-Local-Voice-Agent 之間透過 `ORDER.md` 檔案格式鬆散整合（見該專案卡片）。
- **Current Progress**：治理文件層完整（governance/gate/traceability 齊全），工程能力驗收 NOT_STARTED。
- **Current Blockers**：`HANDOFF.md`：「E 全面本機驗收仍為 NOT_STARTED」；schema v3 是治理候選，舊 runtime consumer 尚未遷移，不得宣稱相容或 enforcement 已完成（`docs/governance/AI_READ_ORDER.md`）。
- **Duplicated Capabilities**：治理 Gate / Review 機制與 AIECP 的 GREEN/YELLOW/RED 分級、MEGIS 的 Gate-driven construction 概念相似（見 [DUPLICATION_ANALYSIS.md](DUPLICATION_ANALYSIS.md)）。
- **Potential Conflicts**：與使用者「confirmed architecture」中「AERIS 跑在 ULTRA-MAERA-2」的說法無法核實，repo 內無對應內容（見 [CONFLICT_ANALYSIS.md](CONFLICT_ANALYSIS.md) C-01）。
- **Do Not Break Rules**：`main` 需 PR + CI + 既有保護規則，禁止直接 push/force push/刪除；定稿 tag 建立後禁止移動或刪除（`BLUEPRINT_BASELINE.md`）。

---

## 2. 0_JN1_AERIS_Local-computer-implementation

- **Project Name**：AERIS Local Implementation
- **Purpose**：AERIS 藍圖的本機執行實作（HOW），提供 `aeris_runtime` Python 套件，涵蓋治理、審查、多項聲學工程領域方法（array beam、auracast、hearing aid、microphone architecture、room correction 等，見 `aeris_runtime/engineering/*.py` 檔案清單，約 30+ 個工程子模組）。
- **Domain**：Acoustic engineering runtime
- **Canonical Blueprint**：以 `0_JN1_AERIS` 的藍圖為 WHAT，本 repo 為 HOW（`0_JN1_AERIS/README.md`）。
- **Current Version**：`UNKNOWN`（本輪未深入讀取此 repo 的 STATUS/ACCEPTANCE 對應文件，僅盤點目錄結構）。
- **Source of Truth**：`AGENTS.md`、`CLAUDE.md`、`aeris.local.policy.yaml`（存在，內容細節本輪未逐一核對）。
- **Acceptance**：`UNKNOWN`——未在本輪讀取範圍內找到獨立 ACCEPTANCE.md；比照 AERIS Core 的三輪檢查機制推測適用，但未經核實，標記 `NOT VERIFIED`。
- **Current Status**：`NOT VERIFIED`（AERIS Core 的 HANDOFF.md 指出 Implementation 相容性「仍待完成」）。
- **Runtime**：Python 套件（`aeris_runtime/__main__.py`、`cli.py`），目錄結構顯示大量工程領域模組皆有對應 `*_review.py` 檔案（例如 `array_beam.py` + `array_beam_review.py`），暗示「builder + reviewer」成對設計模式。
- **Main Machine**：`UNKNOWN`；同樣無 ULTRA-MAERA-2/SPARK-AGAVE 字樣。
- **Agent Roles**：`role_acceptance.py`、`role_specs.py`、`professional_profiles.py` 存在，對應 AERIS 的 100 席位角色系統，細節 `NOT VERIFIED`。
- **External Dependencies**：`UNKNOWN`。
- **Inputs / Outputs**：`UNKNOWN`（需要進一步讀取 `aeris_runtime/cli.py`、`controlplane.py` 才能確認，本輪未讀）。
- **Security Boundary**：`tools/local-only/Protect-AERISReadOnly.ps1`、`Verify-AERISReadOnly.ps1` 暗示有「本機唯讀保護」機制，細節 `NOT VERIFIED`。
- **Evidence Model**：`aeris_runtime/audit.py`、`claim_guard.py` 存在，細節 `NOT VERIFIED`。
- **Integration Points**：`blueprint_compatibility.py`——顯示這個 repo 有機制檢查自己與 AERIS Blueprint repo 的相容性。
- **Current Progress**：`NOT VERIFIED`；至少 30 個工程子領域模組已建立骨架（每個都有 review 對照檔），但完成度未經本輪逐一驗證。
- **Current Blockers**：依 AERIS Core `HANDOFF.md`：「Implementation 相容性、四方版本對齊、100 席位的逐項專業驗證仍待完成」。
- **Duplicated Capabilities**：`UNKNOWN`。
- **Potential Conflicts**：`UNKNOWN`。
- **Do Not Break Rules**：`UNKNOWN`（未讀 `aeris.local.policy.yaml` 細節）。

---

## 3. 0_JN1_AERIS_Supervision（私有 repo）

- **Project Name**：AERIS Supervision
- **Purpose**：AERIS 的**發布監督層**——把 AERIS ISI（三檔案 bundle）以 SHA-256 驗證後發布為不可變的歷史快照（`SUPERVISION_CONTRACT.md`）。
- **Domain**：Publication / release supervision for AERIS only
- **Canonical Blueprint**：`SUPERVISION_CONTRACT.md`（本身即是規格，只有 12 條規則，非長篇藍圖）。
- **Current Version**：`UNKNOWN`（`LATEST.json` 存在但本輪未讀取內容）。
- **Source of Truth**：`SUPERVISION_CONTRACT.md` 規則 7：「ChatGPT web supervisor 必須獨立核對 Blueprint、Implementation、Supervision、local/runtime identity、PR state 與 CI」——即這個 repo 本身**不是**權威真相，只是一個發布快照機制。
- **Acceptance**：規則 6：「Publication 不代表 engineering PASS、release approval 或 Human acceptance」。
- **Current Status**：機制存在（`automation/` 內有多支 PowerShell 腳本：`AERISContinuousWorker.ps1`、`AERISParallelOrchestrator.ps1` 等），但工程實質內容有限，主要是自動化腳本與快照目錄（`snapshots/2026/09/`）。
- **Runtime**：Windows PowerShell 自動化（人類手動執行 `AERIS_Supervision_Publisher.bat` 才觸發，規則 1）。
- **Main Machine**：`UNKNOWN`（規則未提及機器名稱）。
- **Agent Roles**：無 AI agent 角色定義；這是純自動化/發布流程，人類（Chief Engineer）保留最終 GO/NO-GO（規則 8）。
- **External Dependencies**：依賴 `0_JN1_AERIS`（Blueprint）與 `0_JN1_AERIS_Local-computer-implementation`（Implementation）的內容，但**規則 5 明文禁止**：「Publisher must never modify Blueprint or Implementation repositories」——這正是本 SuperSystem repo 應該效法的唯讀邊界原則。
- **Inputs**：AERIS ISI V6 三檔案 bundle（SHA-256 已驗證）。
- **Outputs**：不可變快照目錄 + `LATEST.json` 指標（規則 3、4）。
- **Security Boundary**：規則 3：歷史快照目錄不可覆寫/刪除/rebase/force-push/改寫。
- **Evidence Model**：規則 12：「NO EVIDENCE = NOT DONE」——與 AIECP 的證據分級鐵律精神一致。
- **Integration Points**：只對 AERIS 生態系（Blueprint + Implementation）生效，未提及 MEGIS/AIECP/Voice Agent/SuperBrain。
- **Current Progress**：機制已建立（自動化腳本、快照結構），但發布內容的工程完整性仍取決於 AERIS 自身的驗收狀態。
- **Current Blockers**：`NOT VERIFIED`——本輪未深入讀取 `automation/` 內各腳本的實作細節。
- **Duplicated Capabilities**：`UNKNOWN`——可能與 AIECP 的 GitHub delivery/evidence 機制在概念上有重疊，需要進一步比對（見 GAP_ANALYSIS）。
- **Potential Conflicts**：目前設計明確限定 **Current State = AERIS-only**。若要擴展為「SuperSystem-wide 發布監督」，需要新的、明確的範圍決策——這正是 Blueprint 10 章要求文件化的 Current/Future/Migration Risk/Recommended Boundary。
- **Do Not Break Rules**：`SUPERVISION_CONTRACT.md` 全文；本 SuperSystem repo 絕不提議修改此 repo，只在 [Blueprint/10_SUPERVISION_AND_EVIDENCE.md](../Blueprint/10_SUPERVISION_AND_EVIDENCE.md) 討論其邊界。

---

## 4. Offline-Local-Voice-Agent

- **Project Name**：Offline-Local-Voice-Agent（本機路徑 `C:\0_JN1_Offline-Local-Voice-Agent`，`.ai/STATUS.md`）
- **Purpose**：完全離線的中文語音代理人，透過語音控制 Windows 電腦（ASR/VAD/喚醒詞 → 意圖辨識 → Windows 工具呼叫 → 安全分級 → Vision fallback）。
- **Domain**：Voice / desktop automation，**與 AERIS 聲學工程領域相關但獨立**——`docs/05_AERIS_Integration_Split.md`：「兩專案完全獨立施工、不共用程式碼，只透過 `ORDER.md` 檔案格式互通」。
- **Canonical Blueprint**：`docs/03_ClaudeCode_Execution_Blueprint_v0.1.md`（🟢 正式採用）+ `docs/04_Full_Phase_Plan_and_Compute_Allocation.md`（🟢 有效展開），索引於 `.ai/BLUEPRINT.md`。`docs/01`、`docs/02` 為背景參考，非執行標準。
- **Current Version**：HEAD `86ff7fd`（`.ai/STATUS.md`，2026-09-24 盤點）。
- **Source of Truth**：`.ai/BLUEPRINT.md`（索引） + `.ai/STATUS.md`（進度，即時盤點方法：直接查 `git log`/`git status`/跑回歸測試，不憑印象）。
- **Acceptance**：P0–P6 分階段驗收，各 Phase 有明確 KPI（`.ai/BLUEPRINT.md` 第3、4節）：中文 ASR CER < 5%、指令路由 ≥95%、Wake→ASR < 1.5秒等。
- **Current Status**：P0–P2 已 100% 完成且驗收；P3 電腦操作約 90%；P4 安全機制約 95%；P5 視覺備援部分完成（約定位建議可用，完整操作閉環未完成）；P6 完整整合約 55%（`.ai/STATUS.md`）。
- **Runtime**：**在 RTX Spark（ARM64，統一記憶體架構）上執行**，ASR（whisper.cpp）與 LLM（llama.cpp）皆在其 GPU 上跑通（`.ai/STATUS.md` P0）。**這是本輪盤點中唯一明確指出「在 Spark 硬體上跑」的來源 repo**——但這裡的 "RTX Spark" 是否等同於 2AGAVE128-1MAERA64 藍圖裡的 `SPARK-AGAVE-3`/`SPARK-AGAVE-4`，repo 內文件從未明講，需視為同型號硬體但**專案間彼此不知道對方存在**。
- **Main Machine**：一台 RTX Spark（型號級別，非 2AGAVE 藍圖裡具名的機器）。`UNKNOWN` 是否為同一台實體機器。
- **Agent Roles**：`.ai/CLAUDE_REVIEWER.md`（審查者）、`.ai/CODEX_WORKER.md`（施工者）——與 AIECP 的 Planner/Builder/Reviewer 角色模式一致但獨立定義。
- **External Dependencies**：完全離線，正式運作階段禁止任何雲端 API（`.ai/BLUEPRINT.md` 核心原則1）；本地模型：whisper.cpp、llama.cpp、Qwen3-VL-8B-Instruct（P5 視覺備援）。
- **Inputs**：麥克風語音、螢幕畫面（Vision fallback）。
- **Outputs**：Windows 桌面操作結果（透過 34 個真實工具，`.ai/STATUS.md` P3）。
- **Security Boundary**：L0～L3 風險分級 + 語音/文字雙確認 + Emergency Stop（P4，已驗證約95%）；核心原則2：LLM 不可直接執行任意 Shell/PowerShell，一律走 Structured Tool Call → Policy Engine → Executor；核心原則6：畫面上文字一律視為 UNTRUSTED DATA。
- **Evidence Model**：`progress/p*/REPORT.md` + JSON 結果檔（例如 `p1_asr_bench/results.json`）——每個 Phase 都有可重現的量測證據，模式與 AIECP 的 STATIC/TESTED/CI/ENVIRONMENT 分級精神一致但獨立實作。
- **Integration Points**：唯一已知跨專案整合點——`ORDER.md` 檔案格式，與 AERIS 互通（`docs/05_AERIS_Integration_Split.md`）。**與 AIECP、SuperBrain、ULTRA-MAERA-2/SPARK-AGAVE 命名體系完全沒有連結**（grep 全文搜尋零命中）。
- **Current Progress**：見上方 Current Status；已知限制誠實記錄（喚醒詞誤觸發判別力偏弱、Wake→ASR 延遲 5.16 秒未達 1.5 秒目標、部分工具因裝置限制無法完整驗證）。
- **Current Blockers**：P5 視覺備援閉環未完成；P6 邊界情境覆蓋有限；喚醒詞 8 小時 KPI 未直接測過（已用替代驗證方式取代，經使用者同意）。
- **Duplicated Capabilities**：安全分級（L0-L3）與 AIECP 的 GREEN/YELLOW/RED、SuperBrain 的三色風險分級概念相同但三套獨立實作（見 DUPLICATION_ANALYSIS）。
- **Potential Conflicts**：使用者「confirmed architecture」認為 Voice Agent 跑在 ULTRA-MAERA-2 並把請求送進 AIECP；但實際 repo 顯示 Voice Agent 獨立跑在 Spark 硬體上、完全離線、且與 AIECP 沒有任何已知整合（見 CONFLICT_ANALYSIS C-02）。
- **Do Not Break Rules**：`docs/05_AERIS_Integration_Split.md`：兩專案不共用程式碼；`README.md`類比：本專案資料需留在自己機器上（完全離線原則）。

---

## 5. 0_JN1_MEGIS

- **Project Name**：MEGIS（Mechanical Engineering Generative Intelligence System）
- **Purpose**：把機械/聲學/製造工程師的判斷轉化為可追溯、可驗證、可重現的引導式生成工程流程；LLM 只協助整理設計意圖、提問、解釋結果，不取代幾何核心/物理求解器/工程簽核（`README.md`）。
- **Domain**：Mechanical engineering / generative design
- **Canonical Blueprint**：`MEGIS_Blueprint/MEGIS_..._v3.0-claude-code.md`（V3.0，Gate-driven construction，唯一主要施工依據）；V2.0、V1.0 為封存基線/參考。
- **Current Version**：V3.0（採用決策見 `docs/decisions/ADR-0001-adopt-v3-blueprint.md`）。
- **Source of Truth**：`README.md` + `execution/PROJECT_STATE.md` + `execution/WORK_QUEUE.yaml`。
- **Acceptance**：Gate-driven（G0–G9），「不以日期或推測百分比宣稱完成」，每個 Gate 需可重跑證據才能進下一 Gate。
- **Current Status**：G0（11 項）全部完成；G1 工程契約/golden cases/migration/rollback/14 個錯誤碼/機器可讀 envelope 已固定，仍缺 `G1-REQ-001`（參考案例研究）、`G1-REV-001`/`G1-ACC-001`（審查與簽核）；G2 已完成 geometry contract、fixture base、assembly geometry，仍缺 export/reload pipeline 等；G3–G9 僅有藍圖與工作圖，尚未施工。V3 必要工作圖共 74 項：27 done、1 in progress、46 planned。
- **Runtime**：`apps/web`（Vite/TypeScript 前端，UI-0 進度中心與引導式 demo，綁定 `127.0.0.1`，明文「不應公開至區域網路或網際網路」）+ Python 工程核心（CadQuery、FreeCAD TechDraw fallback）。
- **Main Machine**：唯一正式位置 `C:\0_JN1_MEGIS`（`README.md`）；`UNKNOWN` 是否為 ULTRA-MAERA-2。README 明文「本專案不得修改、共用環境或依賴 `C:\0_JN1_AERIS` 與 `C:\0_JN1_Offline-Local-Voice-Agent`」——**MEGIS 自己主動聲明與 AERIS、Voice Agent 隔離**，這是專案自治原則的直接證據。
- **Agent Roles**：Claude Code 為主要施工者（`CLAUDE.md`、`AGENTS.md` 存在），Gate 審查/簽核機制（`*-REV-*`、`*-ACC-*` 工作圖）。
- **External Dependencies**：CadQuery（幾何核心）、FreeCAD（TechDraw fallback）、COMSOL（本機正式決策為 `out_of_scope`，非阻塞）。
- **Inputs**：機構工程設計意圖（目前僅 UI-0 合成展示資料，非真實工程輸入）。
- **Outputs**：目前僅 `FEASIBILITY_SPIKE` 等級的 STEP/STL/DXF（`maturity: null`），**不是**製造輸出；BOM/drawing/Prototype Package 尚未施工（G5）。
- **Security Boundary**：`docs/ARTIFACT_POLICY.md`（tracked file 1 MiB、evidence JSON 50 KiB、golden 200 KiB 預算）；`docs/CLASSIFICATION_AND_MATURITY.md`（五種 artifact classification，maturity 上限為 `PROTOTYPE`，`ENGINEERING_REVIEWED` 與 `RELEASED` 必須經合格工程師及法規流程）。
- **Evidence Model**：L1 byte hash、L2 semantic fingerprint、跨 process replay（`V3C-DET-001`）；manifest maturity scanner 全庫掃描。
- **Integration Points**：`UNKNOWN`——README 明確排除與 AERIS/Voice Agent 共用環境；與 AIECP/SuperBrain 無任何提及（grep 零命中）。
- **Current Progress**：見 Current Status；有清楚的「為什麼目前網站沒有更多產品或功能」自我揭露表格，逐項標註「現況」與「誠實邊界」。
- **Current Blockers**：`G1-REQ-001`（參考案例參數研究）為目前的關銵路徑阻塞項，其後 `G1-REV-001`/`G1-ACC-001`/`V3C-REV-001`/`V3C-ACC-001` 依序排隊。
- **Duplicated Capabilities**：Gate/Review/Acceptance 機制與 AERIS constitution、AIECP 的 P0-P7 roadmap 概念相似（各自獨立實作）。
- **Potential Conflicts**：與 AERIS/Voice Agent 的「完全隔離」聲明本身沒有衝突（是刻意設計）；但與使用者「所有工程領域都在 ULTRA-MAERA-2 上被 AIECP 統一調度」的目標架構有落差（見 CONFLICT_ANALYSIS C-01）。
- **Do Not Break Rules**：`README.md`：不得修改/共用環境/依賴 AERIS 與 Voice Agent；禁止 `git push --force`；同步前後需核對本地與遠端 commit SHA。

---

## 6. 0_JN1_AIECP

- **Project Name**：0_JN1_AIECP（repo 名稱含 "AI"），但**專案內部文件全程自稱 "AECP"（AI Engineering Control Plane）**，README 標題與 `.ai/*.md` 全部使用 AECP 這個縮寫，未見 repo 內文件使用 "AIECP" 字樣。**這是一個需要記錄的命名不一致（見 CONFLICT_ANALYSIS C-03）。**
- **Purpose**：Windows-first 本機工程控制平面。使用者持續用官方 ChatGPT Web 作為主要對話介面，AECP 提供本機執行環境：Workspace、檔案、repo、工具、任務狀態、政策、驗證、證據、選用本地運算與未來的模型供應商（`Blueprint/00_MASTER_BLUEPRINT.md`）。
- **Domain**：Generic engineering control plane（非特定工程領域，可套用在任何 Git repo 專案上）。
- **Canonical Blueprint**：`Blueprint/00_MASTER_BLUEPRINT.md` 為總入口，`Blueprint/01`–`24` 共 24 份細部規格；`.ai/BLUEPRINT.md` 為施工入口索引。
- **Current Version**：套件版本 `v0.3.0 Preview`（`package.json`）；「V3.0 Multi-Worker」是藍圖施工階段名稱，並非已發布的 3.0.0 版本（README 明文強調不要混淆）。
- **Source of Truth**：`.ai/STATUS.md`（現況）+ `.ai/BLUEPRINT.md`（應該做成什麼樣）+ `.ai/ACCEPTANCE.md`（怎樣算做完）——三份文件互補，任何宣稱與 `Blueprint/23_IMPLEMENTATION_STATUS.md` 或 `STATUS.md` 矛盾時，先更新這兩份文件才能繼續其他工作。
- **Acceptance**：**證據等級鐵律**——STATIC / TESTED / CI / ENVIRONMENT / OWNER-EXTERNAL 五級，下層證據不能冒充上層，「Blueprint 存在 ≠ Runtime 完成」「repo 測試 ≠ 真實 provider 證據」（`.ai/ACCEPTANCE.md` 第0節）。R1–R8 正規需求逐條核對；P0–P7 交付階段驗收閘門。
- **Current Status**（2026-09-24 盤點）：Control Plane 核心、Harness 執行（Planner→Builder→Verify→Reviewer）、治理分級（READ/TEST/WRITE/…/SYSTEM）、GitHub 交付（Draft PR、CI 監控、bounded rework）、可觀測性（JSONL 事件、Command Center）、Multi-Worker（Codex OFFICIAL + Codex PEGA 各自獨立 CODEX_HOME）均已 IMPLEMENTED + TESTED；exact-HEAD CI 多次 SUCCESS（附 run 編號）。**尚未關閉的是 10 項 ENVIRONMENT gate（需真實機器/帳號執行）與 4 類 OWNER-EXTERNAL gate（只有 repo owner 本人能結案，例如 Authenticode 簽章、Microsoft Store 身分）**；另有 6 項刻意不做的設計選擇（例如任意 shell 執行、遠端任務提交）。
- **Runtime**：Electron 桌面應用（context isolation on、Node integration off、sandbox on、narrow preload），Windows x64 + ARM64。
- **Main Machine**：`UNKNOWN`——repo 內文件從未提及 ULTRA-MAERA-2 或任何具名機器；只講「Windows-first」「使用者的電腦」。與 SuperBrain/2AGAVE128-1MAERA64 之間**零文字重疊**（已用 grep 全文搜尋確認）。
- **Agent Roles**：`Blueprint/21_AGENT_ROLES_AND_HANDOFF_PROTOCOL.md` 定義 Supervisor / Planner / Researcher / Builder / Reviewer / Verifier / Maintainer / Local Compute / Harness 九個穩定角色，Worker 身分與角色分離（例如 `role: Builder, worker: codex-official, provider: openai-official`）。目前實際 Worker：Codex OFFICIAL、Codex PEGA（`https://aiapi.t-cyber.com/v1`）；Provider Router 支援 Claude Code、Codex CLI、Gemini CLI、Ollama、OpenCode。
- **External Dependencies**：官方 ChatGPT Web（不碰 DOM/cookie/session）、GitHub（Source of Truth）、GitHub Actions（CI/事件骨幹）、PEGA API endpoint。
- **Inputs**：使用者在 ChatGPT Web 產生的結構化 `aecp.task/v1` Command Card；或 Goal Loop 的 Goal/Definition of Done。
- **Outputs**：`aecp.result/v1` Result Capsule；受治理的 Git commit/push/Draft PR（merge 仍需人工核准）。
- **Security Boundary**：R1「信任邊界」——不得碰 ChatGPT DOM/session；renderer 無 Node 特權；憑證僅以 OS 保護參照存在；RED 操作需明確人工閘門。GitHub 為工程 Source of Truth；Harness/Control Plane 狀態為 runtime 真相；Dashboard 只是投影。
- **Evidence Model**：見上方 Acceptance；Dashboard 對每個 Worker 投影 canonical runtime state，無法驗證時顯示 `UNKNOWN`（而非假裝已知）。
- **Integration Points**：目前設計為「通用控制平面」，可綁定任意 Workspace/Git repo；**未見任何綁定 AERIS/MEGIS/Voice Agent 特定領域邏輯的程式碼或文件**（grep 全文搜尋零命中）。
- **Current Progress**：見 Current Status；npm test 從 254/254 → 290/290 持續增加，多個 Work Order（0001–0005）已由 Claude Code 驗收 CLOSED。
- **Current Blockers**：10 項 ENVIRONMENT gate（真實 Codex OFFICIAL/PEGA 執行、ARM64 實機、Ollama smoke test、Official Full MCP 端到端驗收等）；4 類 OWNER-EXTERNAL gate（Authenticode 簽章憑證、Microsoft Store 身分、遠端網域/TLS 擁有權、供應商正式生產憑證）。
- **Duplicated Capabilities**：Queue/Scheduler/Worker/Router/Evidence/Approval 幾乎與 SuperBrain（2AGAVE128-1MAERA64）的 Control Plane 概念一對一重疊，但兩者是完全獨立的實作，互不知道對方存在（見 DUPLICATION_ANALYSIS，這是本次盤點最大的重複發現）。
- **Potential Conflicts**：見 CONFLICT_ANALYSIS C-01、C-03（AECP vs AIECP 命名；AECP vs SuperBrain 角色重疊）。
- **Do Not Break Rules**：`Blueprint/00_MASTER_BLUEPRINT.md` 產品不變量（Product invariants）：官方 ChatGPT 不可被碰觸；不得規避供應商配額；本地優先資料；最小必要雲端上下文；人類權威；證據優於自我宣稱；可復原性；單一狀態來源。

---

## 7. 0_JN1_2AGAVE128-1MAERA64（SuperBrain）

- **Project Name**：0_JN1_2AGAVE128-1MAERA64，README 自稱 **SuperBrain**。
- **Purpose**：「語音多機調度的混合式 AI 系統」——一台 Surface Laptop Ultra（控制平面，唯一連網）+ 兩台 Surface RTX Spark Dev Box（隔離的本地 AI），雲端工人使用 ChatGPT/Codex、Claude Code、Gemini（`README.md`）。
- **Domain**：Personal multi-machine compute orchestration（**不是特定工程領域**，是通用的「語音下指令 → 多機分派 → 驗證 → 回報」個人助理型控制平面）。
- **Canonical Blueprint**：`.ai/BLUEPRINT.md`（v2.1，明文「本文件是本專案唯一的藍圖依據」，取代三份 ChatGPT 深入研究報告，衝突時以本文件為準）。
- **Current Version**：v2.1（2026-09-24）。
- **Source of Truth**：`.ai/BLUEPRINT.md` + `.ai/ACCEPTANCE.md`（T01–T30 驗收測試）+ `.ai/STATUS.md`。
- **Acceptance**：Phase Gate P0–P12，每個 Phase 全部條件 PASS 才能進下一階段；PASS 需要證據（指令輸出、檔案、log、截圖、audit event），「Agent 自己說完成了不算數」。
- **Current Status**：**研究與規劃已完成，施工尚未開始（P0 未開始）**（`README.md`、`.ai/STATUS.md` 明文）。目前只有 Laptop 部分盤點完成（CPU/GPU/RAM/OS/SQLite 版本等），兩台 Spark 尚未實機盤點。
- **Runtime**：規劃中的 FastAPI Ingress（`127.0.0.1:8765`）+ SQLite（rollback journal，因偵測到的 SQLite 3.49.1 屬 WAL bug 受影響版本）+ `sb` CLI 作為語音與 Agent 的唯一入口。**目前尚未有一行對應的執行期程式碼確認存在**（本輪未在目錄中看到 `src/` 或 API 實作，只有 `SPARK-AGAVE-3/scripts/spark-bootstrap.ps1`、`SPARK-AGAVE-4/scripts/spark-bootstrap.ps1`、`ULTRA-MAERA-2/scripts/*.ps1` 等安裝腳本）。
- **Main Machine**：**這是唯一明確定義 ULTRA-MAERA-2 / SPARK-AGAVE-3 / SPARK-AGAVE-4 三個機器名稱的來源 repo**（`.ai/STATUS.md` 2026-09-25 決策紀錄：「機器名稱定案：Laptop Ultra → ULTRA-MAERA-2、Spark1（FAST）→ SPARK-AGAVE-3、Spark2（DEEP）→ SPARK-AGAVE-4」）。這與使用者本次任務給的機器命名**完全一致**。但角色定義上，`BLUEPRINT.md` §3.1 對 ULTRA-MAERA-2（Laptop）的定義是「Control Plane、Gateway、語音與視覺 I/O、雲端代理、SFTP 收件」，**並未提及 AIECP、AERIS、MEGIS、Voice Agent 這幾個 repo 的名稱或角色**——即機器命名對得上，但「這台機器上要跑哪些工程領域服務」這件事目前只存在於使用者本次任務的敘述裡，不存在於本 repo 的藍圖文字中。
- **Agent Roles**：FAST（Spark1/SPARK-AGAVE-3：批次、RAG、嵌入、VLM）、DEEP（Spark2/SPARK-AGAVE-4：大模型推理、審查、Verifier、長上下文）——與使用者任務描述的「SPARK-AGAVE-3 做事、SPARK-AGAVE-4 驗證」精神一致。雲端工人：Codex（ChatGPT Plus，`codex exec --json`）、Claude Code（Pro，`claude -p`）、Gemini（Antigravity CLI 免費額度，`agy -p`，僅 best-effort）。
- **External Dependencies**：ChatGPT Plus、Claude Pro、Antigravity CLI（Gemini 免費層，注意 2026-06-18 起 Gemini CLI 個人 Google 登入已停止）；reSpeaker XVF3800 麥克風陣列；Logi C920 相機。
- **Inputs**：語音指令（Phase A：ChatGPT 桌面 Voice → Codex → `sb` CLI；Phase C：XVF3800 → 本地 ASR）。
- **Outputs**：任務執行結果 + 語音回報摘要 + Evidence。
- **Security Boundary**：三色風險分級（GREEN/YELLOW/RED，RED 需 exact-action digest 核准）；Spark 完全網路隔離（無 Default Gateway/DNS，防火牆只允許 Laptop 的特定 port）；prompt injection 防護（外部資料視為 UNTRUSTED_DATA，不能取得權限）。
- **Evidence Model**：SQLite state + JSONL audit；DONE 只能由 Verifier 寫入，Agent 最多回報 `WORK_COMPLETE_CLAIMED`。
- **Integration Points**：`UNKNOWN`——藍圖全文未提及與 AERIS/MEGIS/AIECP/Voice Agent 的任何整合點；`sb` CLI 是獨立指令列，Router 依 `config/routing.yaml` 靜態分流表分派任務類型（例如 `zh_summary`、`code_edit`、`architecture_review`），這些任務類型是通用類別，不對應到聲學/機構工程領域。
- **Current Progress**：規劃完整（BLUEPRINT 自我檢查表 §18 全部打勾），施工進度為零（P0 尚未完成盤點）。
- **Current Blockers**：需使用者親自完成 Tailscale 登入、採購 UPS/交換器/網卡、決定測試資料是否可上雲、決定喚醒詞名稱、決定是否退回 Windows 正式版（脫離 Insider build）。
- **Duplicated Capabilities**：Queue/Router/Approval/Worker/Evidence 概念與 AIECP 幾乎一對一重疊（見 DUPLICATION_ANALYSIS）——兩者都是「通用控制平面」設計，但服務對象不同（AIECP 面向 Git 工程任務 + ChatGPT Web；SuperBrain 面向語音下達的個人生活/工程雜項任務 + 多雲端 Agent）。
- **Potential Conflicts**：見 CONFLICT_ANALYSIS C-01（機器命名對上但角色定義未涵蓋 AIECP/AERIS/MEGIS）、C-04（SuperBrain vs AIECP 的 Control Plane 角色重疊）。
- **Do Not Break Rules**：`.ai/CLAUDE_REVIEWER.md`（RED 動作審查規則，存在但本輪未逐行讀取）；「在 T01–T08 全部通過之前，禁止導入 Kubernetes/Redis/NATS/RabbitMQ/PostgreSQL/Grafana/向量資料庫/agent swarm 框架/雙 Spark 叢集/GPT-Live API」。

---

## 排除項目

- **0_JN1_Robotcar**：依使用者指示明確排除，本輪未 clone、未分析。
