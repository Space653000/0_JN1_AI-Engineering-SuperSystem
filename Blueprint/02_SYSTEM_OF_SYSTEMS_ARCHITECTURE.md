# 02 — System-of-Systems Architecture

## 目標架構（TO-BE）

見 [Architecture/SYSTEM_MAP.md](../Architecture/SYSTEM_MAP.md) 的圖。核心流程：

```text
Human → Voice Agent → AIECP → Mission/Task/Queue → Domain(AERIS|MEGIS)
      → Execution(ULTRA local tool | SPARK-AGAVE-3 | Cloud Worker)
      → Verify(SPARK-AGAVE-4 | independent reviewer | deterministic verifier)
      → Evidence → Human Approval → GitHub/Release
```

## AIECP vs SuperBrain 的邊界原則（使用者確認方向）

- **AIECP = 控制平面**：mission/task/workspace/queue/policy/git/ci/approval 的權威。
- **SuperBrain = 運算織理（compute fabric）**：machines/workers/local models/execution/resource routing 的權威。
- 兩者不應互相吸收：AIECP 不應該重新發明一套機器資源分配邏輯；SuperBrain 不應該重新發明一套 Git/CI/Approval 邏輯。

## 現實檢查（AS-IS）

本輪盤點發現，**這個邊界原則目前沒有被任何一個來源 repo 實作或遵守**，原因很直接：AIECP 與 SuperBrain 根本不知道對方存在，各自都獨立長出了一套完整的 Task/Queue/Worker/Provider/Evidence/Approval 機制（詳見 [Audit/DUPLICATION_ANALYSIS.md](../Audit/DUPLICATION_ANALYSIS.md)、[Audit/CONFLICT_ANALYSIS.md](../Audit/CONFLICT_ANALYSIS.md) C-04）。

這不是「哪個 repo 設計錯了」，而是兩個專案在完全不同的時間、針對不同的直接需求（AIECP 針對「跟 ChatGPT Web 協作做 Git 工程任務」；SuperBrain 針對「用語音調度三台自己的機器」）獨立演化出來的結果。

## System-of-Systems 分層（本 repo 提出的整合模型，僅供參考）

```text
┌─────────────────────────────────────────────────────────┐
│ Layer 4 — Human Interface                                │
│   Voice Agent (STT/Intent) ｜ ChatGPT Web ｜ Public Portal│
├─────────────────────────────────────────────────────────┤
│ Layer 3 — Control Plane（權威：AIECP）                     │
│   Mission / Task / Workspace / Queue / Policy /           │
│   Git / CI / Evidence / Approval                          │
├─────────────────────────────────────────────────────────┤
│ Layer 2 — Domain Authority（各自權威，互相平行）             │
│   AERIS（聲學）｜ MEGIS（機構）｜ 未來領域...                 │
├─────────────────────────────────────────────────────────┤
│ Layer 1 — Compute Fabric（權威：SuperBrain）                │
│   ULTRA-MAERA-2（gateway）｜ SPARK-AGAVE-3（FAST）｜         │
│   SPARK-AGAVE-4（DEEP）｜ Cloud Providers                   │
└─────────────────────────────────────────────────────────┘
```

Layer 3 與 Layer 1 之間需要一個「Provider/Worker 適配層」，讓 AIECP 的 Provider Router 可以把「Local Provider」這個既有概念指向 SuperBrain 的 Spark 節點，而不需要 AIECP 自己重新實作機器管理邏輯。這個適配層目前完全不存在，是 [Audit/GAP_ANALYSIS.md](../Audit/GAP_ANALYSIS.md) 列出的第一個缺口。

## 不做的事

本文件不會、也沒有立場去指示 AIECP 或 SuperBrain 的維護者「應該重構成這個分層」。這只是本 SuperSystem repo 對使用者目標架構的一種可能落地方式，實際是否採納、何時採納，取決於 Stephen 本人與各專案自己的治理節奏。
