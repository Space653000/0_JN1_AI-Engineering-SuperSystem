# 17 — Risk / Gap / Conflict Register

彙整自 [Audit/CONFLICT_ANALYSIS.md](../Audit/CONFLICT_ANALYSIS.md) 與 [Audit/GAP_ANALYSIS.md](../Audit/GAP_ANALYSIS.md)，加上風險評級供 Stephen 快速掃視。

| ID | 類型 | 摘要 | 嚴重度 | 詳情 |
|---|---|---|---|---|
| C-01 | Conflict | 使用者目標架構（ULTRA-MAERA-2 承載 AIECP+AERIS+MEGIS+Voice Agent）在任何來源 repo 中都未被實作或互相引用 | 高 — 這是整個 System-of-Systems 願景能否落地的根本前提 | Audit/CONFLICT_ANALYSIS.md C-01 |
| C-02 | Conflict | Voice Agent 實際整合對象是 AERIS（透過 ORDER.md），不是 AIECP；且完全離線設計與 AIECP 依賴 ChatGPT Web 的模式不同 | 高 | Audit/CONFLICT_ANALYSIS.md C-02 |
| C-03 | Conflict | Repo 名稱 `0_JN1_AIECP` vs 專案內部自稱 "AECP" 命名不一致 | 低 — 純命名問題，不影響功能 | Audit/CONFLICT_ANALYSIS.md C-03 |
| C-04 | Conflict — **已裁決** | AIECP 與 SuperBrain 都各自宣稱了「control plane + compute fabric」的完整範圍。**2026-09-25 Stephen 裁定**：Queue/Router/Worker/Evidence/Approval 歸 AIECP，SuperBrain 提升為跨機統籌規劃層、不重建這組控制平面原語 | 中高 → 決策已下，待 AIECP/SuperBrain 各自落地 | Audit/CONFLICT_ANALYSIS.md C-04；Blueprint/18 決策記錄 |
| C-05 | Conflict（已由來源自我澄清） | AERIS `v0.7.0-blueprint.1` tag 容易被誤讀為「已完成」，實際是「設計凍結」 | 低 — repo 自己文件已澄清，只需外部引用時小心措辭 | Audit/CONFLICT_ANALYSIS.md C-05 |
| G-01 | Gap | Voice Agent ↔ AIECP 介面契約完全缺失 | 高 | Audit/GAP_ANALYSIS.md §1 |
| G-02 | Gap | AIECP ↔ AERIS/MEGIS 派工介面完全缺失 | 高 | Audit/GAP_ANALYSIS.md §1 |
| G-03 | Gap | AIECP ↔ SuperBrain 呼叫介面完全缺失 | 中高 | Audit/GAP_ANALYSIS.md §1 |
| G-04 | Gap — **已認領** | SPARK-AGAVE-4 獨立驗證 SPARK-AGAVE-3 的協議完全未定義（使用者原始問題23）。**2026-09-25 Stephen 表示將自行設計**，本 repo 不代為設計，暫緩處理。已附上業界 Verifier Pattern 參考（Audit/EXTERNAL_RESEARCH.md §2）供設計輸入 | 高 → owner 已認領，非阻塞本輪 | Audit/GAP_ANALYSIS.md §1；Blueprint/11；Blueprint/18 決策記錄；Audit/EXTERNAL_RESEARCH.md |
| G-05 | Gap | Public Portal 資料投影契約未定義 | 低 — 使用者已標示為未來項目 | Audit/GAP_ANALYSIS.md §1 |
| G-06 | Gap | 跨專案 GitHub/CI 擁有權未定義（每個 repo 各自為政） | 中 | Audit/GAP_ANALYSIS.md §2 |
| G-07 | Gap | 跨專案統一核准佇列不存在，Stephen 需要在多個系統分別核准 | 中 | Audit/GAP_ANALYSIS.md §2 |
| D-01 | Duplication risk — **已裁決** | AIECP 與 SuperBrain 的 Queue/Scheduler/Router/Worker/Provider 概念高度重疊，各自獨立實作。**2026-09-25 Stephen 裁定**：這組能力唯一歸屬 AIECP，SuperBrain 不得另建同性質實作 | 中高 → 決策已下 | Audit/DUPLICATION_ANALYSIS.md；Blueprint/18 決策記錄 |
| D-02 | Duplication risk | Voice Agent 已驗證的離線語音能力與 SuperBrain 規劃中的 Phase C 語音能力重疊，SuperBrain 藍圖未提及 Voice Agent | 高 — 最直接可行的「避免重造輪子」機會 | Audit/DUPLICATION_ANALYSIS.md |
| D-03 | Duplication (acceptable) | 每個專案各自的 Evidence/風險分級詞彙不同但精神一致 | 低 | Audit/DUPLICATION_ANALYSIS.md；Blueprint/10、13 |
| R-01 | Risk | ULTRA-MAERA-2 是唯一連網 Gateway，若故障則整個目標架構的雲端協作路徑中斷，目前無備援設計 | 中高 | Blueprint/14 |
| R-02 | Risk | 機器身分不確定：Voice Agent 現用的 RTX Spark 是否為 SPARK-AGAVE-3/4 之一，未經確認 | 中 — 影響資源規劃準確性 | Blueprint/03 |
| R-03 | Risk | AERIS Supervision 若未來擴展為跨專案發布監督，可能稀釋其目前簡潔、範圍明確的優點 | 低（僅為潛在風險，非現況問題） | Blueprint/10 |

## 使用方式

這份表格是給 Stephen 快速掃視用的儀表板式清單。實際處理順序、是否處理、由誰處理，完全由 Stephen 或各專案自己的治理流程決定；本 SuperSystem repo 不會主動去「解決」清單裡任何一條（因為解決方案通常需要修改來源 repo，而本 repo 被禁止這麼做）。
