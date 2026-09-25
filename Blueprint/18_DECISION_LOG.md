# 18 — Decision Log

記錄本 SuperSystem repo 建置過程中做的重要判斷。這是本 repo 自己的決策紀錄，不代表任何來源 repo 的決策。

| 日期 | 決策 | 理由 |
|---|---|---|
| 2026-09-25 | 採用只讀方式盤點全部七個來源 repo（其中六個公開透過匿名 clone、一個私有 repo 透過 add_repo 附加 read 權限） | 遵守任務指示的「絕對規則」：只讀不寫其他 repo |
| 2026-09-25 | 排除 `0_JN1_Robotcar` | 使用者明確指示排除 |
| 2026-09-25 | 對 AIECP 命名採用「本 SuperSystem repo 統一使用 AIECP 稱呼，但引用其原文時保留 AECP」的折衷方案 | 使用者堅持系統名稱為 AIECP，但誠實引用來源文件的實際用字（見 Audit/CONFLICT_ANALYSIS.md C-03） |
| 2026-09-25 | 把使用者「confirmed architecture」明確標示為 TO-BE（目標），並在每個相關章節同時列出 AS-IS（現況） | 避免讀者誤以為這是已經運作的系統；同時不擅自用現況推翻使用者的方向 |
| 2026-09-25 | AERIS Supervision 的擴展建議維持保守（建議選項 A：AERIS-only），不主動提議擴大範圍 | 沒有任何來源 repo 表達過擴展需求，避免本 repo 過度延伸建議 |
| 2026-09-25 | 對 AIECP `Blueprint/01`-`20`、`24` 等未逐份精讀的文件，在 Audit/GAP_ANALYSIS.md 中誠實揭露此限制 | 遵守「有證據才下結論」原則，避免基於未讀內容做出過度推論 |
| 2026-09-25 | 使用 grep 全文搜尋交叉驗證「repo 之間是否互相提及」的關鍵發現（C-01、C-02），而非僅憑抽樣閱讀判斷 | 這是本次盤點最重要的結論，需要更高的確定性 |
| 2026-09-25 | **Stephen 裁定 C-04 / D-01（AIECP vs SuperBrain 重疊）分工邊界**：Queue / Router / Worker / Evidence / Approval 這組「控制平面原語」歸 AIECP 所有；SuperBrain **不得**另建一套同性質的 Queue/Router/Worker/Evidence/Approval，SuperBrain 的定位往上提升為**跨機資源調度與統籌規劃層**（決定「哪個任務該去哪台機器」的上層決策，而非重造 AIECP 已有的排程/佇列機制）。此為 Stephen 本人對本 SuperSystem 分工方向的決策，尚未回頭修改任一來源 repo，屬於本 repo 記錄的「建議分工方向」，實際落地仍需 AIECP / SuperBrain 各自專案採納 | 避免 D-01 所述「兩套控制邏輯各自演化、未來分裂成不相容真相」的風險重複；使用者明確裁示不想重工 |
| 2026-09-25 | G-04（SPARK-AGAVE-4 獨立驗證 SPARK-AGAVE-3 的協議）Stephen 認領，列為「使用者後續自行設計」，本輪不代為設計 | 避免本 SuperSystem repo 越權替來源專案做架構決策（house rule #5） |

## 尚待 Stephen 決策的事項（本 repo 整理，不代為決定）

1. 是否要推進 AIECP ↔ AERIS/MEGIS ↔ SuperBrain 的整合，以及優先順序（見 [16](16_ROADMAP_AND_ACCEPTANCE.md)）。
2. AIECP 的命名是否要統一為 "AIECP"（目前 repo 內文件全用 "AECP"）。
3. AERIS Supervision 是否要擴展為跨專案發布監督（見 [10](10_SUPERVISION_AND_EVIDENCE.md)）。
4. Voice Agent 現用的 RTX Spark 機器身分確認（是否為 SPARK-AGAVE-3/4 之一）。
5. ~~SPARK-AGAVE-4 驗證 SPARK-AGAVE-3 的具體協議設計~~ → 已由 Stephen 認領（見上表 2026-09-25），本 repo 不再列為待決事項，改追蹤於 Roadmap（見 [16](16_ROADMAP_AND_ACCEPTANCE.md)）。
