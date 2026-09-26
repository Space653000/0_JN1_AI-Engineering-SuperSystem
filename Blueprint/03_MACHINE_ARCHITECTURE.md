# 03 — Machine Architecture

完整規格與圖見 [Registry/MACHINES.yaml](../Registry/MACHINES.yaml)、[Architecture/MACHINE_MAP.md](../Architecture/MACHINE_MAP.md)。

## 三機命名的來源

機器命名 ULTRA-MAERA-2 / SPARK-AGAVE-3 / SPARK-AGAVE-4 **唯一**在 `0_JN1_2AGAVE128-1MAERA64/.ai/STATUS.md` 的 2026-09-25 決策紀錄中被正式定案，且與使用者本次任務描述的命名完全一致。這代表機器命名層面的方向已經在 SuperBrain repo 自己的治理流程中被確認，不是本 SuperSystem repo 新發明的。

## 機器角色

| 機器 | 硬體 | 角色（SuperBrain 定義） | 角色（使用者目標架構） | 一致性 |
|---|---|---|---|---|
| ULTRA-MAERA-2 | Surface Laptop Ultra, ARM64, 64GB unified | Control Plane / Gateway / 語音視覺 I/O / 雲端代理 / SFTP 收件 | + 承載 Voice Agent、AIECP、AERIS、MEGIS | **部分一致**：Gateway/語音角色一致；「承載四個具名工程 repo」這件事未被 SuperBrain 藍圖提及 |
| SPARK-AGAVE-3 | Surface RTX Spark Dev Box, 128GB unified | FAST：批次/RAG/嵌入/VLM/本地 ASR | AIECP/SuperBrain 派工的 FAST worker | **一致**（概念層級） |
| SPARK-AGAVE-4 | Surface RTX Spark Dev Box, 128GB unified | DEEP：大模型推理/審查/Verifier/長上下文 | 獨立驗證/審查的 DEEP node | **一致**（概念層級） |

## 現實檢查

0. **2026-09-26 重大發現（見 [Blueprint/22 雷達 #7](22_GLOBAL_TECH_RADAR.md)）**：SuperBrain 藍圖裡「Surface RTX Spark，128GB unified」對應的實際產品是 **Microsoft Surface RTX Spark Dev Box**（2026-06-02 Build 2026 發表），**正式上市日是 2026-10-07**——本文撰寫當下（09-26）尚未上市。這代表下面第1點「P0 硬體盤點尚未完成」，很可能不是進度落後，而是**硬體本身還買不到**，建議 Stephen 確認這個推論是否屬實，若屬實應把「等 10/7 上市」明確排進 [Blueprint/16 Roadmap](16_ROADMAP_AND_ACCEPTANCE.md)，而不是繼續當成未解釋的落後項。另外要注意：Microsoft 同場發表的 **Surface Laptop Ultra** 搭載的是規格較低的「RTX Spark **N1X**」GPU 變體，跟 SPARK-AGAVE-3/4 用的完整版 Dev Box 是不同產品，對應 ULTRA-MAERA-2（見上表）用的正是這個較低規格的 N1X。
1. **P0 硬體盤點尚未完成**：SuperBrain `.ai/STATUS.md` 明確指出兩台 Spark 尚未實機盤點，只有 Laptop（ULTRA-MAERA-2）部分完成盤點。
2. **沒有任何 repo 證實 ULTRA-MAERA-2 上真的跑著 AIECP、AERIS、MEGIS 或 Voice Agent**。這四個 repo 的文件都沒有指定具體部署機器。
3. **Voice Agent 已經在一台「型號為 RTX Spark」的機器上跑通完整的離線語音管線**，但這台機器是否就是 SPARK-AGAVE-3 或 SPARK-AGAVE-4，目前無法確認（見 [Audit/CONFLICT_ANALYSIS.md](../Audit/CONFLICT_ANALYSIS.md) C-02）。如果是同一台實體機器，代表 SPARK 節點的角色定義需要同時容納「語音本地推論」與「FAST/DEEP 批次工作」，SuperBrain 藍圖目前沒有考慮到這個負載共存問題。

## 建議的釐清順序（僅供參考）

1. 先確認 Voice Agent 目前實際跑在哪一台實體機器上（序號、MAC address 或其他可核對的硬體識別），與 SuperBrain P0 盤點交叉比對。
2. 若確認是同一台 Spark，SuperBrain 的 FAST/DEEP 資源分配模型需要納入 Voice Agent 既有的 GPU/VRAM 佔用。
3. 若確認是不同機器，代表使用者手上可能有超過兩台 Spark 等級硬體，需要重新盤點總硬體清單。

以上三步驟目前都需要使用者本人或下一輪實機盤點才能確認，本 SuperSystem repo 無法從現有文件推得答案。

---

## 本機資料夾／路徑盤點（2026-09-25 第二輪，逐 repo 重新 clone/pull 確認）

**重要限制**：本 session 只能讀取各來源 repo 在 GitHub 上的內容，**完全沒有 Stephen 實際 Windows 機器的本機檔案系統存取權限**。下表只收錄「repo 文件裡白紙黑字寫出來的本機路徑／runtime 假設」，凡是文件沒有明講的，一律標記 `UNKNOWN / NOT VERIFIED (no local filesystem access from this session)`，不臆測、不用常識推補。

| Repo | 文件中出現的本機路徑 | 來源（檔案 + 大致位置） | 這台路徑對應哪台機器 |
|---|---|---|---|
| AERIS Core | `C:\0_JN1_AERIS` | `README.md`、`HANDOFF.md` | `UNKNOWN`（repo 從未提及具名機器） |
| AERIS Local Implementation | `C:\0_JN1_AERIS`（與 AERIS Core **共用同一本機根目錄**） | `AGENTS.md` 第9行：「Local product/source/state/Evidence root is `C:\0_JN1_AERIS`」；`README.md` 第21行同樣引用 `C:\0_JN1_AERIS\` | `UNKNOWN` |
| AERIS Supervision | 未見任何本機路徑（規則本身未提及） | `SUPERVISION_CONTRACT.md` 全文 | `UNKNOWN` |
| Offline-Local-Voice-Agent | `C:\0_JN1_Offline-Local-Voice-Agent` | `.ai/STATUS.md` 第15行「本地路徑：`C:\0_JN1_Offline-Local-Voice-Agent`」 | 型號 "RTX Spark"，**未確認**是否為 SPARK-AGAVE-3/4（見上方 R-02） |
| MEGIS | `C:\0_JN1_MEGIS` | `execution/PROJECT_STATE.md` 第80行「所有本機寫入必須留在 `C:\0_JN1_MEGIS`」，同段明文「`C:\0_JN1_AERIS` 與 `C:\0_JN1_Offline-Local-Voice-Agent` 不得變更、共用或依賴」 | `UNKNOWN` |
| AIECP | `C:\0_JN1_AIECP` | `Blueprint/23_IMPLEMENTATION_STATUS.md` 第5行、`Reports/V3_IMPLEMENTATION_PROGRESS_REPORT.md` 第5行：「**Current operator checkout:** `C:\0_JN1_AIECP`」 | `UNKNOWN`（repo 全文無具名機器） |
| SuperBrain (2AGAVE128-1MAERA64) | `C:\SuperBrain`（Laptop 端 Core 骨架、venv、SQLite、`sb` CLI 都規劃裝在這裡）；規劃中的 chroot 路徑 `C:\SuperBrain\ingest\inbox\<source>\`、輸出路徑 `C:\SuperBrain\artifacts\<task_id>\` | `.ai/BLUEPRINT.md` 第217、321、327、428、494行 | 明確對應 **ULTRA-MAERA-2**（Laptop，repo 自己的定義） |

**交叉檢查**：本輪對全部 6 個非-SuperBrain 來源 repo 重新執行 `grep -ril "ULTRA-MAERA\|SPARK-AGAVE"`，**零命中**——確認「哪個資料夾裝在哪台具名機器上」這件事，除了 SuperBrain 自己的 `C:\SuperBrain` ↔ ULTRA-MAERA-2 對應之外，**其餘六個路徑（`C:\0_JN1_AERIS`、`C:\0_JN1_Offline-Local-Voice-Agent`、`C:\0_JN1_MEGIS`、`C:\0_JN1_AIECP`）目前沒有任何來源文件指定它們實際裝在 ULTRA-MAERA-2、SPARK-AGAVE-3 或 SPARK-AGAVE-4 中的哪一台**。使用者本次任務描述「這四個都裝在 ULTRA-MAERA-2」是 TO-BE 目標架構，不是任何 repo 已確認的 AS-IS 事實（見 C-01）。
