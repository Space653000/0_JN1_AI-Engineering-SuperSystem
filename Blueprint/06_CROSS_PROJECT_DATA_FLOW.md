# 06 — Cross-Project Data Flow

完整圖示見 [Architecture/DATA_FLOW.md](../Architecture/DATA_FLOW.md)。本文件補充資料分級與隱私邊界。

## 已知的跨專案資料流

1. **Voice Agent → AERIS**（`ORDER.md` 檔案）：唯一已驗證存在的跨專案資料流。資料內容 UNKNOWN（本輪未讀 `ORDER.md` 的 schema），但兩專案刻意保持「不共用程式碼」，代表這是檔案層級的鬆散整合，不是 API 呼叫。
2. **AERIS Core → AERIS Local Impl**：Blueprint（WHAT）→ Implementation（HOW），透過 `blueprint_compatibility.py` 做相容性檢查。
3. **AERIS (Core+Impl) → AERIS Supervision**：人工觸發、SHA-256 驗證的單向快照，規則明文禁止反向寫入。

## 資料分級原則（各專案已各自建立，尚未統一）

| 專案 | 分級詞彙 | 私有資料的處理原則 |
|---|---|---|
| SuperBrain | `privacy_class = LOCAL_ONLY` | 外部測試資料預設不上雲，需人類決定是否脫敏後上雲（見 `.ai/BLUEPRINT.md` §4.3） |
| Voice Agent | UNTRUSTED_DATA（畫面文字） | 畫面上出現的任何文字視為不可信資料，不可當系統指令 |
| AIECP | Context Capsule（最小必要上下文） | 「Minimum necessary cloud context」是產品不變量之一 |
| MEGIS | Artifact classification | `FEASIBILITY_SPIKE` 等級資料不可當製造輸出 |

**觀察**：四個專案各自獨立發明了「不可信資料 / 私有資料 / 最小上下文」的處理原則，精神高度一致（都拒絕把外部/未驗證資料當作可信指令或可信輸出），但沒有一份跨專案文件把這些原則統一成一套資料分級標準。

## 若要打通目標資料流（Human → Voice → AIECP → Domain → Execution → Verify → Evidence → Approval → GitHub），需要新增的資料契約

1. Voice Agent 的語音意圖 → AIECP 的 `aecp.task/v1` schema 之間的轉換層（目前 `aecp.task/v1` 是給 ChatGPT Web 用的，欄位設計未考慮語音場景的即時性與模糊性）。
2. AIECP 的 Task → AERIS/MEGIS 的領域輸入格式之間的轉換層（AERIS/MEGIS 目前都沒有定義外部可呼叫的工程任務輸入 schema）。
3. SPARK-AGAVE-3 的執行結果 → SPARK-AGAVE-4 的驗證輸入格式（完全未定義，見 [Audit/GAP_ANALYSIS.md](../Audit/GAP_ANALYSIS.md)）。

以上三項目前都是 `MISSING`，詳見 [Registry/INTERFACES.yaml](../Registry/INTERFACES.yaml)。
