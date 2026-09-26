# 0_JN1_AI-Engineering-SuperSystem

## 建立初衷（2026-09-26 Stephen 親自澄清，優先於下面的一般說明）

Stephen 每週只有約 $20 額度的 Claude / Codex 可以用在本地七個專案的實際施工，**追不上全世界 AI 進展的即時速度**。這個 repo 存在的**根本理由**，是讓這種落差有解：

- **本 repo 的主要工作是往外看、往雲端看、往全世界最新的 AI 進展看**——持續在 cloud session 裡檢索全球最新的技能、方法、工具、架構、模型、配置，跟七個本地專案的現況對標，找出「別人已經做得更好、我可以借用或仿製」的地方。
- 具體例子（Stephen 原話）：ChatGPT 網頁版的語音輸入體驗比本地語音好、而且網頁對話不燒 API token，這種落差就是本 repo 該主動找出來、寫成建議的東西。第一筆這樣的雷達紀錄見 [Blueprint/22_GLOBAL_TECH_RADAR.md](Blueprint/22_GLOBAL_TECH_RADAR.md)。
- **這件事完全可以在 cloud session 裡做，不需要碰本地任何東西**——這正是這個 repo「只讀不寫其他 repo」規則存在的原因：它的價值不是去改本地程式碼，而是幫本地施工提供即時的外部情報與建議。
- 下面的「System-of-Systems 總藍圖」是這個初衷的**副產品**（先把現況盤點清楚，才知道要往哪裡找情報），不是取代這個初衷。

## 命名對照表（舊名稱／新名稱／功能範疇）

2026-09-26，Stephen 要求本 repo 獨立提案命名，已選定以下對照（詳見 [Blueprint/18 決策記錄](Blueprint/18_DECISION_LOG.md)、[Blueprint/10](Blueprint/10_SUPERVISION_AND_EVIDENCE.md)）。**這是命名與範疇的定義，尚未在任何來源 repo 實際改名。**

| 舊名稱 | 新名稱 | 功能範疇 |
|---|---|---|
| （無，這是新定義的整體概念） | **JN1-UOD**（JN1 Unified Operations Domain） | 三台本地機器（ULTRA-MAERA-2、SPARK-AGAVE-3、SPARK-AGAVE-4）＋ Voice Control ＋ AIECP ＋ AERIS ＋ MEGIS 的混合整體——即「本地實際在跑的一切」的總稱。範圍比 SuperBrain 大：SuperBrain＝JN1-UOD 裡的硬體層（三機），JN1-UOD＝SuperBrain（硬體）＋四個軟體系統。 |
| `0_JN1_AERIS_Supervision`（現況：只監管 AERIS 一個專案的發布快照） | **JN1-UOA**（JN1 Unified Oversight Authority） | 監管整個 JN1-UOD——即監管三台機器＋Voice Control＋AIECP＋AERIS＋MEGIS 全部。目前尚未執行改名，且要等 Stephen 理清 AERIS／AERIS Local Implementation／AERIS Supervision 三者的「Codex 債」後才會動工。 |
| `0_JN1_2AGAVE128-1MAERA64`（SuperBrain） | （不變，仍叫 SuperBrain） | 只指三台機器本身的硬體 compute fabric，是 JN1-UOD 的子集，不是同義詞。 |

## 這是什麼

在上述初衷之下，這個 repo 同時也是 Stephen（`space653000`）旗下多個獨立 AI 工程專案的 **System-of-Systems 總藍圖 / 索引 / 跨專案整合層**。

Stephen 手上已經有好幾個「各自很強但很散」的 repo：AERIS（聲學藍圖）、AERIS Local Implementation（聲學主施工）、MEGIS（機構工程）、AIECP（工程控制平面）、Offline-Local-Voice-Agent（離線語音代理人）、2AGAVE128-1MAERA64／SuperBrain（多機語音調度）、AERIS_Supervision（目前服務 AERIS，目標是擴大成全系統監管）……每個 repo 都有自己的藍圖、治理規則與施工節奏。這個 repo 的工作，是站在這些專案之上，回答「整個系統長什麼樣子」「誰擁有什麼」「哪台機器跑什麼」「資料怎麼流動」「出事了誰負責」這類跨專案問題，並且誠實記錄現實與理想架構之間的落差。

## 這不是什麼

- **不是 monorepo**：不會把其他專案的程式碼搬進來，也不會取代任何專案自己的 Blueprint。
- **不會修改其他 repo**：本 repo 對其他所有 GitHub repository 都是唯讀來源。任何「應該怎麼改」的想法，只會寫成本 repo 裡的建議文件，不會真的去對方 repo 開 PR。
- **不是新的 Control Plane**：不會取代 AIECP 的排程/佇列/審批，也不會取代 SuperBrain 的多機調度。這裡只做「跨系統的說明書」。
- **不是最新即時狀態機**：這裡的內容基於某次盤點時間點（見 [STATUS.md](STATUS.md)），各專案的即時進度仍以各自 repo 為準。

## 怎麼逛這個 repo

| 資料夾 | 內容 |
|---|---|
| [`Blueprint/`](Blueprint/) | 22 份總藍圖文件：系統邊界、機器架構、責任矩陣、資料流、Agent 角色、供應商路由、風險登錄、決策紀錄、介面契約草案、全球技術雷達……`00_MASTER_BLUEPRINT.md` 是入口，`22_GLOBAL_TECH_RADAR.md` 是本 repo 建立初衷的具體實踐。 |
| [`Registry/`](Registry/) | 機器可讀的 YAML 登錄：專案、機器、Agent 角色、供應商、跨專案介面。 |
| [`Audit/`](Audit/) | 對現有 repo 的實際盤點結果：逐專案 Inventory Card、重複能力分析、衝突分析、缺口分析。這裡的每個結論都附來源檔案。 |
| [`Architecture/`](Architecture/) | 系統圖與資料流圖（ASCII / Mermaid），給想先看圖再看字的人。 |
| [`STATUS.md`](STATUS.md) | 這個 SuperSystem repo自己的施工階段追蹤。 |
| [`CLAUDE.md`](CLAUDE.md) | 任何人或 AI 在這個 repo 工作時的house rules。 |

## 核心原則

1. **各專案保持自治**：AERIS、MEGIS、AIECP、Voice Agent、SuperBrain 各自的 Blueprint 仍是各自領域的最終真相；本 repo 只做跨專案的整合說明，衝突時記錄在 [Blueprint/17_RISK_GAP_CONFLICT_REGISTER.md](Blueprint/17_RISK_GAP_CONFLICT_REGISTER.md)，不擅自裁決。
2. **有證據才下結論**：每個「已完成」「已驗證」的說法都要能指到來源檔案；查不到來源或來源互相矛盾的一律標 `UNKNOWN` / `NOT VERIFIED`。
3. **只讀不寫其他 repo**：見上方「這不是什麼」。
