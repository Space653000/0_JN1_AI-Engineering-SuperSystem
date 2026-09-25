# 00 — Master Blueprint

> 本文件直接回答使用者提出的 25 個問題。每個答案先給「確認的目標方向（TO-BE）」，再對照「本輪盤點的實際發現（AS-IS）」，兩者不一致時明確標註，並指向詳細章節與衝突登錄。

## 1. 什麼是整個 AI 系統？

一個由 Stephen 一人主管、跨越六個獨立 GitHub repo 的 AI 工程生態系：三個工程領域權威（AERIS＝聲學、MEGIS＝機構、未來可能更多）、一個通用工程控制平面（AIECP）、一個語音入口（Voice Agent）、一個多機運算織理（SuperBrain：ULTRA-MAERA-2 + SPARK-AGAVE-3 + SPARK-AGAVE-4）、以及一個發布監督層（AERIS Supervision，目前僅服務 AERIS）。目標是讓語音下的一句話，經過控制平面路由到正確的工程領域，在正確的機器上執行，經獨立驗證，交付人類核准後進 GitHub。**AS-IS：這六個 repo 目前是六座各自完整但互不相連的孤島**，見 [02](02_SYSTEM_OF_SYSTEMS_ARCHITECTURE.md)。

## 2. 有哪些專案存在？

見 [01_SYSTEM_INVENTORY.md](01_SYSTEM_INVENTORY.md) 與 [Registry/PROJECTS.yaml](../Registry/PROJECTS.yaml)：AERIS Core、AERIS Local Implementation、AERIS Supervision（私有）、Offline-Local-Voice-Agent、MEGIS、AIECP、2AGAVE128-1MAERA64（SuperBrain）。`0_JN1_Robotcar` 依指示排除。

## 3. 每個專案擁有什麼？

見 [04_PROJECT_RESPONSIBILITY_MATRIX.md](04_PROJECT_RESPONSIBILITY_MATRIX.md)。摘要：AERIS 擁有聲學工程判斷權威；MEGIS 擁有機構工程判斷權威；AIECP 擁有 Git/CI/Task/Approval 的控制平面邏輯（但目前是通用的，未綁定任何領域）；Voice Agent 擁有語音輸入與 Windows 桌面控制；SuperBrain 擁有多機硬體資源與 Router；AERIS Supervision 擁有 AERIS 自己的發布快照。

## 4. 哪台機器跑什麼？

**目標**：ULTRA-MAERA-2 跑 Voice Agent + AIECP + AERIS + MEGIS；SPARK-AGAVE-3 跑 FAST 執行；SPARK-AGAVE-4 跑 DEEP 驗證。**AS-IS：沒有任何來源 repo 證實這個部署已經發生**——AERIS/MEGIS/AIECP 三個 repo 都沒有指定具體機器名稱，只有 SuperBrain repo 定義了機器命名本身。詳見 [03_MACHINE_ARCHITECTURE.md](03_MACHINE_ARCHITECTURE.md)、[Audit/CONFLICT_ANALYSIS.md](../Audit/CONFLICT_ANALYSIS.md) C-01。

## 5. 誰規劃（Plan）？

抽象角色 Planner；目前現實由 Claude Code 擔任（AIECP Blueprint 00 §11：「Claude Code is the preferred Planner/Reviewer adapter」）。見 [11_AI_AGENT_ROLE_ARCHITECTURE.md](11_AI_AGENT_ROLE_ARCHITECTURE.md)。

## 6. 誰建造（Build）？

抽象角色 Builder；目前由 Codex CLI（OFFICIAL 與 PEGA 兩個獨立 worker，各自隔離 CODEX_HOME）擔任，僅在隔離 worktree 內動手。

## 7. 誰審查（Review）？

抽象角色 Reviewer，目前由 Claude Code 擔任（不可為原 Builder 本人，見問題24）。AERIS 另有自己的 ASTRA/SOL 獨立審查 Gate；MEGIS 有自己的 `*-REV-*` Gate 審查。

## 8. 誰驗證（Verify）？

抽象角色 Verifier——鐵律是「確定性測試，不是模型自稱」（AIECP：「A model response is never sufficient proof」）。目標架構下 SPARK-AGAVE-4 是 DEEP 驗證節點，但**目前未實作**，也沒有具體協議（見問題23）。

## 9. 誰派工（Dispatch）？

抽象角色 Harness。目前 AIECP 有自己完整的 Harness（Mission→Task→Queue→Scheduler）；SuperBrain 規劃了自己的 `sb` CLI + Router，但**兩者互不相通**（見 [08](08_AIECP_ORCHESTRATION_ARCHITECTURE.md)、[09](09_SUPERBRAIN_COMPUTE_FABRIC.md)）。

## 10. 誰擁有本地運算？

目標：SuperBrain（ULTRA-MAERA-2 + 兩台 Spark）擁有本地運算資源分配。AS-IS：SuperBrain 的 Router 目前只服務通用任務類型，未納入 AERIS/MEGIS 的工程運算需求（CadQuery、聲學模擬等）。見 [Audit/GAP_ANALYSIS.md](../Audit/GAP_ANALYSIS.md)。

## 11. 誰擁有語音？

目前只有 Offline-Local-Voice-Agent 擁有已驗證的語音能力（97.9% 中文辨識率）。SuperBrain 也規劃了自己的語音方案（Phase A/B/C），兩者互不知道對方存在——這是重複造輪子的高風險點（見 [Audit/DUPLICATION_ANALYSIS.md](../Audit/DUPLICATION_ANALYSIS.md)）。

## 12. 誰擁有 GitHub？

沒有統一角色。每個 repo 各自管理自己的 branch protection 與 CI；Owner 全部是 Stephen 本人。AIECP 有最完整的「Harness 代理 GitHub 交付」機制（governed delivery，merge 仍人工）。見 [Audit/GAP_ANALYSIS.md](../Audit/GAP_ANALYSIS.md)。

## 13. 誰擁有 CI？

同上，沒有跨專案 CI 政策，每個 repo 有自己獨立的 GitHub Actions。AIECP 的 CI 治理（exact-HEAD 驗證、簽章 webhook）目前最成熟。

## 14. 誰擁有證據（Evidence）？

每個系統各自定義自己的證據等級（AIECP 五級 STATIC/TESTED/CI/ENVIRONMENT/OWNER-EXTERNAL；AERIS 的 traceability/review JSON；SuperBrain 的 SQLite+JSONL audit；MEGIS 的 artifact classification+fingerprint）。精神一致（拒絕自我宣稱），詞彙不統一。見 [10_SUPERVISION_AND_EVIDENCE.md](10_SUPERVISION_AND_EVIDENCE.md)。

## 15. 誰擁有人類核准？

Stephen 本人，在每個系統各自的 Approval Gate 裡（AIECP 的 RED 操作核准、SuperBrain 的 exact-action digest、AERIS 的 constitution GATE、Voice Agent 的語音+文字雙確認）。**沒有一個統一的跨系統核准佇列**（見 [Audit/GAP_ANALYSIS.md](../Audit/GAP_ANALYSIS.md)）。

## 16. 三台機器中若有一台故障會怎樣？

**目前沒有任何 repo 定義跨機故障轉移**。SuperBrain 藍圖有 Spark 斷線後 30 秒判定 OFFLINE、收回 lease 重新派工的機制設計，但這只是單機器內的 Worker 層級容錯規劃（尚未實作），不是「三機互為備援」的架構。見 [14_FAILURE_RECOVERY_AND_RESILIENCE.md](14_FAILURE_RECOVERY_AND_RESILIENCE.md)。

## 17. 如果網路斷線會怎樣？

Voice Agent 本身設計為完全離線，不受影響。AIECP 的 Web Safe Bridge 依賴官方 ChatGPT Web，斷線會退化；SuperBrain 規劃了 T17（Internet 斷線仍需維持本地 health/status/基本任務）驗收測試，但尚未實作驗證。ULTRA-MAERA-2 是唯一連網節點，若其斷網，兩台 Spark 本來就設計成本來不連網，不受影響；但雲端 Worker（Codex/Claude/Gemini）全部無法使用。

## 18. 雲端 Token 用完會怎樣？

SuperBrain 有明確設計：Router 依 golden set 分流，Antigravity 免費額度耗盡時自動 fallback（T30 驗收測試，未實作驗證）；AIECP 的 Provider 健康狀態機（AUTH_REQUIRED/UNAVAILABLE）會標記供應商不可用但不會讓 Safe Bridge 失效。兩套機制都存在但都還沒有真實驗證過。

## 19. 本地模型可以取代什麼？

SuperBrain 的判斷原則：golden set 實測，本地分數 ≥ 雲端 85% 的任務類型預設走本地（尚未執行 benchmark）。Voice Agent 已證明本地 ASR/LLM 在特定任務（中文語音辨識、意圖分類）可達到很高準確率。見 [12_PROVIDER_AND_MODEL_ROUTING.md](12_PROVIDER_AND_MODEL_ROUTING.md)。

## 20. AIECP 如何派工到 AERIS/MEGIS？

**目前完全沒有這條路徑。** AIECP 的任務 schema（`aecp.task/v1`）是通用的（`inspect-workspace`、`git-status`），AERIS/MEGIS 也沒有暴露可被外部呼叫的 API。這是本次盤點中最關鍵的缺失介面之一，見 [Registry/INTERFACES.yaml](../Registry/INTERFACES.yaml) 的 `aiecp-to-aeris`、`aiecp-to-megis`。

## 21. Voice Agent 如何把請求送進 AIECP？

**目前沒有這條路徑。** Voice Agent 實際的整合對象是 AERIS（透過 `ORDER.md`），不是 AIECP；且 Voice Agent 設計原則是完全離線，AIECP 則依賴官方 ChatGPT Web 作為主要輸入介面，兩者的輸入模型本質上不同。見 [Audit/CONFLICT_ANALYSIS.md](../Audit/CONFLICT_ANALYSIS.md) C-02、[07_CROSS_PROJECT_HANDOFF_PROTOCOL.md](07_CROSS_PROJECT_HANDOFF_PROTOCOL.md)。

## 22. AIECP 如何呼叫 SuperBrain？

**目前沒有這條路徑。** 兩者是完全獨立的通用控制平面實作，見 [Audit/CONFLICT_ANALYSIS.md](../Audit/CONFLICT_ANALYSIS.md) C-04、[08](08_AIECP_ORCHESTRATION_ARCHITECTURE.md)/[09](09_SUPERBRAIN_COMPUTE_FABRIC.md) 提出的建議邊界（僅供參考，未被任一來源 repo 採納）。

## 23. SPARK-AGAVE-4 如何獨立驗證 SPARK-AGAVE-3？

**沒有任何來源 repo 回答這個問題。** SuperBrain 藍圖只定義了 FAST/DEEP 的角色分工（批次執行 vs 推理審查），沒有具體的「DEEP 節點驗證 FAST 節點輸出」的觸發條件、資料格式或介面協議。這是需要新設計的部分，見 [Audit/GAP_ANALYSIS.md](../Audit/GAP_ANALYSIS.md)、[11_AI_AGENT_ROLE_ARCHITECTURE.md](11_AI_AGENT_ROLE_ARCHITECTURE.md) 的建議方向。

## 24. 如何避免 Builder 審查自己？

AIECP 的既有機制已經回答了這個問題：Planner/Builder/Reviewer/Verifier 是分離的角色，Worker 身分（例如 codex-official）與 Provider 身分分開追蹤，Reviewer 目前指派給 Claude Code（不同於擔任 Builder 的 Codex），且「Neither OFFICIAL nor PEGA may self-declare engineering completion」（AIECP Blueprint 00 §11A）。這個既有設計值得作為整個 SuperSystem 的通用原則：**任何角色都不能同時是自己工作的驗證者**。SuperBrain 的 SPARK-AGAVE-4 若要落實這個原則，其驗證邏輯／模型不應與 SPARK-AGAVE-3 共用同一次推理結果。

## 25. 每個 repo 的 Source of Truth 在哪？

見 [05_SOURCE_OF_TRUTH_AND_AUTHORITY.md](05_SOURCE_OF_TRUTH_AND_AUTHORITY.md) 完整矩陣。摘要：
- AERIS Core → `docs/AERIS_BLUEPRINT_ZH_TW.md` + `constitution.md`
- AERIS Local Impl → `AGENTS.md`（Implementation 層，以 Core 的 Blueprint 為 WHAT）
- AERIS Supervision → `SUPERVISION_CONTRACT.md`（本身不是工程真相，只是發布快照規則）
- Voice Agent → `.ai/BLUEPRINT.md`（索引）+ `.ai/STATUS.md`（即時查證式進度）
- MEGIS → `MEGIS_Blueprint/..._v3.0-claude-code.md` + `execution/PROJECT_STATE.md`
- AIECP → `.ai/BLUEPRINT.md` + `.ai/STATUS.md` + `.ai/ACCEPTANCE.md`（三份互補）
- SuperBrain → `.ai/BLUEPRINT.md`（明文「本文件是本專案唯一的藍圖依據」）

---

## 本 SuperSystem repo 自己的定位聲明

這份文件描述的「目標架構」是使用者本次任務指示中確認的方向，**不是任何來源 repo 已經採納的架構**。本 SuperSystem repo 的角色是把這個目標寫清楚、把現實落差誠實列出來（見 [17_RISK_GAP_CONFLICT_REGISTER.md](17_RISK_GAP_CONFLICT_REGISTER.md)），並在 Registry 裡用機器可讀格式登錄現況，供未來任何一個專案的治理流程參考。本 repo 不會、也沒有權限去修改任何來源 repo 使其符合這個目標。
