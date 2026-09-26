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

### 2026-09-26 更新：正式名稱已選定

擴大後的 Supervision 正式名稱為 **JN1 Unified Oversight Authority（JN1-UOA）**，監管對象是 **JN1 Unified Operations Domain（JN1-UOD）**——即「三台本地機器＋Voice Control＋AIECP＋AERIS＋MEGIS」這個混合整體的正式名稱（見下方定義區塊與 [Blueprint/18 決策記錄](18_DECISION_LOG.md)）。兩個名稱都是本 repo 應 Stephen 要求獨立提案、由 Stephen 選定，**尚未在任何來源 repo 落地**，`0_JN1_AERIS_Supervision` 目前仍叫這個名字。

> **JN1-UOD 與 SuperBrain 的關係**：SuperBrain（`0_JN1_2AGAVE128-1MAERA64`）＝ULTRA-MAERA-2＋SPARK-AGAVE-3＋SPARK-AGAVE-4 三台機器本身（純硬體層 compute fabric，見 [Blueprint/09](09_SUPERBRAIN_COMPUTE_FABRIC.md)）。**JN1-UOD 範圍更大**：SuperBrain 這三台機器 **加上** 跑在其上的四個軟體層（Voice Control、AIECP、AERIS、MEGIS）合起來的整體運作實體。也就是 JN1-UOD = SuperBrain（硬體）＋ Voice/AIECP/AERIS/MEGIS（軟體），JN1-UOA 監管的是這整個 JN1-UOD，不只是硬體層。

### Stephen 已確認選項 B 為目標方向

Stephen 已明確裁定（見 [Blueprint/18 決策記錄](18_DECISION_LOG.md) 2026-09-26）：**選項 B——AERIS Supervision 擴大為 SuperSystem-wide 發布監督**是目標方向，監管範圍涵蓋：

- 三台本地機器（ULTRA-MAERA-2、SPARK-AGAVE-3、SPARK-AGAVE-4）
- Voice Control
- AIECP
- AERIS
- MEGIS

即上面列出的所有 Migration Risk（bundle 格式差異、權威混淆風險、私有 repo 存取範圍）都是**未來要處理的真實工程問題**，不再是「要不要做」的假設性討論，而是「怎麼做」的落地問題。

**重要邊界**：這是目標方向的確認，**不是本 SuperSystem repo 代為執行的實作**——本 repo 沒有寫入權限、也不應該去改 `0_JN1_AERIS_Supervision`。實際的改名/擴大範圍/整合各專案 bundle 格式，需要 Stephen 另開一個對該 repo 有寫入權限的 session 執行。Stephen 也表示這件事要等他先理清 AERIS／AERIS Local Implementation／AERIS Supervision 三者目前的「Codex 債」之後才會動工（見 [Blueprint/16](16_ROADMAP_AND_ACCEPTANCE.md)）。

**本 repo 能先做的準備工作**（僅供參考，等 Stephen 決定要開始時再評估是否需要）：
1. 先盤點清楚 AIECP（`RELEASE_PROVENANCE.json` + SHA256SUMS）、MEGIS（目前無正式 Release 機制）、SuperBrain（尚無 Release 概念）各自現有的證據/發布格式差異，作為未來設計統一 bundle 格式的輸入。
2. 追蹤全球「多系統統一監督/審計層」的參考架構（可作為 [22_GLOBAL_TECH_RADAR.md](22_GLOBAL_TECH_RADAR.md) 的下一個雷達方向）。

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
