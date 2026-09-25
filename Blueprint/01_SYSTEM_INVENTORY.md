# 01 — System Inventory

完整清單見 [Registry/PROJECTS.yaml](../Registry/PROJECTS.yaml)（機器可讀）與 [Audit/REPOSITORY_INVENTORY.md](../Audit/REPOSITORY_INVENTORY.md)（逐專案詳細卡片，含來源引用）。

## 專案清單

| # | Repo | 角色定位 | 領域 | 狀態摘要 |
|---|---|---|---|---|
| 1 | `0_JN1_AERIS` | 聲學工程 Blueprint / 治理 | Acoustic engineering | A–D NOT VERIFIED, E NOT_STARTED |
| 2 | `0_JN1_AERIS_Local-computer-implementation` | AERIS 的 HOW（執行期實作） | Acoustic engineering runtime | 大部分細節 UNKNOWN（本輪未逐一深讀） |
| 3 | `0_JN1_AERIS_Supervision`（私有） | AERIS 發布監督/快照 | Publication | 機制存在，範圍僅 AERIS |
| 4 | `Offline-Local-Voice-Agent` | 離線語音代理人 | Voice / desktop automation | P0-P2 完成，P3-P6 部分完成 |
| 5 | `0_JN1_MEGIS` | 機構工程生成式系統 | Mechanical engineering | G0 完成，G1 接近完成，G2 部分，G3-G9 未開始 |
| 6 | `0_JN1_AIECP` | 通用工程控制平面（自稱 AECP） | Generic control plane | 核心已 IMPLEMENTED+TESTED+CI，10 項 ENVIRONMENT + 4 類 OWNER-EXTERNAL gate 待補 |
| 7 | `0_JN1_2AGAVE128-1MAERA64` | SuperBrain：多機語音調度 | Generic multi-machine orchestration | 規劃完成，P0 施工未開始 |

## 明確排除

- `0_JN1_Robotcar`：依使用者指示排除，本輪未 clone、未分析。

## 觀察：專案之間的耦合程度

七個 repo 中，六個對彼此**沒有程式碼或設定上的耦合**（MEGIS 甚至在自己的 README 明文聲明「不得修改、共用環境或依賴 AERIS 與 Voice Agent」）。唯一的實際耦合是 Voice Agent 與 AERIS Core 之間透過 `ORDER.md` 檔案格式的鬆散整合。這代表目前的生態系是刻意設計成「高度自治、低度耦合」，這對於避免專案互相拖累是好事，但也代表使用者設想的「System-of-Systems」需要從零開始搭建整合層——這正是本 SuperSystem repo 存在的理由。
