# 10 — Supervision and Evidence

## AERIS Supervision：現況 / 可能的未來 / 遷移風險 / 建議邊界

### Current State（現況，來源：`SUPERVISION_CONTRACT.md`）

- 唯一職責：把已 SHA-256 驗證的 AERIS ISI V6 三檔案 bundle 發布為不可變的歷史快照。
- 人類手動觸發 `AERIS_Supervision_Publisher.bat` 是每次發布的授權邊界（規則1）。
- 明文禁止修改 Blueprint 或 Implementation repository（規則5）。
- 明文聲明「Publication 不代表 engineering PASS、release approval 或 Human acceptance」（規則6）。
- ChatGPT web supervisor 仍須獨立核對 Blueprint、Implementation、Supervision、local/runtime identity、PR state 與 CI（規則7）。
- 「NO EVIDENCE = NOT DONE」（規則12）。
- **範圍：僅服務 AERIS 一個專案。**

### Possible Future State（可能的未來方向，供評估）

**選項 A — 維持 AERIS-only**：Supervision 機制保持現狀，其他專案（MEGIS、AIECP、SuperBrain）各自建立自己的發布監督機制（或沿用自己現有的 CI/Release 流程，例如 AIECP 已有的 SHA256SUMS + RELEASE_PROVENANCE.json）。

**選項 B — 擴展為 SuperSystem-wide 發布監督**：由這套機制（或仿照其設計原則的新機制）統一發布所有專案的快照，提供跨專案的「誰在什麼時候發布了什麼」的單一時間軸。

### Migration Risk（若選擇選項 B）

1. **職責邊界模糊化風險**：目前 `SUPERVISION_CONTRACT.md` 的簡潔性（12 條規則）部分來自於範圍窄（只服務一個專案的一種 bundle 格式）。擴展到多專案，勢必要處理不同專案的 bundle 格式差異（AIECP 的 Release 是 Electron 安裝檔+SHA256SUMS；MEGIS 目前沒有正式 Release 機制；SuperBrain 尚未有 Release 概念）。
2. **權威混淆風險**：規則7 要求「supervisor 獨立核對」，如果一個機制要橫跨六個專案的 Blueprint/Implementation 版本核對，其查證複雜度會大幅增加，且可能被誤認為「跨專案的工程驗收權威」（實際上規則6 已經明文排除這個誤解，但擴大範圍後更容易被誤讀）。
3. **私有 repo 存取風險**：本次盤點已確認 AERIS Supervision 是私有 repo；若要擴展服務其他專案，需要重新評估存取範圍與權限模型。

### Recommended Boundary（本 repo 建議，非強制）

維持**選項 A（AERIS-only）**作為短期方向，理由：
- 目前沒有任何其他來源 repo 表達過需要這種發布監督機制的需求（MEGIS、AIECP、SuperBrain 都有自己足夠的證據/CI/Release 機制）。
- 擴大範圍是一個需要獨立、明確決策的架構變更，不應該因為本次盤點順帶建議就被默默採納。

若 Stephen 未來決定要做選項 B，建議先在這個 SuperSystem repo 裡（而不是直接修改 AERIS Supervision 本身）寫一份獨立的擴展提案文件，經過明確評估後，再交給 AERIS Supervision 專案自己的治理流程決定是否採納。

---

## 跨專案 Evidence 詞彙對照（供理解，非強制統一）

| 專案 | 證據等級詞彙 | 核心原則 |
|---|---|---|
| AIECP | STATIC / TESTED / CI / ENVIRONMENT / OWNER-EXTERNAL | 下層證據不能冒充上層；「Blueprint 存在 ≠ Runtime 完成」 |
| AERIS | PASS（僅限特定 commit 與文件範圍）/ REVIEW_PENDING / NOT VERIFIED | 「CI 文件檢查不能證明產品完成」 |
| AERIS Supervision | SHA-256 verified bundle / Publication ≠ PASS | 「NO EVIDENCE = NOT DONE」 |
| SuperBrain | 指令輸出 / 檔案 / log / 截圖 / audit event | 「Agent 自己說完成了不算數」 |
| MEGIS | classification（`FEASIBILITY_SPIKE`/`PROTOTYPE`/…）+ maturity | 「不會用既有 V2 證據冒充 V3 合規」 |
| Voice Agent | `progress/p*/REPORT.md` + 量測 JSON | 已知限制誠實記錄，不隱藏未達標項目 |

**共同精神**：所有六個專案都拒絕「模型自稱完成」作為證據，都要求可重現/可核對的具體產出。這是整個生態系少數已經自然收斂的一致文化，值得在 SuperSystem 層級明文肯定，而不需要強迫統一詞彙。
