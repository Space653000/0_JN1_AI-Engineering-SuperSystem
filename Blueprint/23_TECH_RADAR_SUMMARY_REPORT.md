# 23 — Tech Radar 彙整比較報告

> 這是 [22_GLOBAL_TECH_RADAR.md](22_GLOBAL_TECH_RADAR.md) 41 條雷達的**彙整版**：一張表看完「本地現況 vs 2026 最新科技 vs 本 repo 建議」。長話短說版，完整引用來源請點對應雷達條目。#1-16 是第一輪；#17-41 是 2026-09-26 第二輪（Stephen 要求「先列100+候選再篩選收斂」後的成果，篩選過程見 [25_TECH_RADAR_CANDIDATE_LONGLIST.md](25_TECH_RADAR_CANDIDATE_LONGLIST.md)）。

---

## 願景導向四欄比較（2026-09-26 新增，回應 Stephen「先看願景、往外找前沿」的修正）

> 這一段是 [26_VISION_DRIVEN_FRONTIER_RADAR.md](26_VISION_DRIVEN_FRONTIER_RADAR.md) 的濃縮版，重點是**四欄比較**，不是三欄：①本地現在真正在做什麼、②既有雷達#1-41已經找到、可以直接拿來用的近期改良、③**用白話文講「換我是你我會怎麼選」**、④真正的世界最前沿有多遠、還值不值得現在追。第③欄是特別為 Stephen 寫的：不堆術語，直接講白話的成本/效益/風險判斷，目的是讓不是每個領域都懂技術細節的人也能一眼看懂怎麼選。完整引用來源、逐條檢索紀錄請見 [26](26_VISION_DRIVEN_FRONTIER_RADAR.md) 對應章節。

### AERIS（聲學工程）

| 欄位 | 內容 |
|---|---|
| ① 現有實作 | 聲學模擬/量測工具鏈本身**沒有公開確認用哪一套**（`UNKNOWN`，見既有雷達 [#4](22_GLOBAL_TECH_RADAR.md)）；但治理面已經有明確的 GATE-01~08（Blueprint 唯一真相、Evidence Before DONE、獨立審查、四方版本一致），出自 `constitution.md` |
| ② 既有雷達可拿改良 | [#4](22_GLOBAL_TECH_RADAR.md) Pyroomacoustics 可選、[#21](22_GLOBAL_TECH_RADAR.md) 非線性失真AI預測（仍學術階段） |
| ③ 白話建議與取捨 | 白話講：AERIS 現在最缺的**不是更厲害的聲學AI工具，而是先把「證據制度」真的做出來**。這輪往全世界找，發現「AI直接學物理方程去設計揚聲器」這件事，全世界目前也只有零星幾篇論文、幾本碩士論文在做，還沒有一個能直接下載安裝的產品——換句話說，**就算現在花時間去追這個「全球最前沿」，也追不到一個能用的東西，只會浪費時間**。反而是 AERIS 自己已經設計好的「證據優先、獨立審查、不能自己證明自己做完」這套規矩，跟航空/醫療器材業界2026年最新在收斂的做法方向是一致的、甚至更嚴格。**我的建議：現階段不要為了追聲學AI黑科技分心，先把 A-D 階段做完、E 驗收真的跑起來，這比追那些還在實驗室的論文技術，投資報酬率高很多**。 |
| ④ 真正前沿還有多遠 | 聲學AI設計：學術/碩論階段，離「可安裝產品」還很遠。治理方法論：AERIS 已經站對位置，落差在執行完整度不在方向。 |

### MEGIS（機構工程）

| 欄位 | 內容 |
|---|---|
| ① 現有實作 | Gate制人工審查（G0-G9）+ CadQuery 幾何核心，LLM 明確被限制只能整理意圖、不能碰幾何/物理求解器（`README.md` 第5行） |
| ② 既有雷達可拿改良 | [#3](22_GLOBAL_TECH_RADAR.md)/[#24](22_GLOBAL_TECH_RADAR.md) CadQuery AI生態系、[#22](22_GLOBAL_TECH_RADAR.md) GD&T自動標註、[#23](22_GLOBAL_TECH_RADAR.md) 拓樸優化+FEA Surrogate |
| ③ 白話建議與取捨 | 白話講：MEGIS 現在的做法（先確定性規則跟受限幾何、LLM只負責講人話）**剛好就是全世界最新一篇「Physics-in-the-Loop」論文（2026年5月才發表）在講的最先進做法**——意思是 MEGIS 沒有走錯方向，甚至比很多商用工具更早想到這一點。真正的差距是**規模**：Autodesk、Altair這些大廠已經能做到「巨觀+微觀同時做拓樸優化」「積層製造的噴頭參數直接算進生成設計」，MEGIS目前還在治具跟簡單幾何的層級。**我的建議：不用急著跟大廠拚規模，那是燒錢燒團隊才做得到的事；但等 MEGIS 真的要做拓樸優化這塊（現在還沒做），可以直接參考「製造參數直接放進生成演算法」這個具體做法，比「先生成、後面才檢查做不做得出來」的舊流程更省工**。 |
| ④ 真正前沿還有多遠 | 方法論方向對齊前沿論文；規模與工業級平台（Autodesk/Altair）仍有數量級差距，屬合理差距（單人維運 vs 大廠平台）。 |

### AIECP（控制平面/治理）

| 欄位 | 內容 |
|---|---|
| ① 現有實作 | Mission/Task/Queue/Scheduler/Policy/Approval/Evidence 自建控制面；九條產品不可退讓原則（本地優先、人類權威、證據優於自我宣稱等），出自 `.ai/BLUEPRINT.md` |
| ② 既有雷達可拿改良 | [#5](22_GLOBAL_TECH_RADAR.md)/[#16](22_GLOBAL_TECH_RADAR.md)/[#27](22_GLOBAL_TECH_RADAR.md) SLSA/供應鏈證據、[#10](22_GLOBAL_TECH_RADAR.md)/[#15](22_GLOBAL_TECH_RADAR.md)/[#36](22_GLOBAL_TECH_RADAR.md) 獨立驗證方法 |
| ③ 白話建議與取捨 | 白話講：AIECP 這九條原則（本地優先、人要能核准、不能自己說完成就算完成…）**幾乎一條一條都對得上2026年全世界正在發展的「AI agent治理」新研究**——這代表 AIECP 一開始的設計方向就是對的，不是關起門來自己發明的土方法。但這些新研究（CAVA、Proof of Execution 這些）大多是2026年才剛發表的論文或剛開源的工具，**還沒有一個「大家都公認的標準」**，所以現在要 AIECP 換掉自己那套系統、改用別人的框架，其實時機還太早、風險比好處大。**我的建議：現階段維持自己的做法，但记一筆——下次要大改版時，可以參考這些新框架「動作發生當下就自動產生防偽證明」的想法，把現有「事後留紀錄」的證據系統升級成「當下就有密碼學證明」，會比現在更難造假**。 |
| ④ 真正前沿還有多遠 | 願景方向對齊2026年agentic治理研究前沿；具體技術（CAVA/Proof of Execution）尚未形成業界標準，換入風險大於現階段效益。 |

### Offline-Local-Voice-Agent（語音控制）

| 欄位 | 內容 |
|---|---|
| ① 現有實作 | 100%離線語音控制Windows，P0-P6分階段驗收，控制優先順序 API>UI Automation>...>Vision，出自 `README.md`／`.ai/BLUEPRINT.md` |
| ② 既有雷達可拿改良 | [#1](22_GLOBAL_TECH_RADAR.md) 語音輸入引擎對標、[#8](22_GLOBAL_TECH_RADAR.md) 喚醒詞、[#40](22_GLOBAL_TECH_RADAR.md) Computer Use GA（不建議採用，離線/雲端前提相反） |
| ③ 白話建議與取捨 | 白話講：Voice Agent「先讓AI聽懂、想清楚要做什麼，再用固定格式的指令去執行」這個設計，跟國外最新做輔助機器人語音控制的研究（史丹佛VoicePilot這類）是同一套思路，方向沒問題。這輪往外找還發現一個新東西：2026年的研究在講「機器人不只是聽話照做，還要會反問、會主動講狀態」——這種「雙向對話」比 Voice Agent 現在規劃的「你講一句、它做一件事、回報一句」更進一步。**我的建議：這個雙向對話的功能先記下來就好，現在完全不用花力氣做**，因為 Voice Agent 連最基礎的「聽得懂、控制得動 Windows」這關都還沒完全過關（P0都還在驗證），先把地基做穩，雙向對話這種進階功能留到之後有餘力再說。 |
| ④ 真正前沿還有多遠 | 核心控制哲學已對齊前沿研究；「雙向澄清對話」是下一個前沿方向，但屬於錦上添花，非現階段優先。 |

### SuperBrain（`0_JN1_2AGAVE128-1MAERA64`，混合運算織構）

| 欄位 | 內容 |
|---|---|
| ① 現有實作 | 三機分工：Laptop唯一連網Gateway，兩台Spark隔離LAN只跑本地AI，「本地能做到雲端80-90%水準的工作就交給本地」，出自 `.ai/BLUEPRINT.md` |
| ② 既有雷達可拿改良 | [#2](22_GLOBAL_TECH_RADAR.md)/[#19](22_GLOBAL_TECH_RADAR.md) 本地LLM選型、[#9](22_GLOBAL_TECH_RADAR.md)/[#20](22_GLOBAL_TECH_RADAR.md) 跨機推理加速 |
| ③ 白話建議與取捨 | 白話講：SuperBrain「哪些工作交給本地、哪些求助雲端」用一個具體數字門檻（本地分數要到雲端的85%才交給本地）來決定，這個做法**剛好跟2026年業界在講的混合雲端/邊緣架構的做法一致**，不是自己土法煉鋼。往外找到的前沿案例，動不動就是「幾十到幾百台機器」的艦隊管理規模，SuperBrain現在只有3台機器。**我的建議：現在完全不需要因為「國外在討論更大規模的管理系統」就跟著把3台機器的系統搞得很複雜（比如上Kubernetes這種艦隊管理工具）**——那是殺雞用牛刀，現有簡單的三機分工設計已經夠用、也夠貼近前沿的精神，等哪天機器真的擴充到10台以上，再回頭認真評估要不要上更複雜的管理架構。 |
| ④ 真正前沿還有多遠 | 架構理念（可替換工人、本地優先量化門檻）已對齊前沿；規模差距（3台 vs 前沿討論的數十~數百台）是合理差距，非缺陷。 |

### `0_JN1_AERIS_Local-computer-implementation`（本機落地層）

| 欄位 | 內容 |
|---|---|
| ① 現有實作 | 只落實 AERIS Core 已核准 Blueprint，不可自行發明架構（GATE-02），四方版本一致靠人工/AI核對SHA（GATE-06） |
| ② 既有雷達可拿改良 | 繼承 §AERIS 各項，另見 [#16](22_GLOBAL_TECH_RADAR.md) SLSA落地細節 |
| ③ 白話建議與取捨 | 白話講：這個 repo 的工作是「照著藍圖蓋房子」，本身沒有自己的目標。它現在核對「藍圖版本、程式版本、本機版本、正在跑的服務版本」這四樣東西有沒有兜起來，靠的是人工比對雜湊值。國外最新研究在做的是「讓執行的當下就自動產生一個沒辦法造假的證明」，而不是事後再去核對。**我的建議：這個不急，現有的人工核對方法沒有錯，只是比較累、比較容易漏；等以後真的發現人工核對常常出錯或太花時間，再考慮導入這種自動化證明工具，現在花心力在這上面不是最優先的事**。 |
| ④ 真正前沿還有多遠 | 問題定義（四方一致）已抓對重點；自動化驗證技術尚屬2026年新興研究，非現成可用方案。 |

### `0_JN1_AERIS_Supervision`

| 欄位 | 內容 |
|---|---|
| ① 現有實作 | `NOT VERIFIED / ACCESS DENIED`——私有repo，本次任務已嘗試以唯讀權限接入，但實際讀取被本session權限系統阻擋，依規定不得用其他公開資訊推測內容 |
| ② 既有雷達可拿改良 | 不適用 |
| ③ 白話建議與取捨 | 白話講：這個專案現在裡面裝了什麼，我完全看不到，所以沒辦法給任何建議——**不是「我覺得它做得不夠好」，而是「我根本沒有資格評論」**。硬要用外部公開資訊去猜它現在在做什麼，反而會誤導 Stephen，所以這裡誠實交白卷。 |
| ④ 真正前沿還有多遠 | 無法評估（無可驗證的願景基礎） |

### JN1-UOA / JN1-UOD（未來統一監管概念）

| 欄位 | 內容 |
|---|---|
| ① 現有實作 | 尚未落地——目前只是本 repo 應 Stephen 要求提案並經選定的命名概念，`0_JN1_AERIS_Supervision` 本身尚未改名或擴大（見 [Blueprint/18](18_DECISION_LOG.md)） |
| ② 既有雷達可拿改良 | [#6](22_GLOBAL_TECH_RADAR.md)/[#33](22_GLOBAL_TECH_RADAR.md)：直接採用 OpenTelemetry GenAI 語意慣例做統一監管資料格式，Phoenix（單人維運較輕量）或 Langfuse（功能更完整）二選一自架 |
| ③ 白話建議與取捨 | 白話講：現在雷達建議的「統一資料格式」只解決了「大家講同一種語言」這個問題，這是必要的，但不是最難的部分。這輪往外找核能、航空業界的監管做法，發現他們最頭痛的其實是「文件上寫的規矩」跟「實際發生的事情」對不對得起來——2026年的調查發現，就連很多重視安全的大公司，都做不到「回頭去確認寫在文件裡的規矩真的有被遵守」。**我的建議：Stephen 未來設計 JN1-UOA 時，不要以為裝了 OpenTelemetry+Phoenix這類工具收集資料，監管就做完了——那只是「格式統一」，真正難的是「怎麼確認AIECP/AERIS/MEGIS/Voice Agent/SuperBrain實際做的事，真的符合它們自己講好的規矩」，這件事全世界目前都還沒有解法，不是裝個工具就能解決的**，這件事要留到JN1-UOA真正要動工設計時，當作核心問題來面對，而不是順手用個監控工具帶過去。 |
| ④ 真正前沿還有多遠 | 資料格式整合層：業界標準明確，可直接採用。「控制措施-實際結果對應驗證」層：全球尚無收斂解法，屬長期觀察方向。 |

> 完整檢索紀錄、原文引用與更多子發現，見 [26_VISION_DRIVEN_FRONTIER_RADAR.md](26_VISION_DRIVEN_FRONTIER_RADAR.md)。

---

## 一頁看完

| # | 主題 | 本地現況 | 2026 最新科技 | 建議 |
|---|---|---|---|---|
| 1 | 語音輸入 | Voice Agent 離線管線，中文辨識97.9%（實測） | ChatGPT GPT-Live 端到端音訊模型，232ms延遲；開源可替代方案：Parakeet TDT + Kokoro/Qwen3-TTS | 🟢 評估開源方案補強架構思路，但**不要因為外部聽起來好就重寫**——先 A/B 測試 |
| 2 | 本地LLM選型 | SuperBrain 規劃跑在128GB機器上 | 頻寬瓶頸(273GB/s)，MoE模型(Qwen3-Coder-Next/Laguna S 2.1)比dense模型適合 | 🟢 選型優先看 MoE，不是參數量（見#19更新：部分熱門模型實際塞不進128GB） |
| 3 | 機構設計(MEGIS) | Gate制人工+CadQuery | Zoo.dev/AdamCAD 能做簡單托架，量產公差還不行 | 🟡 觀察，不取代 Gate 制（見#24更新：優先看CadQuery自己的AI生態系） |
| 4 | 聲學模擬(AERIS) | 未知現有工具 | 無揚聲器專用AI工具；Pyroomacoustics可選 | ⚪ 現況已足夠（見#21延伸：非線性失真AI預測仍是學術階段） |
| 5 | Evidence標準(AIECP) | 自家5級證據(STATIC~OWNER-EXTERNAL) | SLSA供應鏈證明框架(Level 0-3) | 🟢 評估對齊SLSA Level 2/3，缺簽章這一步 |
| 6 | 監管資料格式(JN1-UOA) | 未設計 | OpenTelemetry GenAI語意慣例，業界標準 | 🟢 直接採用，不要自創格式（見#33：Phoenix比Langfuse更輕量，適合單人維運） |
| 7 | 硬體身分(R-02) | "型號RTX Spark"身分未確認 | **已確認**＝Microsoft Surface RTX Spark Dev Box，2026-06-02 Build發表，**2026-10-07才正式上市** | 🔴 **P0卡住可能是因為硬體還沒上市**；上市後可套用官方規格（見#31：晶片是ARM64 Grace CPU，即SPARK-AGAVE是ARM64機器） |
| 8 | 喚醒詞 | Voice Agent現有引擎未知 | onnx-wakeword：<130KB、<10ms | 🟡 若現有太重可評估換 |
| 9 | 跨機推理(SuperBrain) | 兩台Spark各自獨立跑 | EXO/vLLM+Ray可切分大模型到多機 | 🟢 想跑更大模型可評估，2.5GbE網路低於業界建議（見#20：EAGLE-3推測解碼可先緩解頻寬瓶頸，不用先上多機） |
| 10 | **獨立驗證(G-04)** | 待Stephen設計 | LLM Judge驗證與執行式驗證器有32.4%分歧率；主流benchmark被證實可造假 | ⚠️ **確定性測試優先，LLM Judge只能輔助**（見#36：2026年新研究進一步證實多agent debate不可靠） |
| 11 | 並行Agent沙盒(AIECP) | Codex OFFICIAL/PEGA worktree隔離，卡在Windows/ARM64驗證 | Git worktree隔離已是業界標準；更進階是容器化+防火牆 | 🟢 架構方向對（見#30：微軟2026-06官方推出MXC SDK正面回應這個缺口；見#31：ARM64工具鏈2026年整體仍早期） |
| 12 | DFM審查(MEGIS) | Gate審查階段抓問題 | 2026做法：建模階段即時+審查階段完整，兩層疊加 | 🟢 評估加裝in-CAD即時檢查（見#22：CoLab AutoReview等GD&T自動標註工具可加在Gate審查前） |
| 13 | 憑證管理(AIECP) | 未知是否短效/透過broker | 短效、限定範圍、broker簽發、推理引擎不碰原始憑證 | 🟢 評估是否符合這個模式（見#32：Infisical Agent Vault是較輕量的具體落地選項） |
| 14 | 跨專案知識庫 | 人工clone七repo盤點 | LlamaIndex/Haystack本地RAG，混合搜尋+知識圖譜 | 🟡 盤點頻率高了再考慮，現在不急 |
| 15 | G-04深挖：獨立驗證實作 | 待Stephen設計 | Behavioral Equivalence Harness（驗證器不含模型）、Contract-Driven Adversarial Verification（結構性隔離兩agent） | 🟢 SuperBrain現有的Spark互不連線設計天生符合結構性隔離（見#36：mutation testing/property-based testing是具體落地技術） |
| 16 | SLSA落地細節(AIECP) | 自家SHA256SUMS+RELEASE_PROVENANCE.json | GitHub Artifact Attestations（幾行YAML）；`slsa-github-generator`可直接衝Level 3 | 🟢 先上Artifact Attestations成本最低（見#27：加上SBOM+OpenSSF Scorecard補強） |
| 17 | 中文ASR/全雙工語音 | Voice Agent中文ASR引擎未知，97.9%實測辨識率 | SenseVoice/FunASR中文CER 7.81% vs Whisper 20%+；Kyutai Moshi全雙工延遲160-200ms | 🟢 拿SenseVoice/FunASR做A/B測試對標，比通用多語言ASR更貼近中文場景 |
| 18 | 開源TTS選型更新 | 未知現有TTS方案 | CosyVoice3(18種中文方言)、F5-TTS、GPT-SoVITS(少樣本克隆) | 🟢 若中文場景為主，優先評估GPT-SoVITS/CosyVoice3而非英文為主的Kokoro |
| 19 | 本地LLM 128GB容量現實檢查 | SuperBrain規劃128GB機器 | DeepSeek-V3.2/GLM-5.2實際塞不進128GB；Qwen3系列(Apache 2.0)+Mistral Medium 3.5可行 | 🟢 選型時把「4-bit量化後實際佔用」列為硬性篩選條件，修正雷達#2 |
| 20 | 本地推理加速 | 未評估 | EAGLE-3推測解碼，2026已是vLLM/SGLang/TensorRT-LLM標準功能，最高4.79倍加速 | 🟢 直接緩解SPARK硬體273GB/s頻寬瓶頸，成本低於評估多機分散推理 |
| 21 | 揚聲器非線性失真AI預測 | 未知 | 深度學習THD/IMD預測誤差僅1.08%/0.34%，但仍是學術/碩論階段 | 🟡 觀察，尚無現成工具可用 |
| 22 | MEGIS GD&T自動標註 | Gate制人工審查 | CoLab AutoReview等工具可自動抓缺失基準/標註不一致 | 🟢 評估加裝在Gate審查前做第一輪自動掃描 |
| 23 | 拓樸優化+FEA Surrogate Model | 未知模擬工作量 | 開源拓樸優化工具走向模組化多後端；FEA surrogate model工業界已用專利取代FEM solver呼叫，10^4倍加速案例 | 🟢 若模擬迭代量大，值得評估；需先確認MEGIS/AERIS實際迭代量 |
| 24 | MEGIS CadQuery AI生態系 | 已確認用CadQuery | CadQuery生態系已有暴露MCP server的AI Copilot、Claude Code skill | 🟢 更新#3建議：優先看CadQuery自己生態系的AI工具，比獨立Text-to-CAD產品整合成本低 |
| 25 | MCP 2026-07-28重大改版 | 本session即透過MCP運作 | 協定核心變無狀態、移除初始化交握，官方稱「最大幅度修訂」 | 🟢 若AIECP規劃MCP-compatible Provider，需注意這是斷點升級不是小版本 |
| 26 | A2A協定+AGENTS.md標準化 | 七個repo都各自定義任務schema，無共通協定 | A2A(Linux Foundation治理)解決agent互相發現/委派任務；AGENTS.md已60000+專案採用 | 🟢 對應G-01/G-02/G-03，設計跨系統介面前先看A2A是否已解決同樣問題 |
| 27 | 供應鏈證據延伸 | SHA256SUMS+RELEASE_PROVENANCE.json | OpenSSF Scorecard+SBOM(CycloneDX/SPDX)是2026標準組合 | 🟢 與#16 Artifact Attestations一起排進同一輪低成本落地 |
| 28 | AIECP Harness對照 | Mission/Task/Queue/Scheduler自建 | LangGraph/CrewAI/AutoGen三分天下；Temporal.io獲3億美元融資做AI agent基礎設施 | 🟡 方向一致不急著換，Temporal的Durable Execution可在遇到失敗復原需求時評估 |
| 29 | AIECP Builder對照 | Codex OFFICIAL/PEGA不可自我核准 | Devin完全自主vs GitHub Copilot Coding Agent異步沙盒+draft PR | 🟡 AIECP現有「不可自我核准」比Devin更保守，方向沒走偏 |
| 30 | Windows沙盒官方新方案 | 卡在Windows/ARM64沙盒驗證 | 微軟2026-06-02發表MXC SDK，跨Windows/WSL的政策驅動agent執行層 | 🟢 升級為高優先：官方剛推出正面回應這個缺口的工具，優先評估 |
| 31 | Windows ARM64工具鏈成熟度 | SPARK硬體身分已確認 | **新確認：SPARK-AGAVE是ARM64機器**(Grace CPU)；WSL3(2026-06預覽)、GH Actions ARM64 runner仍preview | 🟢 上市後第一個要測的是WSL3 GPU/NPU passthrough相容性，整條ARM64路線2026年仍早期 |
| 32 | 憑證管理落地選項 | 未知是否短效broker | Infisical Agent Vault(輕量)、SPIFFE/SPIRE(重量級workload身分) | 🟢 先評估Infisical，規模變大再考慮SPIFFE/SPIRE |
| 33 | 可觀測性落地選型 | 未設計 | Phoenix單process+MIT-like但ELv2授權，Langfuse四服務但MIT授權+更完整功能 | 🟢 單人維運先用Phoenix做PoC，需求變複雜再遷移Langfuse |
| 34 | **成本優化：Prompt Caching降價** | Stephen每週~$20額度 | 2026-09-01 Claude cache read再降75%到$0.25/M token | 🟢 **本輪對Stephen預算痛點最直接的一條**：檢查重複性高的呼叫是否命中快取 |
| 35 | AI Gateway/Router落地 | AIECP各自呼叫供應商 | LiteLLM統一閘道+快取；RouteLLM可省85%成本同時保留95%品質 | 🟢 與#34一起評估，純粹省錢不改架構本質 |
| 36 | G-04驗證方法更新 | 待Stephen設計 | Mutation testing+property-based testing是2026驗證AI程式碼標準做法；多agent debate進一步被證實不可靠 | 🟢 強化#10/#15警示：SPARK-AGAVE-4應優先做確定性mutation testing，而非LLM debate |
| 37 | Contract Testing(Pact) | G-01/G-02介面完全缺失(高嚴重度) | Pact消費者驅動契約測試是業界標準做法 | 🟢 高優先：直接對應G-01/G-02，未來設計介面時的具體方法論參考 |
| 38 | Chaos Engineering | R-01單點故障風險，跨機容錯未實作 | ReliabilityBench等LLM agent故障注入框架2026年成熟 | 🟡 等SuperBrain容錯機制真的實作出來再用故障注入驗證 |
| 39 | Windows NPU/Aion 1.0 | #7已知預裝Windows ML | Build 2026發布Aion 1.0裝置端SLM(14B/32K context) | 🟢 評估輕量任務丟NPU常駐模型、重任務才用GPU大模型的分層策略 |
| 40 | Anthropic Computer Use GA | Voice Agent完全離線設計 | 2026-08-19 computer use等工具GA，支援batch actions | 🟡 離線/雲端前提相反，不建議採用，但batch actions介面設計可參考 |
| 41 | Unsloth QLoRA微調 | 未提出微調需求 | QLoRA+Unsloth可在單張消費級GPU微調7-8B模型 | 🟡 先記錄，等真的出現領域術語微調需求再評估 |

## 三個最該優先看的（2026-09-26 第二輪更新）

1. **#34 Prompt Caching降價75%——本repo存在理由本身的直接解方**：2026-09-01起Claude cache read降到$0.25/M token，直接針對Stephen每週$20額度的痛點。建議立即檢查AIECP、本SuperSystem repo自己的Claude Code使用模式是否命中快取，這是本輪唯一「不用開發、純粹調整使用方式」就能拿到立即效益的項目。
2. **#30+#31 Windows官方沙盒/ARM64工具鏈——直接回應既有#11缺口**：微軟2026-06-02同一場發表會既公布了SPARK Dev Box也公布了MXC SDK（官方agent沙盒方案），且本輪首次明確點出SPARK-AGAVE是**ARM64機器**（Grace CPU）。這改變了雷達#11「業界資料稀少」的解讀——答案剛出現、還在早期階段，Stephen硬體到手後第一個該測的就是WSL3在RTX Spark上的相容性。
3. **#37 Contract Testing (Pact)——直接對應G-01/G-02高嚴重度缺口**：這兩個介面缺失缺口長期停在「完全沒有介面」，Pact提供一個具體、不需要任何一方先開放API就能開始釐清雙方預期的方法論，值得作為未來設計介面時的起手式參考。

（原本#1-16輪的三個優先——硬體身分確認#7、G-04驗證警示#10、SLSA+憑證管理#5/#13——仍然有效，不因新一輪發現而降級，只是新增了#34/#30-31/#37這三個第二輪最值得優先看的。）

## 標籤說明

🟢 建議評估（值得花時間驗證）　🟡 持續觀察（未成熟或現在不急）　⚪ 現況已足夠（不用追）

## 使用方式

這份報告是快照，第一輪2026-09-26一次跑完16個主題，第二輪同日再擴充25個主題（篩選過程見[25](25_TECH_RADAR_CANDIDATE_LONGLIST.md)）。想更新單一條目，跟Claude說「更新雷達 #7」；想跑全新主題，說「跑雷達：XX」；想重新跑被篩除的候選，說「跑雷達：候選#XX」。完整細節、每條的原始引用來源，見[22_GLOBAL_TECH_RADAR.md](22_GLOBAL_TECH_RADAR.md)。
