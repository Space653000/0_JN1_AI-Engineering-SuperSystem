# 16 — Roadmap and Acceptance

## 本 SuperSystem repo 自己的 Roadmap

見 [STATUS.md](../STATUS.md)——Phase 0-6 已全部完成（盤點、比較、架構、機器、權威矩陣、總藍圖、活體進度追蹤表建立）。**逐專案最新進度與更新方式一律以 [19_MASTER_PROGRESS_TRACKER.md](19_MASTER_PROGRESS_TRACKER.md) 為準**，下表為第二輪盤點（2026-09-25，重新 clone/pull 全部來源 repo）時的摘要快照。

## 各專案自己的施工路線圖摘要（僅整理，不代為決策優先順序；2026-09-25 第二輪重新確認）

| 專案 | 目前階段 | 下一個關鍵里程碑 |
|---|---|---|
| AERIS Core | 治理層完整，工程驗收 NOT_STARTED（HEAD `64576bd`，本輪重新確認**無變化**） | E 全面本機驗收（無時程） |
| MEGIS | **本輪重新確認有實質推進**：G0–G3 全部 closed/accepted，G4 已啟動（G4-MOD-001/G4-GRF-001 done，G4-MOD-002 in_progress） | G4-MOD-002 → G4-IMP-001 → G4-REV-001 → G4-ACC-001 |
| AIECP | 控制平面核心完成，10 ENVIRONMENT + 4 OWNER-EXTERNAL gate 待補（本輪重新確認：施工單 0001–0005 全部倉庫內關閉，但 ENVIRONMENT/OWNER-EXTERNAL 阻塞項數量未變；PR #7 尚未合併） | 真實 Windows/ARM64 環境上跑通 Codex OFFICIAL+PEGA 並行執行 |
| Voice Agent | P0-P2 完成，P3-P6 部分完成（HEAD 前進至 `4628e30`，完成度數字本輪重新確認**未變**） | P5 視覺備援完整操作閉環；P6 邊界情境覆蓋 |
| SuperBrain | 規劃完成，P0 未開始（HEAD 前進至 PR #2 合併，內容與機器命名決策本輪重新確認**未變**） | 完成三機硬體盤點 → P1 影片同款體驗 → P2 隔離網路 |
| AERIS Supervision | 機制已建立，最新快照 `S0005`（本輪重新確認**無變化**） | 視 Stephen 是否決定擴展範圍（見 [10](10_SUPERVISION_AND_EVIDENCE.md)） |

## 重疊裁決現況（2026-09-25 第二輪重新確認，取代舊有「待處理」敘述）

- ✅ **D-01 / C-04 已裁決並持續生效**：Queue/Router/Worker/Evidence/Approval 歸 AIECP 所有，SuperBrain 提升為跨機統籌規劃層。本輪重新讀取雙方最新 STATUS 文件，**未發現任何一方違反此分工**（SuperBrain 未新增 Queue/Router/Worker 實作；AIECP 持續深化既有控制平面）。
- ⚠️ **D-02 仍未解決，且仍是最值得優先處理的重工風險**：Voice Agent 已驗證的離線語音管線與 SuperBrain 規劃中的 Phase C 語音方案完全重疊，本輪重新對兩專案全文（含 SuperBrain 新增的三個機器資料夾）交叉 grep 比對，**兩者仍互不知道對方存在**。
- ⚠️ **G-01（Voice Agent↔AIECP）、G-02（AIECP↔AERIS/MEGIS）、G-03（AIECP↔SuperBrain）介面契約仍完全缺失**——本輪未見任何一個來源 repo 新增對應文件或程式碼，維持「下一優先待辦」定位。
- 🟡 **G-04（SPARK-AGAVE-4 驗證 SPARK-AGAVE-3 協議）維持 Stephen 認領、待其自行設計**，本輪不代為設計，非本 SuperSystem repo 的阻塞項。

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
