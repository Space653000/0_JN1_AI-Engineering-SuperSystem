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

## 尚待 Stephen 決策的事項（本 repo 整理，不代為決定）

1. 是否要推進 AIECP ↔ AERIS/MEGIS ↔ SuperBrain 的整合，以及優先順序（見 [16](16_ROADMAP_AND_ACCEPTANCE.md)）。
2. AIECP 的命名是否要統一為 "AIECP"（目前 repo 內文件全用 "AECP"）。
3. AERIS Supervision 是否要擴展為跨專案發布監督（見 [10](10_SUPERVISION_AND_EVIDENCE.md)）。
4. Voice Agent 現用的 RTX Spark 機器身分確認（是否為 SPARK-AGAVE-3/4 之一）。
5. SPARK-AGAVE-4 驗證 SPARK-AGAVE-3 的具體協議設計（使用者原始問題23，目前完全空白）。
