# 16 — Roadmap and Acceptance

## 本 SuperSystem repo 自己的 Roadmap

見 [STATUS.md](../STATUS.md)——Phase 0-5 本輪已全部完成（盤點、比較、架構、機器、權威矩陣、總藍圖）。

## 各專案自己的施工路線圖摘要（僅整理，不代為決策優先順序）

| 專案 | 目前階段 | 下一個關鍵里程碑 |
|---|---|---|
| AERIS Core | 治理層完整，工程驗收 NOT_STARTED | E 全面本機驗收（無時程） |
| MEGIS | G0 完成，G1 接近完成 | `G1-REQ-001`（參考案例研究）→ `G1-REV-001`/`G1-ACC-001` → G2 剩餘工作 |
| AIECP | 控制平面核心完成，10 ENVIRONMENT + 4 OWNER-EXTERNAL gate 待補 | 真實 Windows/ARM64 環境上跑通 Codex OFFICIAL+PEGA 並行執行 |
| Voice Agent | P0-P2 完成，P3-P6 部分完成 | P5 視覺備援完整操作閉環；P6 邊界情境覆蓋 |
| SuperBrain | 規劃完成，P0 未開始 | 完成三機硬體盤點 → P1 影片同款體驗 → P2 隔離網路 |
| AERIS Supervision | 機制已建立 | 視 Stephen 是否決定擴展範圍（見 [10](10_SUPERVISION_AND_EVIDENCE.md)） |

## 若要推進使用者目標架構（跨專案整合），建議的優先順序（本 repo 提案，非強制）

這不是任何專案的官方路線圖，只是本 SuperSystem repo 基於本輪發現的落差，提出的一種可能排序：

1. **釐清機器身分**（見 [03](03_MACHINE_ARCHITECTURE.md)）：確認 Voice Agent 現在跑的 RTX Spark 是否為 SPARK-AGAVE-3/4 之一。這是後續所有機器層級整合的前提。
2. **完成 SuperBrain P0-P2**：沒有實機盤點與網路隔離，SuperBrain 作為 compute fabric 的角色無法驗證。
3. **設計 AIECP ↔ 領域專案的路由介面**（見 [08](08_AIECP_ORCHESTRATION_ARCHITECTURE.md)）：先從一個最小場景開始（例如 AIECP 能否呼叫 AERIS 的某個唯讀查詢功能），不必一次做完整整合。
4. **設計 Voice Agent → AIECP 的 handoff**（見 [07](07_CROSS_PROJECT_HANDOFF_PROTOCOL.md)）：或者評估是否維持現狀（Voice Agent → AERIS 直接整合），視使用者實際使用場景的優先順序而定。
5. **設計 SPARK-AGAVE-4 驗證 SPARK-AGAVE-3 的協議**（見 [11](11_AI_AGENT_ROLE_ARCHITECTURE.md)）：在 SuperBrain P5-P7（FAST/DEEP 上線）階段一併設計。

**這份排序只是建議，不是任何專案已承諾的計畫，且完全不涉及本 repo 對其他 repo 的任何修改動作。**

## 驗收標準的統一鐵律（跨所有專案已自然收斂，值得明文延續）

不論哪個專案、哪個層級的整合，都應該延續本輪盤點發現的共同文化：**證據優於自我宣稱，Blueprint 存在不等於 Runtime 完成，下層證據不能冒充上層證據**。這是 AIECP 講得最完整的一套語言（STATIC/TESTED/CI/ENVIRONMENT/OWNER-EXTERNAL），但精神在其他五個專案裡都能找到對應版本（見 [10](10_SUPERVISION_AND_EVIDENCE.md)）。
