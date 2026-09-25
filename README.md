# 0_JN1_AI-Engineering-SuperSystem

## 這是什麼

這是 Stephen（`space653000`）旗下多個獨立 AI 工程專案的 **System-of-Systems 總藍圖 / 索引 / 跨專案整合層**。

Stephen 手上已經有好幾個「各自很強但很散」的 repo：AERIS（聲學工程）、MEGIS（機構工程）、AIECP（工程控制平面）、Offline-Local-Voice-Agent（離線語音代理人）、2AGAVE128-1MAERA64／SuperBrain（多機語音調度）、AERIS_Supervision（AERIS 發布監督）……每個 repo 都有自己的藍圖、治理規則與施工節奏。這個 repo 的工作，是站在這些專案之上，回答「整個系統長什麼樣子」「誰擁有什麼」「哪台機器跑什麼」「資料怎麼流動」「出事了誰負責」這類跨專案問題，並且誠實記錄現實與理想架構之間的落差。

## 這不是什麼

- **不是 monorepo**：不會把其他專案的程式碼搬進來，也不會取代任何專案自己的 Blueprint。
- **不會修改其他 repo**：本 repo 對其他所有 GitHub repository 都是唯讀來源。任何「應該怎麼改」的想法，只會寫成本 repo 裡的建議文件，不會真的去對方 repo 開 PR。
- **不是新的 Control Plane**：不會取代 AIECP 的排程/佇列/審批，也不會取代 SuperBrain 的多機調度。這裡只做「跨系統的說明書」。
- **不是最新即時狀態機**：這裡的內容基於某次盤點時間點（見 [STATUS.md](STATUS.md)），各專案的即時進度仍以各自 repo 為準。

## 怎麼逛這個 repo

| 資料夾 | 內容 |
|---|---|
| [`Blueprint/`](Blueprint/) | 19 份總藍圖文件：系統邊界、機器架構、責任矩陣、資料流、Agent 角色、供應商路由、風險登錄、決策紀錄……`00_MASTER_BLUEPRINT.md` 是入口，直接回答使用者提出的 25 個問題。 |
| [`Registry/`](Registry/) | 機器可讀的 YAML 登錄：專案、機器、Agent 角色、供應商、跨專案介面。 |
| [`Audit/`](Audit/) | 對現有 repo 的實際盤點結果：逐專案 Inventory Card、重複能力分析、衝突分析、缺口分析。這裡的每個結論都附來源檔案。 |
| [`Architecture/`](Architecture/) | 系統圖與資料流圖（ASCII / Mermaid），給想先看圖再看字的人。 |
| [`STATUS.md`](STATUS.md) | 這個 SuperSystem repo自己的施工階段追蹤。 |
| [`CLAUDE.md`](CLAUDE.md) | 任何人或 AI 在這個 repo 工作時的house rules。 |

## 核心原則

1. **各專案保持自治**：AERIS、MEGIS、AIECP、Voice Agent、SuperBrain 各自的 Blueprint 仍是各自領域的最終真相；本 repo 只做跨專案的整合說明，衝突時記錄在 [Blueprint/17_RISK_GAP_CONFLICT_REGISTER.md](Blueprint/17_RISK_GAP_CONFLICT_REGISTER.md)，不擅自裁決。
2. **有證據才下結論**：每個「已完成」「已驗證」的說法都要能指到來源檔案；查不到來源或來源互相矛盾的一律標 `UNKNOWN` / `NOT VERIFIED`。
3. **只讀不寫其他 repo**：見上方「這不是什麼」。
