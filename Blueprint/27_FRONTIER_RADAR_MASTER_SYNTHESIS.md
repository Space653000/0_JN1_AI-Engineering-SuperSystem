# 27 — Frontier Radar Master Synthesis

> **2026-09-27 更新**：Stephen 明確決定**暫緩 AIECP Trusted Signing 申請**（個人開發考量，不是技術問題），並指定**現在要聚焦做**的是另外四件事：AERIS 能力地圖自查、MEGIS 能力地圖自查、AIECP 憑證管理對照、SPARK 硬體選型準備。這四項已重新排序、加上「去哪裡做／怎麼做／預期產出」的具體執行說明，見下方新增的〈2026-09-27 聚焦執行清單〉。原本的精華清單與完整分級表保留在後面當完整記錄，未刪減內容，只是不再是首要閱讀順序。

> **這是本 repo「雷達系列」的總結案（closeout）**，回應 Stephen 2026-09-27 的明確指示（見 [18_DECISION_LOG.md](18_DECISION_LOG.md)）：不要再逐批問「這批可以嗎」，改成自己評估價值、一次彙整回報。這份文件是那次彙整的產物，也是本輪任務的最終交付——之後不會再有第二輪、第三輪的「還要不要繼續」。
>
> 涵蓋範圍：[22_GLOBAL_TECH_RADAR.md](22_GLOBAL_TECH_RADAR.md)（41 條 bottom-up 工具對照）＋ [26_VISION_DRIVEN_FRONTIER_RADAR.md](26_VISION_DRIVEN_FRONTIER_RADAR.md)（22 節 top-down 願景對照，含本輪新增的 §22）——累計橫跨兩輪任務、約 100 個候選主題篩選檢索後，共 **63 條有實質檢索內容的發現**（`0_JN1_AERIS_Supervision` 因私有 repo 無法存取，標記 `NOT VERIFIED`，不計入 63 條）。每一條的原始出處、完整檢索來源、詳細建議，都留在 22/26 裡，這份文件**不重新引用新的外部來源**，只做「這 63 條裡，哪些真的值得你花時間」的自我評分與濃縮。

---

## 一句話總結

這兩輪雷達檢索最終告訴 Stephen 的是：**SuperSystem 七個專案的治理理念（Evidence-before-DONE、人類最終權限、角色分離、本地優先）在 2026 年全球前沿裡站得住腳，甚至比很多業界案例更嚴謹**——真正的落差幾乎都不是「方向錯了」，而是「還沒做完」或「規模/時機還沒到」；63 條發現裡，只有 8 條是現在就該花時間處理的具體行動，其餘多數是「記下來、等前置條件到位（硬體上市、介面真的動工、專案自己盤點完）再做」的正確方向，另一批則是誠實查證後判定「這條前沿是給企業規模用的，單人規模不用追」。

---

## 2026-09-27 聚焦執行清單（Stephen 指定，取代原本的「先做§20」順序）

> Stephen 決定：**AIECP Trusted Signing 暫緩**（個人開發，不想現在處理行政流程），改成優先做以下四件事。每件都附「去哪裡做／怎麼做／預期產出」，是可以直接照著執行的步驟，不是抽象建議。排序＝建議執行順序，理由是前兩項（AERIS/MEGIS 自查）互相獨立、隨時可做；第三項（AIECP憑證）需要先讀一次現有程式碼；第四項（SPARK選型）現在只能做「準備」，實測要等硬體到貨。

### 第1優先｜AERIS 能力地圖自查（對應願景 [§9](26_VISION_DRIVEN_FRONTIER_RADAR.md)）

**去哪裡做**：
- 對照來源（外部，四份交集）：INCE（Institute of Noise Control Engineering）噪音控制工程師認證考照範圍、ASA（Acoustical Society of America）14個技術委員會清單（官網 Technical Committees 頁面）、AES（Audio Engineering Society）技術委員會範疇、IEC 61094 麥克風校準/陣列標準文件目錄。
- 被檢查對象（本地）：`0_JN1_AERIS` repo 的 100 席位角色清單（`role_specs.py`、`role_acceptance.py`，見 AERIS Local Implementation 的 `aeris_runtime` 套件）+ `docs/AERIS_BLUEPRINT_ZH_TW.md` 對席位的描述段落。

**怎麼做**：
1. 先把 INCE/ASA/AES/IEC 61094 這四份清單的類別名稱抄成一份簡單的 Excel 或文字列表（不用查到很深，抓「大分類」層級即可，例如：噪音控制、心理聲學、揚聲器換能器設計、麥克風換能器設計、MEMS 麥克風、陣列波束成形、房間聲學、電聲量測、法規合規…）。
2. 拿這份列表逐一去對照 AERIS 現有的 100 席位清單，每個大分類標記「有對應席位／沒有對應席位／不確定」。
3. **特別檢查兩點**（Stephen 之前已確認 AERIS 範疇含揚聲器＋麥克風）：麥克風校準是不是有獨立席位，還是被籠統歸進「聲學量測」；陣列波束成形是不是有獨立席位，還是完全沒被涵蓋。

**預期產出**：一份「AERIS 100席位 vs 四份業界清單」的缺口對照表。這個自查結果屬於 AERIS 自己專案的工作範圍，執行時建議直接在 `0_JN1_AERIS` repo（本 SuperSystem repo 不能代為修改），完成後可以回來這裡告訴 Claude，我幫你更新 [26 §9](26_VISION_DRIVEN_FRONTIER_RADAR.md) 記錄自查結果。

### 第2優先｜MEGIS 能力地圖自查（對應願景 [§12](26_VISION_DRIVEN_FRONTIER_RADAR.md)）

**去哪裡做**：
- 對照來源（外部，比 AERIS 好找，是官方骨架不是拼湊）：ABET（Accreditation Board for Engineering and Technology）Engineering Accreditation Commission 的《Criteria for Accrediting Engineering Programs》裡 Mechanical Engineering Program Criteria 章節（ABET 官網 Accreditation → Accreditation Criteria 可下載當年度 PDF）；ASME《Vision 2030》報告（搜尋 "ASME Vision 2030" 即可找到 PDF，內容是訪談1,470位業界人士的調查結果）。
- 被檢查對象（本地）：`0_JN1_MEGIS` repo 的 `execution/PROJECT_STATE.md`（G0-G9各Gate範圍描述）+ `MEGIS_Blueprint/…v3.0-claude-code.md`。

**怎麼做**：
1. 讀 ABET 準則裡 Mechanical Engineering 那段，確認它明確要求「熱力系統」（thermal systems）與「機械系統」（mechanical systems）兩軌都要涵蓋。
2. 對照 MEGIS 現有 G0-G9：目前已知驗證範圍是治具幾何（G2）跟聲學/機器人薄切片（G7/G8）——**這些明顯屬於機械系統這軌，逐一確認有沒有任何 Gate 涵蓋熱力系統**（熱傳、流體、能量轉換這類）。
3. 讀 ASME Vision 2030 的調查結論（畢業生缺「實務經驗、溝通能力、系統性思維」），對照 MEGIS 的 Gate 審查機制有沒有涵蓋「系統性思維」這種跨模組整合考量，還是只逐一驗證單一零件。

**預期產出**：一份「MEGIS G0-G9 vs ABET雙軌+ASME調查」缺口清單，特別標出「有沒有熱力系統軌」這個具體是非題的答案。執行位置同樣是 `0_JN1_MEGIS` repo 自己的工作範圍，完成後回來這裡我幫你更新 [26 §12](26_VISION_DRIVEN_FRONTIER_RADAR.md)。

### 第3優先｜AIECP 憑證管理對照（對應雷達 [#13](22_GLOBAL_TECH_RADAR.md)、[#32](22_GLOBAL_TECH_RADAR.md)）

**去哪裡做**：
- 被檢查對象：`0_JN1_AIECP` repo 裡 Codex OFFICIAL / Codex PEGA 兩個 worker 目前怎麼拿到 GitHub token、API key 的設定方式（通常會在 `.env`、環境變數設定腳本、或 CI 設定檔裡）。
- 對照原則（已查過，見雷達#13/#32）：憑證應該**短效、限定任務範圍、由獨立的 broker 簽發、AI 推理引擎本身不直接持有原始憑證**。

**怎麼做**：
1. 找出 AIECP 現在憑證/token 儲存與取用的實際位置（跟著程式碼找 `os.environ`、`.env`、GitHub Secrets 設定）。
2. 逐條對照上面四個原則：現在是不是長效憑證直接寫死？Codex worker 是不是直接讀到原始 token，而不是透過 broker 拿臨時憑證？
3. 若想低成本先做 PoC：可以評估 **Infisical**（雷達#32 提到的輕量方案）——先不用上 SPIFFE/SPIRE 那種重量級架構，Infisical 對單人專案來說門檻低很多。

**預期產出**：一份「現況 vs 四項原則」對照表，標出哪幾項現在不符合，作為之後要不要導入 Infisical 的判斷依據。執行位置是 `0_JN1_AIECP` repo。

### 第4優先｜SPARK 硬體選型準備（對應雷達 [#2](22_GLOBAL_TECH_RADAR.md)/[#19](22_GLOBAL_TECH_RADAR.md)/[#9](22_GLOBAL_TECH_RADAR.md)，硬體 2026-10-07 才上市，現在只能做「準備」）

**去哪裡做**：`Blueprint/24_SPARK_HARDWARE_READINESS.md`（本 repo 既有檔案，已經有到貨前準備清單）。

**怎麼做（現在，硬體還沒到貨前就能做）**：
1. 把雷達#2 的選型原則（優先看 MoE 架構如 Qwen3-Coder-Next/Laguna S 2.1，不是看參數量）跟雷達#19 的硬性篩選條件（4-bit量化後實際佔用要算清楚，DeepSeek-V3.2/GLM-5.2 這類塞不進128GB）整理成一份「到貨後第一天就要跑的選型 checklist」。
2. 把雷達#9 的跨機推理選項（EXO／vLLM+Ray）也放進同一份 checklist，標註「要先測 2.5GbE 頻寬夠不夠，這是已知風險」。
3. 這份 checklist 直接補進 `Blueprint/24_SPARK_HARDWARE_READINESS.md`，跟既有的硬體安裝 SOP 放在一起，這樣硬體到貨那天你只要打開一份文件照做。

**預期產出**：`Blueprint/24` 新增一節「到貨後第一天選型 checklist」。這一步本 SuperSystem repo 現在就可以幫你把 checklist 寫好（不用等硬體），實際跑 benchmark 才需要等 10/7。

---

## 精華清單：63 條裡最值得你現在花時間看的 16 條

> Stephen 指定要的「一口氣」清單——不是 63 條的縮寫版，是自我評分後真正篩出來、值得優先讀的部分。每條保留「現有實作 / 既有雷達可拿改良 / 白話建議與取捨 / 前沿還有多遠」四欄精神，壓縮成一行，完整內容看括號裡的連結。

### 🔴 現在就做（8 條，全部具體、低成本、不需要等任何前置條件）

| # | 發現 | 一行摘要 |
|---|---|---|
| 1 | 雷達#34 [Prompt Caching 降價75%](22_GLOBAL_TECH_RADAR.md) | 現有：每次呼叫都當新輸入計費／可改良：2026-09-01起cache read降到$0.25/M token／建議：檢查AIECP、本repo自己重複讀長CLAUDE.md/BLUEPRINT.md的場景有沒有命中快取／前沿距離：零，是現在就能查的設定問題，不是要追的技術前沿 |
| 2 | 願景§20 [AIECP Trusted Signing 申請](26_VISION_DRIVEN_FRONTIER_RADAR.md) | **2026-09-27 Stephen 決定暫緩**（個人開發考量，非技術問題）。現有：Windows/Store 簽章身分卡住是 OWNER-EXTERNAL gate／可改良：查到 Microsoft Trusted Signing $9.99/月、EV Sole Proprietor 免公司、Store 註冊費已取消／備註：資訊已查清楚保留在案，Stephen 決定何時要處理都可以隨時回來看這條，不會過期 |
| 3 | 願景§14 [SuperBrain Bastion Host 資安習慣](26_VISION_DRIVEN_FRONTIER_RADAR.md) | 現有：Laptop 是唯一連網 Gateway，但沒有刻意精簡/稽核／可改良：這正是成熟30年的Bastion Host模式，只差沒套用它的兩條慣例／建議：精簡 Laptop 對外服務、對 Laptop→Spark 的每個指令做集中紀錄，不花錢不裝新工具／前沿距離：前沿早就有答案，只是沒被套用 |
| 4 | 雷達#24 [CadQuery AI 生態系（MCP copilot）](22_GLOBAL_TECH_RADAR.md) | 現有：MEGIS 已用 CadQuery，G4 幾何草案階段是人工／可改良：CadQuery 生態系自己長出暴露 MCP server 的 AI copilot，跟本 SuperSystem 技術棧天然相容／建議：優先評估這個而不是 Zoo.dev/AdamCAD 這類獨立商業產品／前沿距離：現成工具已存在，可以直接下載試 |
| 5 | 雷達#16 [SLSA Artifact Attestations](22_GLOBAL_TECH_RADAR.md) | 現有：AIECP 只有 SHA256SUMS + RELEASE_PROVENANCE.json／可改良：GitHub Artifact Attestations 幾行 YAML 就能加官方可驗證建置證明／建議：低成本先上這個，要衝 SLSA Level 3 再上 slsa-github-generator／前沿距離：幾行設定的距離 |
| 6 | 雷達#25 [MCP 2026-07-28 重大改版](22_GLOBAL_TECH_RADAR.md) | 現有：本 session／AIECP 的整合都建立在 MCP 協定上／可改良：這次改版是有狀態→無狀態的斷點升級，不是版本號小補丁／建議：立即確認現有任何 MCP 整合假設是否還成立，這是必須做的相容性檢查／前沿距離：不是要不要追前沿，是不確認就可能已經壞了 |
| 7 | 願景§9 [AERIS 100席位 vs 聲學工程能力地圖交集自查](26_VISION_DRIVEN_FRONTIER_RADAR.md) | 現有：AERIS 100 席位清單內容本身 `UNKNOWN`／可改良：查到 INCE+ASA+AES+IEC 61094 交集可以自建一份對照草稿／建議：Stephen 拿自己的 100 席位清單，對照這份草稿看揚聲器/麥克風兩條線有沒有漏格／前沿距離：全世界本來就沒有官方清單，方法已備好，剩下是核對動作 |
| 8 | 願景§12 [MEGIS Gate架構 vs ABET/ASME Vision 2030 交集自查](26_VISION_DRIVEN_FRONTIER_RADAR.md) | 現有：G0-G9 覆蓋範圍未知是否含熱力系統／可改良：ABET機械工程準則+ASME Vision 2030是全球公信力最高的兩份骨架／建議：核對 G0-G9 是否只做機械系統這軌、有沒有涵蓋熱力系統這軌／前沿距離：比 AERIS 那份更具體可執行，值得優先排 |

### 🟡 記下來、時機到了再做（挑 6 條最重要的，完整 42 條見下方分級表）

| # | 發現 | 一行摘要 |
|---|---|---|
| 9 | 雷達#7/#31 [硬體確認：Surface RTX Spark Dev Box + ARM64](22_GLOBAL_TECH_RADAR.md) | 128GB、ARM64架構、WSL3是正確目標——但要等 **2026-10-07 上市** 才能實測，這解釋了SuperBrain P0卡住的真正原因 |
| 10 | 雷達#19 [本地LLM容量修正](22_GLOBAL_TECH_RADAR.md) | DeepSeek-V3.2/GLM-5.2 量化後都超過128GB，等硬體到手做選型時**必須**先查量化後實際佔用，不能只看MoE/dense |
| 11 | 雷達#10/#15/#36 + 願景§18 [G-04獨立驗證的警示與素材](22_GLOBAL_TECH_RADAR.md) | LLM Judge有32.4%分歧率、主流benchmark被證實有漏洞、mutation testing/確定性驗證優於LLM debate——等Stephen自己設計G-04時直接用 |
| 12 | 願景§10 [AIECP借用MDO詞彙設計G-02](26_VISION_DRIVEN_FRONTIER_RADAR.md) | MEGIS↔AERIS協調問題航太業界已經用MDO處理三十年，OpenMDAO的資料流設計可參考——等G-02真的要設計時用 |
| 13 | 願景§16 [G-01/G-02/G-03借用A2A schema](26_VISION_DRIVEN_FRONTIER_RADAR.md) | A2A協定逐欄位對照發現AIECP現有草案在驗收標準/權限範圍上已比A2A標準更嚴謹，只需補幾個欄位——等正式設計schema時用 |
| 14 | 願景§22 [Zero-Trust Agent Identity跨系統信任邊界](26_VISION_DRIVEN_FRONTIER_RADAR.md)（本輪新增） | G-01/G-02/G-03若真的落地，每次跨repo呼叫該用短效、限定單一任務的憑證——但要等這些介面本身先存在才輪得到討論這層 |

### ⚪ 已查過，暫不追（挑 2 條最有代表性的，完整清單見下方分級表）

| # | 發現 | 一行摘要 |
|---|---|---|
| 15 | 願景§21 [Provider路由/企業級FinOps不適用單人規模](26_VISION_DRIVEN_FRONTIER_RADAR.md) | Bandit-based路由、企業FinOps都是為千萬美元級支出設計，Stephen目前的靜態golden-set門檻+檢查cache命中率已經是這個規模該有的解法 |
| 16 | 雷達#38 + 願景§5 [SuperBrain艦隊管理/Chaos Engineering過早](22_GLOBAL_TECH_RADAR.md) | 3台機器連「10-20台用人工管理」這個業界最低門檻都還沒到，K3s/Kubernetes式編排、故障注入測試都是規模到了才需要 |

---

## 自我評分後的完整行動分級表（63 條全覆蓋）

> 分級標準：🔴 具體、低成本、高影響、本週/本月該做；🟡 方向正確但要等時機/前置條件；⚪ 查過了，結論是不適合現在的規模或已有更好的既有做法。每列附一句話理由，不逐條展開細節（細節見雷達#/願景§原文連結）。

### 🔴 現在就該處理（8 條）— 已完整列在上方精華清單第1-8條，不重複列出

### 🟡 記下來，時機到了再做（42 條）

**依賴「SPARK-AGAVE-3/4 於 2026-10-07 上市、實機到手」（8 條）**

| 發現 | 一行理由 |
|---|---|
| 雷達#2 本地LLM選型MoE架構 | 需要實機benchmark才能驗證頻寬瓶頸假設 |
| 雷達#7 硬體規格確認 | 未上市，只能先記錄官方公開規格 |
| 雷達#9 跨機分散式推理EXO/vLLM+Ray | 2.5GbE頻寬是否足夠要實測 |
| 雷達#19 本地LLM容量修正 | 選型時的硬性篩選條件，等硬體到手選型階段用 |
| 雷達#20 EAGLE-3推測解碼 | 要等vLLM/SGLang部署時一併評估 |
| 雷達#30 MXC SDK沙盒隔離 | 早期預覽，等實機到手做Windows/ARM64驗證 |
| 雷達#31 WSL3+ARM64 CI | WSL3剛預覽，第一個相容性測試排在硬體到手後 |
| 雷達#39 Aion 1.0裝置端SLM | 等SuperBrain任務分層需求出現 |

**依賴「Stephen自行設計G-04」（4 條，明確排除本repo代為設計）**

| 發現 | 一行理由 |
|---|---|
| 雷達#10 LLM Judge風險警示 | 給Stephen設計G-04時的參考警示，非現在要做的事 |
| 雷達#15 具體開源驗證實作範例 | 同上，Stephen認領後才用得上 |
| 雷達#36 Mutation Testing等具體技術 | 同上 |
| 願景§18 GSN形狀驗證報告參考範例 | 明確標註非協定設計，只是參考起點 |

**依賴「各專案自己先盤點/擴充/動工」（15 條）**

| 發現 | 一行理由 |
|---|---|
| 雷達#1 語音架構端到端演進 | 需先確認Voice Agent現有ASR引擎名稱才能A/B |
| 雷達#8 開源喚醒詞 | 同上，需先有基準才能比較 |
| 雷達#12 in-CAD即時DFM | 等MEGIS G4擴充建模階段時評估 |
| 雷達#13 憑證broker短效簽發 | 等AIECP規模擴大到需要時 |
| 雷達#17 中文ASR SenseVoice/FunASR | 值得測但非核心功能瓶頸 |
| 雷達#18 開源TTS CosyVoice3/F5-TTS | 同上 |
| 雷達#22 GD&T自動標註工具 | 等MEGIS G4擴充審查階段時評估 |
| 雷達#26 A2A協定+AGENTS.md | 等G-01/G-02/G-03正式設計時採用 |
| 雷達#27 SBOM+OpenSSF Scorecard | 延伸#16，做完#16後再排 |
| 雷達#28 LangGraph/CrewAI/Temporal.io | AIECP現有架構還不需要換，等真的遇到失敗恢復問題再評估 |
| 雷達#32 Infisical Vault/SPIFFE | 值得評估但非本週優先，等憑證問題真的浮現 |
| 雷達#33 Langfuse vs Phoenix | 等JN1-UOA真正動工才需要選型 |
| 雷達#35 LiteLLM+RouteLLM | 需要架構投入，非本週最優先 |
| 雷達#37 Pact Contract Testing | 等G-01/G-02真的要設計介面時用 |
| 願景§2 MEGIS拓樸優化NCTO | 等MEGIS真的做拓樸優化Gate時參考 |

**依賴「JN1-UOA/Public Portal這類尚未動工的基礎設施」（7 條）**

| 發現 | 一行理由 |
|---|---|
| 雷達#5 SLSA對齊 | 概念層，已被#16具體化為行動項，這裡是延續觀察 |
| 雷達#6 OpenTelemetry GenAI監管標準 | JN1-UOA尚未動工，等基礎做完再選格式 |
| 願景§3 AIECP治理框架CAVA/Proof of Execution | 研究階段工具，等AIECP下次重大版本演進時評估 |
| 願景§6 AERIS Local Impl的Proof of Execution參考 | 低優先，方向正確，等GATE-06自動化需求出現 |
| 願景§8 JN1-UOA監管架構（核能韌性治理） | JN1-UOA只是命名選定，離真正設計架構還很早 |
| 願景§13 JN1-UOA Assurance Case(GSN/CAE) | 同上，等JN1-UOA真正設計呈現層時採用 |
| 願景§17 Public Portal安全公開投影 | G-05明確低優先，等基礎做完 |

**依賴「等Voice Agent核心迴圈先穩定」（2 條）**

| 發現 | 一行理由 |
|---|---|
| 願景§4 Voice Agent雙向溝通 | 等P0-P4核心語音控制迴圈穩定後才該評估 |
| 願景§11 Voice Agent雙向澄清對話CLARA | 同上，Stephen已接受方向但明確排在核心功能之後 |

**其他明確有前置條件的（6 條）**

| 發現 | 一行理由 |
|---|---|
| 願景§10 AIECP借用MDO詞彙 | 精華清單已列，等G-02真的要設計時用 |
| 願景§15 AERIS可重現模擬RO-Crate/GNU Guix | AERIS實際用哪套模擬工具目前`UNKNOWN`，需先確認 |
| 願景§16 G-01/02/03借用A2A schema | 精華清單已列，等正式設計schema時用 |
| 願景§19 R-01單一連網閘道對策 | 已有結論「不需要蓋第二台Gateway」，USB備援WAN是可選項，非本週急迫 |
| 願景§22 Zero-Trust Agent Identity | 精華清單已列，等G-01/02/03介面本身先存在 |
| 雷達#11 Codex worktree Windows ARM64缺口 | 已被雷達#30/#31更新取代，這裡僅存脈絡參考 |

### ⚪ 已知道，暫不追，除非情況改變（13 條）

| 發現 | 一行理由 |
|---|---|
| 雷達#3 MEGIS Text-to-CAD（Zoo.dev/AdamCAD） | 已被#24（CadQuery自己的AI生態系）取代為更貼合現有技術棧的選項 |
| 雷達#4 AERIS聲學模擬工具（Pyroomacoustics/Acoular） | 揚聲器設計本身沒有找到比現有工程模擬工具更好的AI專用替代品 |
| 雷達#11（原始版本） Windows ARM64沙盒隔離缺口 | 業界資料稀少的結論已被#30/#31更新（MXC SDK/WSL3已出現），不再是無解問題 |
| 雷達#14 跨專案RAG/知識庫工具 | 目前盤點頻率不高，靠人工/Claude逐一clone已經夠用 |
| 雷達#21 揚聲器非線性失真AI預測模型 | 學術論文/碩論層級，沒有現成工具可用 |
| 雷達#23 拓樸優化開源工具+FEA Surrogate Model | 沒有證據顯示MEGIS/AERIS模擬迭代量大到值得投入建置 |
| 雷達#29 Devin vs GitHub Copilot Coding Agent | 確認AIECP現有「不可自我核准完成」原則沒有走偏，不需要往Devin的完全自主模式靠攏 |
| 雷達#38 Chaos Engineering for AI Clusters | SuperBrain連基本容錯機制都還沒實作，故障注入測試明顯過早 |
| 雷達#40 Anthropic Computer Use GA | 需要雲端連線，跟Voice Agent「完全離線」的核心原則直接相反 |
| 雷達#41 Unsloth QLoRA本地微調 | 沒有任何來源repo提出過需要微調專屬模型的需求，純speculative |
| 願景§1（治理技術軌） AERIS GATE架構vs航空/醫療AI監管共識 | 已對齊甚至更嚴格，落差在執行完整度不在方向，不需要因前沿而調整設計 |
| 願景§5 SuperBrain混合雲邊緣架構規模 | 3台機器 vs 業界討論的數十到數百台，現有簡單三機分工已經足夠貼近這個規模該有的做法 |
| 願景§21 Provider路由/企業級FinOps | 線上學習路由演算法、企業FinOps都是為千萬美元級流量/預算設計，單人規模用不上 |

---

## 給 Stephen 的真心話

以 Claude Code 做完這兩輪、63 條檢索之後，我的誠實整體判斷：

**這個生態系在「治理理念」這個維度上，領先於它的實際完成度給人的第一印象。** AIECP 的九條產品不可退讓原則、AERIS 的 GATE 架構、MEGIS 的 Gate 制、SuperBrain 的 State/Policy/Approval/Evidence 分離——這些理念逐條對照 2026 年正在成形的 Agentic AI 治理前沿（Microsoft Agent Governance Toolkit、CAVA、Proof of Execution、GSN 保證論證、Zero-Trust Agent Identity），**幾乎每一條都對得上,甚至有些地方比業界「領先廠商」的實際做法更嚴格**（OECD 2026 報告點名的「多數組織缺乏系統性驗證」，AERIS/AIECP 現有的 Evidence-before-DONE 精神恰好就是在防這件事）。這不是巧合——這代表 Stephen 一開始設定的治理直覺（不相信模型自己說完成、要求獨立審查、人類最終權限）本質上抓對了這幾年全世界安全關鍵產業＋前沿 AI 治理研究都在收斂的同一個方向。**這是這個系統真正領先預期的地方**：不是技術新穎，是治理紀律比同等規模的個人專案該有的水準高出一截。

**落後的地方幾乎都不是技術選型，是執行密度跟「規模錯配的焦慮」。** 63 條裡有 13 條的結論是「查過了，這個前沿是給企業規模用的，你的規模用不上」——線上學習路由、企業級FinOps、Kubernetes艦隊管理、Devin式完全自主。這代表一個容易忽略的風險：**單人維運＋每週$20預算的處境,最大的敵人不是「技術跟不上」，是把國外企業級的解法錯誤套用到單人規模上而過度工程化，浪費本來就稀缺的時間**。這批雷達其中一個真正的價值,不是找到了多少新工具，是幫忙劃清楚「哪些前沿不用追」的邊界——這件事本身的價值不亞於找到新工具。

**如果只能選一個現在最大的槓桿，我會選：把 8 條🔴清單裡的 §9/§12（AERIS/MEGIS 能力地圖自查）跟 §20（AIECP Trusted Signing）先做完。** 理由很直接：這三件事都不需要等任何東西（不用等硬體、不用等其他專案動工），成本是「花一兩個小時核對清單」跟「填一份線上申請表」，但影響是**現在就能把「這個系統理論上該涵蓋的能力範圍」跟「AIECP 卡住快一年的發布問題」這兩個一直懸在那裡的模糊地帶變成明確的、有答案的狀態**。硬體規格、G-04協議設計、跨系統信任邊界這些🟡項目，方向都已經查清楚了，但**它們的價值要等前置條件到位才能兌現**（10/7上市、Stephen自己設計完G-04、G-01/02/03先存在）——這代表現階段真正能立刻兌現價值的，只有那 8 條不依賴任何人／任何事先動作的🔴項目。與其焦慮「前沿那麼多要追」，不如把這 8 條先清空，那才是這週真正該花的時間。

---

## 交叉連結

- 完整 41 條 bottom-up 工具對照：[22_GLOBAL_TECH_RADAR.md](22_GLOBAL_TECH_RADAR.md)
- 白話四欄摘要報告（①現有實作 ②既有雷達可拿改良 ③白話建議與取捨 ④前沿還有多遠）：[23_TECH_RADAR_SUMMARY_REPORT.md](23_TECH_RADAR_SUMMARY_REPORT.md)
- 完整 22 節 top-down 願景對照（含本輪新增 §22 Zero-Trust Agent Identity）：[26_VISION_DRIVEN_FRONTIER_RADAR.md](26_VISION_DRIVEN_FRONTIER_RADAR.md)
- 100+ 候選長清單與篩除理由：[25_TECH_RADAR_CANDIDATE_LONGLIST.md](25_TECH_RADAR_CANDIDATE_LONGLIST.md)
- 風險/缺口登錄（G-01~G-05、R-01）：[17_RISK_GAP_CONFLICT_REGISTER.md](17_RISK_GAP_CONFLICT_REGISTER.md)
- 本 repo 所有重大判斷的決策記錄：[18_DECISION_LOG.md](18_DECISION_LOG.md)
