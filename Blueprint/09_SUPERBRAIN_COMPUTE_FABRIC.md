# 09 — SuperBrain Compute Fabric

## 使用者定義

「SuperBrain = ULTRA-MAERA-2 + SPARK-AGAVE-3 + SPARK-AGAVE-4 together（compute fabric），不是只有 2AGAVE128-1MAERA64 這個 repo 本身」。

## 與實際 repo 內容的核對

`0_JN1_2AGAVE128-1MAERA64` repo 自己的 README 第一行就自稱 **SuperBrain**，且確實定義了三機協同架構（`.ai/BLUEPRINT.md` §3、§5）。就「SuperBrain = 三機整體」這一點，**repo 內容與使用者定義是一致的**——這是本次盤點中少數幾個「目標與現實吻合」的地方。

但有一個重要的範圍落差：repo 定義的 SuperBrain 職責是「語音多機調度的**通用**混合式 AI 系統」——它的 Router 分流表（`config/routing.yaml`）處理的任務類型是 `zh_summary`、`test_data_analysis`、`code_edit`、`architecture_review` 這類通用類別，**完全沒有涵蓋 AERIS 的聲學工程運算或 MEGIS 的機構工程幾何運算**。也就是說：

- **機器層級**（三台機器叫什麼名字、各自角色）：repo 定義與使用者目標**一致**。
- **職責範圍層級**（SuperBrain 是否等於「承載整個工程生態系運算需求的 compute fabric」）：repo 定義是**個人語音助理型的通用調度系統**，範圍比使用者設想的「整個工程生態系的運算織理」窄。

## SuperBrain 現有架構重點（來源真實內容）

- Router 判定順序：安全閘門 → 隱私閘門 → 確定性工具 → 分流表 → 工人健康與額度 → 升級。
- 本地優先規則：golden set 上，本地分數 ≥ 雲端 85% 的任務類型預設走本地。
- 三色風險分級（GREEN/YELLOW/RED），RED 需 exact-action digest 核准。
- DONE 只能由 Verifier 寫入；Agent 最多回報 `WORK_COMPLETE_CLAIMED`。
- Spark 節點完全網路隔離，只能透過 Laptop（ULTRA-MAERA-2）向雲端求援（NEEDS_ESCALATION 流程）。
- 施工階段 P0-P12，目前卡在 **P0（三台機器盤點）尚未完成**。

## 命名補充（2026-09-26）：SuperBrain 與 JN1-UOD 的範圍差異

SuperBrain＝三台機器本身（硬體層 compute fabric）。**Stephen 已選定一個更大範圍的正式名稱 JN1 Unified Operations Domain（JN1-UOD）**，涵蓋 SuperBrain（三機硬體）＋跑在其上的四個軟體層（Voice Control、AIECP、AERIS、MEGIS）整體。未來 AERIS Supervision 擴大後（改名 JN1 Unified Oversight Authority，JN1-UOA）監管的對象就是這整個 JN1-UOD，不只是 SuperBrain 硬體。詳見 [Blueprint/10](10_SUPERVISION_AND_EVIDENCE.md)、[Blueprint/18 決策記錄](18_DECISION_LOG.md)。這只是命名與範圍定義，**尚未在任何來源 repo 落地**。

## Stephen 已裁決的分工邊界（2026-09-25，見 [Blueprint/18](18_DECISION_LOG.md)）

為避免 C-04 / D-01 所述「AIECP 與 SuperBrain 重工」風險，Stephen 裁定：

- **Queue、Router、Worker、Evidence、Approval 這組控制平面原語唯一歸屬 AIECP**。SuperBrain 不建置、也不維護第二套同性質的佇列/排程/供應商路由/證據/核准機制。
- **SuperBrain 的定位提升為跨機資源調度與統籌規劃層**：負責「這個任務該去 ULTRA-MAERA-2 的本機工具、SPARK-AGAVE-3、還是 Cloud Worker」這類機器層級的資源決策與統籌，而不是重造 AIECP 已有的排程/佇列/核准機制。實務上應理解為 SuperBrain 消費 AIECP 的 Queue/Router 輸出、在其上做跨機分派，而不是自己另開一條平行的任務生命週期。
- 這是**決策方向**，尚未回頭修改 `0_JN1_AIECP` 或 `0_JN1_2AGAVE128-1MAERA64` 任一來源 repo；實際程式碼落地由兩個專案各自的治理流程執行。
- **G-04（SPARK-AGAVE-4 獨立驗證 SPARK-AGAVE-3 的協議）Stephen 已認領，後續自行設計**，本 repo 暫不代為設計，僅追蹤於 Roadmap。2026 年業界「Verifier Pattern / Maker-Checker」的公開參考模式已整理於 [Audit/EXTERNAL_RESEARCH.md](../Audit/EXTERNAL_RESEARCH.md) §2，供設計時參考（非強制採用）。

外部業界對「Control Plane vs Orchestration」的分層描述，與上述分工裁決方向一致，詳見 [Audit/EXTERNAL_RESEARCH.md](../Audit/EXTERNAL_RESEARCH.md) §1。

## 建議的職責擴展方向（僅供參考，未被 repo 採納）

若要讓 SuperBrain 真正成為「整個 SuperSystem 的 compute fabric」，需要：

1. 在 `config/routing.yaml` 的分流表新增工程領域任務類型（例如 `acoustic_simulation`、`geometry_generation`），並定義這些任務類型該路由到哪個節點。
2. AIECP 的 Provider Router 把 SPARK-AGAVE-3/4 註冊為 Local Provider 選項（見 [08](08_AIECP_ORCHESTRATION_ARCHITECTURE.md)）。
3. 定義 SPARK-AGAVE-4 對 SPARK-AGAVE-3 的獨立驗證協議（見 [11](11_AI_AGENT_ROLE_ARCHITECTURE.md) 與 [Audit/GAP_ANALYSIS.md](../Audit/GAP_ANALYSIS.md)，目前完全空白）。

這些都是新設計工作，不是「修正現有錯誤」——SuperBrain repo 目前的範圍設定本身沒有問題，只是還沒有被要求涵蓋整個工程生態系。
