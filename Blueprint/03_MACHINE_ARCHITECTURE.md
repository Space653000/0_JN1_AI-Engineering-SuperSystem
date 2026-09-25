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

1. **P0 硬體盤點尚未完成**：SuperBrain `.ai/STATUS.md` 明確指出兩台 Spark 尚未實機盤點，只有 Laptop（ULTRA-MAERA-2）部分完成盤點。
2. **沒有任何 repo 證實 ULTRA-MAERA-2 上真的跑著 AIECP、AERIS、MEGIS 或 Voice Agent**。這四個 repo 的文件都沒有指定具體部署機器。
3. **Voice Agent 已經在一台「型號為 RTX Spark」的機器上跑通完整的離線語音管線**，但這台機器是否就是 SPARK-AGAVE-3 或 SPARK-AGAVE-4，目前無法確認（見 [Audit/CONFLICT_ANALYSIS.md](../Audit/CONFLICT_ANALYSIS.md) C-02）。如果是同一台實體機器，代表 SPARK 節點的角色定義需要同時容納「語音本地推論」與「FAST/DEEP 批次工作」，SuperBrain 藍圖目前沒有考慮到這個負載共存問題。

## 建議的釐清順序（僅供參考）

1. 先確認 Voice Agent 目前實際跑在哪一台實體機器上（序號、MAC address 或其他可核對的硬體識別），與 SuperBrain P0 盤點交叉比對。
2. 若確認是同一台 Spark，SuperBrain 的 FAST/DEEP 資源分配模型需要納入 Voice Agent 既有的 GPU/VRAM 佔用。
3. 若確認是不同機器，代表使用者手上可能有超過兩台 Spark 等級硬體，需要重新盤點總硬體清單。

以上三步驟目前都需要使用者本人或下一輪實機盤點才能確認，本 SuperSystem repo 無法從現有文件推得答案。
