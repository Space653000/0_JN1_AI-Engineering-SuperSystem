# 19 — Master Progress Tracker

**建立時間**：2026-09-25（第二輪盤點，重新 clone/pull 全部 7 個來源 repo 的當前 HEAD，非沿用第一輪快照）。
**建立者**：Claude（Sonnet 5），Claude Code Remote session。
**這份文件的定位**：這是本 repo 新增的第 19 份 Blueprint 文件，是「跨專案總覽儀表板」，不取代任何一份 00–18 號文件，而是把它們的結論收斂成一張表。新增原因與命名方式記錄於 [18_DECISION_LOG.md](18_DECISION_LOG.md)。

---

## 怎麼用這份表（給 Stephen）

這份表是**活的追蹤表**，不是一次性快照。用法：

1. 你在任何一個專案（AERIS / MEGIS / AIECP / Voice Agent / SuperBrain / AERIS Supervision）完成一個階段的工作後，**回來這裡**，自己編輯下面表格對應那一列，或者直接叫 Claude Code「幫我把 XX 專案的進度更新一下」，Claude 會重新 clone/pull 那個來源 repo、重讀它自己的 STATUS/PROJECT_STATE 檔案，再回來改這張表——不會憑印象亂填。
2. 「Stephen 回報進度」欄位是給你手動勾的 checkbox，Claude 不會自己把它打勾（除非你明確說「這項我做完了，幫我打勾」）。這一欄記錄的是「你有沒有回來跟這份藍圖報告過」，不是自動偵測的工程完成度。
3. 每一列的「進度來源」都指到來源 repo 的實際檔案路徑，不是本 repo 憑空寫的百分比。如果你發現某一列跟來源 repo 現況對不上，代表這份表該更新了——直接說「重新盤點 XX」即可。
4. 這份表**不會**主動幫任何來源 repo 做架構決策；它只負責如實反映各專案自己宣稱的現況。

---

## 主表：七個來源 repo 全景

| Project | GitHub Repo | Local Folder | Machine | Domain/Purpose | Current Phase/Gate | Progress（引用來源） | Source of Truth | 最新驗證 commit/日期 | 已知重疊（解決/未解決，對照 D-xx/C-xx） | 下一個里程碑 | Stephen 回報進度 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| **AERIS Core** | [Space653000/0_JN1_AERIS](https://github.com/Space653000/0_JN1_AERIS) | `C:\0_JN1_AERIS`（`AGENTS.md`／`README.md`，已驗證） | `UNKNOWN`（repo 全文無 ULTRA-MAERA-2/SPARK-AGAVE 字樣） | 聲學工程「1 人類主管 + 100 席位」設計藍圖與治理 | 治理層 v0.7.0-blueprint.1（設計凍結，非產品完成） | A–D NOT VERIFIED；E（全面本機驗收）NOT_STARTED（`HANDOFF.md`） | `docs/AERIS_BLUEPRINT_ZH_TW.md` + `BLUEPRINT_BASELINE.md` | `64576bd`（2026-09-08）— 本輪重新 clone 驗證，**與第一輪盤點相比無變化** | 治理 Gate 機制與 AIECP/MEGIS 概念相似（合理重複，D-03） | E 全面本機驗收（無時程，`HANDOFF.md`） | - [ ] 尚未回報 |
| **AERIS Local Implementation** | [Space653000/0_JN1_AERIS_Local-computer-implementation](https://github.com/Space653000/0_JN1_AERIS_Local-computer-implementation) | `C:\0_JN1_AERIS`（與 AERIS Core 共用同一本機根目錄，`AGENTS.md` 第9行已驗證） | `UNKNOWN` | AERIS 藍圖的本機執行實作（HOW），`aeris_runtime` Python 套件 | `UNKNOWN`（repo 內未見獨立 STATUS/ACCEPTANCE 文件） | `NOT VERIFIED`（AERIS Core `HANDOFF.md` 稱相容性仍待完成） | `AGENTS.md`、`CLAUDE.md` | `44c0e50`（2026-09-10）— 本輪重新 clone 驗證，**無變化** | `UNKNOWN` | `UNKNOWN` | - [ ] 尚未回報 |
| **AERIS Supervision**（私有） | [Space653000/0_JN1_AERIS_Supervision](https://github.com/Space653000/0_JN1_AERIS_Supervision) | `UNKNOWN`（規則未提及本機路徑） | `UNKNOWN` | AERIS 發布監督：SHA-256 驗證的三檔案 bundle → 不可變快照 | 機制已建立；scope = AERIS-only | 最新快照 `S0005`（`LATEST.json`，published 2026-09-09） | `SUPERVISION_CONTRACT.md` | `d99b64f`（2026-09-10）— 本輪重新讀取，**無變化** | 若擴展為跨專案發布監督需新決策（R-03，非現況問題） | 視 Stephen 是否決定擴展範圍 | - [ ] 尚未回報 |
| **Offline-Local-Voice-Agent** | [Space653000/Offline-Local-Voice-Agent](https://github.com/Space653000/Offline-Local-Voice-Agent) | `C:\0_JN1_Offline-Local-Voice-Agent`（`.ai/STATUS.md`，已驗證） | 一台型號 "RTX Spark"（**未確認**是否為 SPARK-AGAVE-3/4，R-02） | 完全離線中文語音代理人，語音控制 Windows 電腦 | P0–P2 完成；P3~90%；P4~95%；P5 部分（vision advisory fallback）；P6~55% | 同第一輪盤點數字**無變化**，僅 HEAD 往前推進（新增 Codex workflow + P5 Segment 2 建議模式驗證），核心完成度百分比未變（`.ai/STATUS.md`） | `.ai/BLUEPRINT.md` + `.ai/STATUS.md` | `4628e30`（2026-09-24）— 本輪重新 clone，**HEAD 已從第一輪引用的 `86ff7fd` 前進，但 STATUS.md 記錄的完成度數字相同**，屬於同一輪工作的延續，非退步或新一輪重大進度 | 與 SuperBrain Phase C 語音規劃重疊，**D-02 未解決**（見下方「重疊」章節）；與 AERIS 透過 `ORDER.md` 整合（合理，非重疊風險） | P5 Executor 依視覺結果執行的完整閉環；P6 邊界情境覆蓋 | - [ ] 尚未回報 |
| **MEGIS** | [Space653000/0_JN1_MEGIS](https://github.com/Space653000/0_JN1_MEGIS) | `C:\0_JN1_MEGIS`（`execution/PROJECT_STATE.md` 第80行，已驗證；明文不得共用/依賴 AERIS、Voice Agent） | `UNKNOWN` | 機構工程生成式智慧系統，Gate-driven construction | **G4 — 模組與限制條件組合（active）**；G4-MOD-002 進行中 | **本輪盤點發現重大進度**：G0（11/11）、G1、G2、G3 全部 closed/accepted（`SO-0002`/`SO-0003`/`SO-0004`）；G4-MOD-001、G4-GRF-001 已 done；G4-MOD-002 in_progress；G4-IMP-001/REV-001/ACC-001 planned（`execution/PROJECT_STATE.md`） | `execution/PROJECT_STATE.md` + `MEGIS_Blueprint/…v3.0-claude-code.md` | `2d74b37`（2026-09-22）— **相較第一輪盤點（引用時 G1 接近完成、G2 partial、G3-G9 未開始）有實質推進**，屬於本輪盤點最大的進度變化 | 與 AERIS/Voice Agent 明確隔離聲明本身無衝突（設計選擇）；Gate/Review 機制與 AERIS constitution、AIECP roadmap 概念相似（合理重複） | G4-MOD-002（PCB/USB-C/M3 composition）→ G4-IMP-001 → G4-REV-001 → G4-ACC-001 | - [ ] 尚未回報 |
| **AIECP** | [Space653000/0_JN1_AIECP](https://github.com/Space653000/0_JN1_AIECP)（內部文件自稱 "AECP"，見 C-03） | `C:\0_JN1_AIECP`（`Blueprint/23_IMPLEMENTATION_STATUS.md` 第5行，已驗證） | `UNKNOWN` | Windows-first 本機工程控制平面（Mission/Task/Workspace/Queue/Policy/Git/CI/Evidence/Approval） | 控制平面核心 IMPLEMENTED+TESTED+CI；10 ENVIRONMENT gate + 4 OWNER-EXTERNAL gate 未結案 | 施工單 0001–0005 全部倉庫內完成並由 Claude 驗收 CLOSED；`npm test` 290/290 PASS；PR #7（`feat/control-plane-complete-loop`）**目前為 OPEN，尚未合併**（`.ai/STATUS.md`） | `.ai/STATUS.md` + `.ai/BLUEPRINT.md` + `.ai/ACCEPTANCE.md` | `71dea25`（2026-09-24，Merge PR #7）— **相較第一輪盤點，倉庫內證據持續增加（施工單全數關閉），但 ENVIRONMENT/OWNER-EXTERNAL 阻塞項數量與內容未變（仍是 10 項 + 4 類）**，package.json 版本仍為 `0.3.0` | **D-01/C-04 已裁決**：Queue/Router/Worker/Evidence/Approval 歸 AIECP 所有，SuperBrain 不重建（見 [18](18_DECISION_LOG.md)） | 在真實 Windows/ARM64 環境跑通 Codex OFFICIAL+PEGA 並行執行（ENVIRONMENT gate） | - [ ] 尚未回報 |
| **SuperBrain (2AGAVE128-1MAERA64)** | [Space653000/0_JN1_2AGAVE128-1MAERA64](https://github.com/Space653000/0_JN1_2AGAVE128-1MAERA64) | `C:\SuperBrain`（`.ai/BLUEPRINT.md` 第217/321/327/494行，已驗證；三台機器自己的安裝腳本另放 `ULTRA-MAERA-2/`、`SPARK-AGAVE-3/`、`SPARK-AGAVE-4/` 子資料夾） | **唯一定義 ULTRA-MAERA-2/SPARK-AGAVE-3/SPARK-AGAVE-4 三個機器名稱的來源**（`.ai/STATUS.md` 2026-09-25 決策） | 語音多機調度的混合式 AI 系統：Laptop（gateway/control）+ 兩台 Spark（FAST/DEEP） | P0（硬體盤點）尚未完成——**2026-09-26 新發現：可能原因是硬體尚未上市**（見下方進度來源欄） | 研究與規劃已完成；Laptop 部分盤點完成，兩台 Spark **尚未實機盤點**；機器命名已定案並新增三個機器資料夾（含安裝 SOP/腳本）（`.ai/STATUS.md`）。**2026-09-26 雷達確認**：SPARK-AGAVE-3/4 對應硬體為 Microsoft Surface RTX Spark Dev Box，**正式上市日 2026-10-07**，本文撰寫時尚未上市（見 [Blueprint/22 雷達#7](22_GLOBAL_TECH_RADAR.md)），P0 卡住很可能不是進度落後而是硬體還買不到——此為推論，待 Stephen 確認 | `.ai/BLUEPRINT.md` + `.ai/STATUS.md` + `.ai/ACCEPTANCE.md` | `c4e0f75`（2026-09-25 20:05，Merge PR #2）— **本輪重新 clone 時 HEAD 比原始盤點時間更新（同日），內容與第一輪引用的 2026-09-25 決策紀錄一致，屬於同一輪工作的延續**，P0 完成度未變 | **D-01/C-04 已裁決**（見上）；**D-02 未解決**：SuperBrain Phase C 語音規劃仍完全未提及 Voice Agent 既有的離線語音成果（本輪 grep 交叉驗證，零命中） | 完成三機硬體盤點 → P1 影片同款體驗 → P2 隔離網路 | - [ ] 尚未回報 |

---

## 重疊（Overlap）現況總表 — 只反映已裁決 vs 仍未解決

| ID | 摘要 | 現況 | 本輪重新確認方式 |
|---|---|---|---|
| **D-01 / C-04** | AIECP 與 SuperBrain 的 Queue/Router/Worker/Evidence/Approval「控制平面原語」重疊 | ✅ **已裁決**（2026-09-25，Stephen）：歸 AIECP 所有，SuperBrain 提升為跨機資源調度/統籌規劃層，不重建這組原語 | 重新讀取 AIECP `.ai/STATUS.md` 與 SuperBrain `.ai/STATUS.md`：兩者現況描述皆與裁決方向一致（AIECP 持續深化控制平面；SuperBrain 仍停留在 P0 盤點階段，未新增任何 Queue/Router/Worker 實作） |
| **D-02** | Voice Agent 已驗證的離線語音能力 與 SuperBrain 規劃中 Phase C 語音能力重疊 | ⚠️ **仍未解決**——不在 2026-09-25 決策範圍內 | 本輪對 SuperBrain 全文（含新增的 `ULTRA-MAERA-2/`、`SPARK-AGAVE-3/`、`SPARK-AGAVE-4/` 資料夾）與 Voice Agent 全文重新 grep 交叉比對，**零命中**——兩專案仍互不知道對方存在，需要 Stephen 或兩專案自行決定是否整合 |
| **D-03** | Evidence/風險分級詞彙各專案不同但精神一致 | 合理重複，不需處理 | 無變化 |
| **C-01** | 使用者「confirmed architecture」（ULTRA-MAERA-2 承載四個工程 repo）未被任何來源 repo 實作 | ⚠️ **仍未解決** | 本輪對全部 6 個非 SuperBrain repo 重新 grep `ULTRA-MAERA\|SPARK-AGAVE`，**零命中**，與第一輪盤點結論一致 |
| **C-02** | Voice Agent 實際整合對象是 AERIS（`ORDER.md`），不是 AIECP | ⚠️ **仍未解決**（現況如此，非需要修復的錯誤，只是與使用者目標架構有落差） | 無變化 |
| **C-03** | Repo 名 AIECP vs 內部自稱 AECP | 低嚴重度命名問題，未變 | 無變化 |
| **G-01/G-02/G-03** | Voice Agent↔AIECP、AIECP↔AERIS/MEGIS、AIECP↔SuperBrain 介面契約缺失 | ⚠️ **仍未解決** | 本輪未見任一來源 repo 新增對應介面文件或程式碼 |
| **G-04** | SPARK-AGAVE-4 獨立驗證 SPARK-AGAVE-3 協議 | 🟡 **Stephen 已認領，待其自行設計**，非本輪阻塞 | 無變化，本 repo 不代為設計 |

---

## 本輪（第二輪）重新盤點 vs 第一輪盤點：逐專案差異

| 專案 | 第一輪盤點結論 | 第二輪（本次）重新 clone/pull 結果 | 是否有實質變化 |
|---|---|---|---|
| AERIS Core | tag `v0.7.0-blueprint.1`，A-D NOT VERIFIED，E NOT_STARTED | HEAD 仍為同一批次（`64576bd`，2026-09-08），tag 不變 | **無變化** |
| AERIS Local Impl | 目錄結構盤點，未深讀 | HEAD `44c0e50`（2026-09-10）不變 | **無變化** |
| AERIS Supervision | 快照機制存在，`LATEST.json` 未讀內容 | 本輪讀取 `LATEST.json`：最新快照 `S0005`（2026-09-09） | **無變化**（僅補齊第一輪未讀的細節） |
| Voice Agent | HEAD `86ff7fd`，P0-P2 100%、P3~90%、P4~95%、P5 partial、P6~55% | HEAD 前進至 `4628e30`（新增 Codex workflow + P5 Segment 2 advisory fallback），但 STATUS.md 記錄的完成度數字**相同** | **HEAD 前進但完成度數字不變**（同一輪工作的延續，非新進度） |
| **MEGIS** | G0 done，G1 mostly done，G2 partial，G3–G9 not started | **G0–G3 全部 closed/accepted，G4 已啟動且 G4-MOD-001/G4-GRF-001 已 done** | **✅ 有實質推進，本輪盤點最大變化** |
| AIECP | 控制平面核心 IMPLEMENTED+TESTED+CI，10 ENVIRONMENT + 4 OWNER-EXTERNAL gate 未結案，npm test 290/290 | HEAD 前進至 PR #7（`control-plane-complete-loop`，OPEN 未合併），npm test 仍 290/290，ENVIRONMENT/OWNER-EXTERNAL gate 數量與清單不變 | **無實質變化**（倉庫內證據持續累積，但外部阻塞項未減少） |
| SuperBrain | 研究規劃完成，P0 未開始，2026-09-25 機器命名定案 | HEAD 前進至 PR #2（2026-09-25 20:05），內容與第一輪引用的同一輪決策一致，P0 完成度不變 | **無實質變化**（同一輪工作的延續） |

---

## 誠實揭露：這份表無法驗證的部分

1. **所有「Machine」欄位除 SuperBrain 自身定義外都是 `UNKNOWN`**——本 session 只能讀取 GitHub 上的內容，**完全沒有 Stephen 實際 Windows 機器的本機檔案系統存取權限**。任何關於「哪個資料夾實際裝在哪台機器上」的問題，凡是沒有在來源 repo 文件裡白紙黑字寫出來的，一律標記 `UNKNOWN / NOT VERIFIED (no local filesystem access from this session)`，不臆測。
2. Voice Agent 目前運行的「RTX Spark」是否為 SPARK-AGAVE-3 或 SPARK-AGAVE-4 其中一台，**沒有任何 repo 內容能回答這個問題**（R-02，未解決）。
3. AERIS Local Implementation 與 AERIS Supervision 的內部驗收/CI 細節，本輪仍未逐行深讀（維持第一輪盤點時記錄的 `NOT VERIFIED` 標記），只確認了 HEAD commit 與是否有新內容。

---

## 相關文件

- 詳細來源引用見 [Audit/REPOSITORY_INVENTORY.md](../Audit/REPOSITORY_INVENTORY.md)（含本輪新增的「第二輪重新驗證」章節）
- 重疊分析原始表格見 [Audit/DUPLICATION_ANALYSIS.md](../Audit/DUPLICATION_ANALYSIS.md)
- 風險/落差/衝突完整清單見 [17_RISK_GAP_CONFLICT_REGISTER.md](17_RISK_GAP_CONFLICT_REGISTER.md)
- 機器架構細節見 [03_MACHINE_ARCHITECTURE.md](03_MACHINE_ARCHITECTURE.md)
- 決策紀錄見 [18_DECISION_LOG.md](18_DECISION_LOG.md)
