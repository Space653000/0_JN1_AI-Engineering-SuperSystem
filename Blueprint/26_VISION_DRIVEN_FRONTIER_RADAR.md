# 26 — Vision-Driven Frontier Radar

> **這份文件跟 [22_GLOBAL_TECH_RADAR.md](22_GLOBAL_TECH_RADAR.md) 是互補、不是取代。** #1-41 是 bottom-up：先看來源 repo 現在用什麼工具/做法，再找「有沒有更新的同類工具」。這份是 Stephen 2026-09-26 明確要求的修正方向——**top-down、願景優先**：先只讀各專案自己藍圖裡「這個能力終極應該長什麼樣子」的願景語言（不看現有工具選擇、不看目前進度），再完全獨立地問「這個工程能力的世界前沿現在在哪裡」，最後才回頭看「本地離前沿有多遠」。
>
> Stephen 原話：「你的重視度不該過於專注在內部這幾個GitHub...你要以藍圖的發想規劃去外部全世界雲端找更好更新更全面更完善的功能技術...精力和能量和權責往外往雲端90%，檢索本地和現有github和盤點進度10%」。
>
> 依 [CLAUDE.md](../CLAUDE.md)：以下對七個來源 repo 的一切描述都只是**引用其公開藍圖文件的既有語言**，不是本 repo 的架構決策；外部前沿研究一律附真實檢索來源；找不到來源或無法存取一律標記 `UNKNOWN`/`NOT VERIFIED`/`ACCESS DENIED`。

---

## 讀法

每個專案一節，固定三段：

1. **願景摘要**：只從該 repo 自己的主要藍圖文件擷取「這個能力的終極目的/野心」，附「repo + 檔案路徑 + 大致行號」引用。
2. **前沿檢索**（2-4 條）：完全獨立於該 repo 現有做法，問「這個工程能力在全世界目前最前沿長什麼樣子」，附真實檢索來源與日期。
3. **🟢/🟡/⚪ 落差評估**：這個前沿離本地現況有多遠、值不值得現在投入。

---

## 1. AERIS — 聲學工程

### 願景摘要
> 「AERIS 一位人類主管＋100 個聲學專業能力席位的聲學工程師 = 一人抵百人 AI 聲學公司。」
> — `0_JN1_AERIS/README.md` 第 1 行

> 「AERIS 是由一位 Human Chief Engineer 掌握最終權限、以聲學工程需求為中心、用可替換的 AI 模型協助完成工作，並以證據、獨立審查及可重現性判定成果是否可信的工程組織系統。」
> — `0_JN1_AERIS/docs/AERIS_BLUEPRINT_ZH_TW.md`（「一句話說清楚 AERIS」段落，約第 11-13 行）

**兩層願景**：(a) 聲學工程能力本身要做到「一人抵百人」的專業廣度與深度；(b) 支撐這個能力的治理機制（Evidence Before DONE、獨立審查、四方版本一致，見 `constitution.md` GATE-01~08）本身要做到讓 AI 產出的工程判斷「可信」。前沿檢索因此分兩軌：聲學技術本身、以及支撐可信度的驗證方法論。

### 前沿檢索（2026-09-26）

**A. 聲學技術軌：生成式/物理知情揚聲器設計**
- 2026 年的研究前沿是把深度學習跟聲學物理方程直接耦合，而不是把聲音當一般音訊訊號處理：Physics-informed neural operators 已被用於原位聲學吸音材料特性鑑定（in situ characterization of locally reacting sound absorbers）；用於亥姆霍茲方程解的生成模型正在建立聲學材料資料集，讓「給定目標頻響，生成滿足亥姆霍茲方程的材料/幾何」變得可行。
- 揚聲器非線性建模：2025 年隆德大學碩士論文「Modeling Loudspeaker Nonlinearities with Deep Learning」與參數陣列揚聲器的非線性失真辨識/補償深度學習方法（feedforward WaveNet變體）代表這條線目前仍以學術碩論/單一論文為主，尚未整合進商用設計流程。
- 語音生成的物理知情神經運算元（Physics-Informed Neural Operator for Speech Production Analysis, 2026-06）顯示這套「神經運算元逼近物理場」的方法論正在從語音生成擴散到更廣的聲學正向/逆向問題。

Sources: [Physics-informed neural operators for the in situ characterization of locally reacting sound absorbers (arXiv 2604.07412)](https://arxiv.org/pdf/2604.07412) · [Generative Models for Helmholtz Equation Solutions: A Dataset of Acoustic Materials (arXiv 2510.09657)](https://arxiv.org/pdf/2510.09657) · [Modeling Loudspeaker Nonlinearities with Deep Learning (Lund University, 2025)](https://lup.lub.lu.se/student-papers/record/9207826/file/9207827.pdf) · [Deep Learning-Based Approach for Identification and Compensation of Nonlinear Distortions in Parametric Array Loudspeakers (arXiv 2412.01092)](https://arxiv.org/pdf/2412.01092) · [Physics-Informed Neural Operator for Speech Production Analysis (arXiv 2606.22364)](https://arxiv.org/pdf/2606.22364)

**B. 治理技術軌：安全關鍵產業的證據/監督架構**（直接對應 AERIS 的 GATE-01~08、Evidence Before DONE、獨立審查）
- 航空與醫療器材的 2026 年 AI 監管共識：需要「嚴謹的安全案例(safety case)、可問責的人類、漸進式導入、共享產業標準」——EUROCAE/SAE AS6983/ED-324 這個航空 AI 流程標準正由 600 人委員會制定中；EU AI Act 高風險系統要求「人類監督、技術穩健性、資料治理、明確問責」直接透過 Article 108 流入 EASA 現行安全體制。
- Safety case（安全案例）方法論正從航空/核能領域被引入通用 AI 安全治理，但 2026 年 OECD 對 20 個組織的分析發現：多數組織雖然結合量化指標與專家質性判斷，**但很少揭露完整方法論，也很少系統性驗證「文件記錄的控制措施」是否真的對應到「實際結果」**——這正是 AERIS GATE-04「文件存在僅證明文件存在」所警示的同一個落差,只是這裡是業界規模的實證。

Sources: [Fifty Years of Specification Completeness: What Aviation Certification Tells AI Governance About Epoch Limits, Proof Surfaces, and the Structural Gap (arXiv 2606.25120)](https://arxiv.org/pdf/2606.25120) · [Safety-Critical AI in Aviation: Where Things Really Stand (Aviathrust, 2026)](https://www.aviathrust.com/article/safety-critical-ai-in-aviation) · [Safety-Critical AI: Lessons from Aviation for Machine Learning Systems (Censinet)](https://censinet.com/perspectives/safety-critical-ai-lessons-aviation-machine-learning) · [The 2026 Singapore Consensus on Global AI Safety Research Priorities (arXiv 2608.14611)](https://arxiv.org/pdf/2608.14611)

### 🟡/⚪ 建議評估
- **聲學技術軌 🟡 持續觀察**：AERIS 目前的聲學工具鏈本身狀態是 `UNKNOWN`（本 repo 從未取得 AERIS 實際使用哪套模擬/量測軟體的確認，見 [22 條目#4](22_GLOBAL_TECH_RADAR.md)），但即使確認了，這輪檢索顯示「AI 直接學物理方程去做揚聲器逆向設計」全球仍是**單篇論文/碩論等級**，離「AERIS 可以直接採用」還很遠——這符合 Stephen 要求的誠實標註：這是真前沿，但不是可安裝的產品。
- **治理技術軌 🟢 值得對照**：AERIS 自己的 GATE 架構（獨立審查、四方版本一致、Evidence Before DONE）**方向上已經跟航空/醫療器材 2026 年正在收斂的 safety case 共識高度一致**，甚至比 OECD 點名的「多數組織」更嚴格（因為 GATE-04/05 明確禁止自我證明、要求可重現）。真正的落差不是「AERIS 治理理念落後前沿」，而是「AERIS 自己都承認 A-D 尚未完成、E 全面驗收 NOT_STARTED」——**前沿在方法論層面 AERIS 已經站對位置，落差在執行完整度，不在願景**。

---

## 2. MEGIS — 機構工程

### 願景摘要
> 「MEGIS 的目標，是將機械、聲學與製造工程師的判斷轉化為可追溯、可驗證、可重現的引導式生成工程流程。系統以確定性工程資料、規則、受限幾何與驗證證據為核心；LLM 只協助整理設計意圖、提出問題與解釋結果，不取代幾何核心、物理求解器或工程簽核。」
> — `0_JN1_MEGIS/README.md` 第 5 行

補充：v3.0 藍圖本身說明「本文件把 v1.0 的產品願景，經 v2.0 的 Gate 化，進一步精確化為可由 AI Agent 持續施工、由獨立 Agent 與具名工程師驗證、可中斷恢復、可稽核追溯的工程計畫」（`MEGIS_Blueprint/…v3.0-claude-code.md` 第 56 行）。**MEGIS 的願景核心不是「用 AI 畫圖」，而是「確定性 + 可驗證性優先於生成速度」**——LLM 明確被限定在意圖整理/提問/解釋角色，幾何核心與物理求解器不可被取代。

### 前沿檢索（2026-09-26）

- **多尺度拓樸優化**：西北大學 2025 年專利提出 C-HiDeNN-TD（Convolution-Hierarchical Deep-learning Neural Network Tensor Decomposition），首次讓巨觀/微觀尺度拓樸問題可以同時求解，解決過去高保真多尺度優化被有限元分析算力卡住的瓶頸。
- **製造感知拓樸優化**：Nozzle-Constrained Topology Optimization (NCTO) 這類工具把「噴頭間距、列印方向」這類積層製造參數直接內建進生成演算法，讓生成結果從一開始就考慮各向異性結構限制，而不是生成後再做 DFM 審查。
- **物理落地的混合式 agentic 架構**：2026-05 的「Physics-in-the-Loop: A Hybrid Agentic Architecture for Validated CAD Engineering Design」直接對應 MEGIS 的核心哲學——用 agentic 架構把 LLM 的設計意圖生成跟物理求解器的驗證迴圈綁在一起，而不是讓 LLM 自己判斷幾何是否合理。
- **產業平台**：Autodesk 目前是拓樸優化/生成設計專利最多的廠商（至少 12 件涵蓋空心/晶格拓樸、ML 設計生成、收斂控制形狀優化），Altair Inspire 已把拓樸優化、結構模擬、製造限制三者耦合成單一工作流。

Sources: [Generative Design & Topology Optimization 2026 (PatSnap)](https://www.patsnap.com/resources/blog/rd-blog/generative-design-topology-optimization-2026/) · [Physics-in-the-Loop: A Hybrid Agentic Architecture for Validated CAD Engineering Design (arXiv 2605.19717)](https://arxiv.org/pdf/2605.19717) · [Advanced Generative AI in Mechanical Design, Topology Optimization, Manufacturing Process Planning, and Industry 5.0 (Zenodo 2026)](https://zenodo.org/records/18149970) · [TopoStyle: Supporting Iterative Design with Generative AI for 2.5D Topology Optimization (arXiv 2604.21315)](https://arxiv.org/pdf/2604.21315)

### 🟢 建議評估
MEGIS 目前的 Gate 架構（G0-G9，見 v3.0 藍圖）本質上就是「確定性工程資料＋受限幾何＋驗證證據」的落地版本，這跟 Physics-in-the-Loop 這篇論文描述的架構**方向完全一致，甚至更早就在往這個方向走**（MEGIS 明確禁止 LLM 取代幾何核心跟物理求解器，這正是 Physics-in-the-Loop 論文提出的核心設計原則）。真正的落差在**規模**：MEGIS 目前的驗證範圍是治具幾何（G2）跟聲學/機器人薄切片（G7/G8），而 Autodesk/Altair 級的生成設計平台已經在做「巨觀+微觀同時拓樸優化」跟「積層製造參數內建生成」這種工業級規模化能力。這不代表 MEGIS 方向錯——MEGIS 的單人可稽核治理模式本來就不是要對標大廠平台的規模，但**如果 MEGIS 未來真的要做拓樸優化（v3.0 藍圖尚未涵蓋），NCTO 這類「製造參數直接進生成演算法」的作法，是比「先生成後 DFM 審查」更前沿的落地路徑**，值得在 MEGIS 設計拓樸優化 Gate 時參考。

---

## 3. AIECP — 控制平面/治理

### 願景摘要
> 產品不可退讓原則（Product Invariants）：「本地優先：原始碼、大型 log、索引、embedding、執行證據預設留在本地」「人類權威：破壞性/特權/憑證/發布/計費/安全敏感/不可逆操作需要政策檢查與通常需要明確人工核准」「證據優於自我宣稱：任務完成靠狀態/測試/證據證明，不是模型自己說完成」「可恢復性：每個任務都有持久狀態、trace、輸出、timeout/cancel 行為與明確終止狀態」「單一狀態來源：Board、Pipeline、Graph、Trace 都是同一個 canonical task/workspace model 的視圖」
> — `0_JN1_AIECP/.ai/BLUEPRINT.md` 第 11-27 行（摘自 `Blueprint/00_MASTER_BLUEPRINT.md`）

**AIECP 的願景不綁定任何特定 vendor**——它是「Mission/Task/Queue/Evidence/Approval」這套治理理想本身，目的是讓「AI 對話」跟「使用者電腦上真實工程工作」之間有一層可證明、可恢復、人類仍掌權的操作層。

### 前沿檢索（2026-09-26）

- **Agentic AI 執行期治理**：2026 年出現一批專門針對「AI agent 本身要被當作一級公民治理」的框架——微軟 2026-04 開源的 Agent Governance Toolkit 聲稱是第一個涵蓋 OWASP Top 10 Agentic AI 風險（含 goal hijacking、tool misuse、identity abuse、memory poisoning、cascading failures、rogue agents）的工具，用確定性、次毫秒級的政策執行。
- **Runtime 證據/認證**：CAVA (Canonical Action Verification and Attestation, 2026-07) 提出對 agentic AI 系統的「規範動作驗證與認證」；Proof of Execution (2026-07) 做「受治理 AI agent 動作的執行期驗證」；Decision Evidence Maturity Model for Agentic AI (2026-05) 提出屬性層級的證據成熟度規格——這些都是把「Evidence 優於自我宣稱」這個 AIECP 核心不可退讓原則，往「可形式化驗證、可稽核」方向推進的具體嘗試。
- **可問責結構**：「From Traceability to Justifiability: Accountability Structures in Agentic Software Engineering」(2026-08) 主張光有 traceability（誰做了什麼）不夠，還需要 justifiability（為什麼這個決定是對的），這是比 AIECP 目前的 Evidence/Trace 概念更進一步的問責標準。
- **供應鏈**：SLSA 相容的 build provenance（`attest-build-provenance`）已是 2026 年軟體供應鏈安全的基礎配備，這點與既有雷達 [#16](22_GLOBAL_TECH_RADAR.md) 一致，此處不重複展開。

Sources: [Introducing the Agent Governance Toolkit (Microsoft Open Source Blog, 2026-04-02)](https://opensource.microsoft.com/blog/2026/04/02/introducing-the-agent-governance-toolkit-open-source-runtime-security-for-ai-agents/) · [CAVA: Canonical Action Verification and Attestation for Runtime Governance of Agentic AI Systems (arXiv 2607.13716)](https://arxiv.org/pdf/2607.13716) · [Proof of Execution: Runtime Verification for Governed AI Agent Actions (arXiv 2607.05397)](https://arxiv.org/pdf/2607.05397) · [Decision Evidence Maturity Model for Agentic AI (arXiv 2605.04093)](https://arxiv.org/pdf/2605.04093) · [From Traceability to Justifiability: Accountability Structures in Agentic Software Engineering (arXiv 2608.23610)](https://arxiv.org/pdf/2608.23610) · [The 2026 Guide to Software Supply Chain Security (Cloudsmith)](https://cloudsmith.com/blog/the-2026-guide-to-software-supply-chain-security-from-static-sboms-to-agentic-governance)

### 🟢 建議評估
AIECP 的九條產品不可退讓原則跟 2026 年正在成形的「Agentic AI 治理」領域**幾乎逐條對應**：本地優先 ↔ 資料主權治理、人類權威 ↔ human-in-the-loop 政策執行、證據優於自我宣稱 ↔ Decision Evidence Maturity Model、可恢復性 ↔ Proof of Execution。這代表 AIECP 的願景設計方向**沒有偏離全球前沿**，但落差在於：這些前沿框架（CAVA、Proof of Execution、Agent Governance Toolkit）大多還是 2026 年才發表的研究/剛開源的工具，**尚未有「哪個是業界公認標準」的收斂**——AIECP 目前自建 Mission/Task/Queue/Evidence/Approval 架構是合理的（沒有現成標準可以直接採用），但下次重大版本演進時，值得評估這幾個新框架的「動作級認證(action-level attestation)」概念是否可以補進 AIECP 現有的 Evidence 模型，讓 Evidence 從「事後留痕」進化到「動作發生當下就產生密碼學可驗證的認證」。

---

## 4. Offline-Local-Voice-Agent — 語音控制

### 願景摘要
> 「100% 離線、斷網可用的 Windows 語音桌面代理人。使用者說話（或打字）→ 本機 AI 理解 → 安全控制 Windows（開程式、找檔案、UI 操作）→ 語音（或文字）回覆。」
> — `Offline-Local-Voice-Agent/README.md` 第 1-2 行

> 「完全離線——正式運作階段禁止任何雲端API。這條限定的是運算，不是設計靈感：介面/UX/功能可以參考現有雲端AI助理的做法...但推論本身必須留在本機完成」
> — `Offline-Local-Voice-Agent/.ai/BLUEPRINT.md` 第 26-28 行

**核心願景是「離線優先」跟「自然語音控制整台電腦」的結合**，且明確排序「API first > UI Automation > PowerShell/Win32 > AutoHotkey > 鍵盤滑鼠模擬 > Vision」的控制優先順序（同檔案第 3 節）。

### 前沿檢索（2026-09-26）

- **輔助型機器人語音控制的臨床驗證**：2026 年一項針對神經系統疾病患者使用語音控制輔助機械手臂的可用性研究發現：除了一位神經受損者外，所有參與者都能成功用語音介面操作，而搖桿介面只有 11 人能操作成功——這個結果雖然是機械手臂而非桌面控制，但證實了「語音是失能/受限使用情境下比傳統輸入更可及(accessible)的介面模式」這個更廣義的前沿判斷，跟 Voice Agent 面向的是不同使用情境（Voice Agent 是生產力工具，不是輔具），但控制哲學上有共通點。
- **LLM 作為語音介面驅動物理輔助機器人**：VoicePilot（Stanford，最初 2024，持續被引用到 2026 年的輔助機器人語音介面研究）展示了「LLM 當語音理解與任務規劃的中介層，再轉成結構化機器人動作」的模式——這與 Voice Agent 藍圖「意圖辨識/Tool Calling」階段（P2）的設計哲學相同：LLM 負責理解意圖、產出結構化呼叫，不直接執行任意指令。
- **雙向人機溝通**：「Bidirectional Human-Robot Communication for Physical Human-Robot Interaction」(arXiv 2601.10796, 2026) 討論的是「機器人不只聽指令，也要主動回報狀態、詢問澄清」的雙向溝通模式，這比 Voice Agent 目前規劃的「單向語音→意圖→執行→回報」流程更進一步；如果 Voice Agent 未來要做到真正「自然」的語音控制（而不只是命令式），雙向澄清對話是下一個前沿方向。

Sources: [Testing the usability of a voice control system for assistive robotic arms in people with neurological conditions (J NeuroEngineering Rehabil, 2026)](https://link.springer.com/article/10.1186/s12984-026-01902-1) · [VoicePilot: Harnessing LLMs as Speech Interfaces for Physically Assistive Robots (arXiv 2404.04066)](https://arxiv.org/pdf/2404.04066) · [Bidirectional Human-Robot Communication for Physical Human-Robot Interaction (arXiv 2601.10796)](https://arxiv.org/pdf/2601.10796) · [Giving Sense to Inputs: Toward an Accessible Control Framework for Shared Autonomy (arXiv 2501.16929)](https://arxiv.org/pdf/2501.16929)

### 🟡 建議評估
Voice Agent 目前規劃的 P0-P6 階段（VAD+喚醒詞+ASR → 意圖辨識 → Windows Automation → 安全分級 → Vision fallback → 整合測試）**在控制哲學上已經跟 VoicePilot 這類前沿輔助機器人語音介面研究一致**（LLM 做理解與規劃、不直接執行任意指令、結構化 tool call）。真正的落差是「雙向溝通」——目前 Voice Agent 藍圖沒有規劃「機器人/系統主動詢問澄清」這個環節，而這正是 2026 年輔助人機互動研究認為的下一個前沿。這屬於**🟡 值得記錄但不是現階段優先**：Voice Agent 目前連 P0（硬體/生態驗證）都需要先確認完成，雙向澄清對話是等核心語音控制迴圈穩定後才該評估的進階能力，不應該現在就分心投入。

---

## 5. SuperBrain（`0_JN1_2AGAVE128-1MAERA64`）— 混合運算織構

### 願景摘要
> 「核心原則：One SuperBrain, Many Replaceable Workers。ChatGPT、Claude、Gemini 與兩台 Spark 都是可以替換的工人；State、Policy、Approval、Evidence 則由你自己掌握。」
> — `0_JN1_2AGAVE128-1MAERA64/.ai/BLUEPRINT.md` 第 10 行

補充需求基線：「本地能做到雲端 80～90% 水準的工作，就交給本地」「Laptop 擴充周邊...兩台 Spark 盡量不連網，只跑本地 AI 並把能力發揮到極致；做不到的部分，透過 Laptop 向雲端求援」（同檔案第 1 節）。**願景核心是「State/Policy/Approval/Evidence 由使用者掌握、執行工人(worker)可替換」這個架構理念，不是綁定特定雲端或本地供應商**。

### 前沿檢索（2026-09-26）

- **混合雲/邊緣機器人架構**：2026 年業界共識是「按職責分層」——延遲敏感的自主性留在機器人/邊緣本地，協調、分析、訓練、艦隊管理放在合適的基礎設施層；當一個實體環境有數十到數百台機器共用時，facility edge server（介於個別機器與遠端基礎設施之間）開始有價值。這跟 SuperBrain「Laptop 是唯一連網 Gateway、兩台 Spark 隔離 LAN 只跑本地」的分層設計方向一致，但規模差三個數量級（SuperBrain 是 3 台機器，這類前沿討論的是數十到數百台）。
- **異質硬體混合多 agent 系統**：2026 年 AMD 展示的案例，是把工作拆分給 AMD Ryzen AI agentic PC 跟 AMD Instinct 資料中心基礎設施，混合架構被稱為「實務上的預設選擇：雲端做規模化與訓練，邊緣做速度與局部性」。
- **本地 vs 雲端決策框架**：2026 年的「Hybrid AI Architecture: Local Models + Cloud Frontier Models」討論的決策框架跟 SuperBrain 藍圖的「golden set 本地分數 ≥ 雲端 85% 走本地」量化門檻思路相似，代表 SuperBrain 這條「用量化基準決定本地/雲端路由」的設計已經對齊業界正在收斂的做法，而不是自創的權宜之計。

Sources: [Hybrid Cloud Edge Robotics: Enterprise Guide 2026 (Nezzhub)](https://nezzhub.com/hybrid-cloud-edge-robotics/) · [Advancing AI 2026: From Gigawatt Data Centers to Agentic PCs (AMD)](https://www.amd.com/en/blogs/2026/advancing-ai-2026-from-gigawatt-data-centers-to-agentic.html) · [How to Build a Hybrid AI Architecture: Local Models + Cloud Frontier Models (MindStudio)](https://www.mindstudio.ai/blog/hybrid-ai-architecture-local-models-cloud-frontier) · [Local AI vs Cloud AI in 2026: When to Run Models on Your Own Hardware (MindStudio)](https://www.mindstudio.ai/blog/local-ai-vs-cloud-ai-2026)

### ⚪ 建議評估
SuperBrain 的「One SuperBrain, Many Replaceable Workers」跟「本地優先、雲端求援」這兩條核心設計原則，**跟 2026 年混合雲/邊緣架構的業界共識方向一致，沒有走偏**；量化路由門檻（本地≥雲端85%走本地）這個具體做法甚至比很多業界討論更精確可執行。真正的落差純粹是**規模**——前沿討論的 facility edge server 案例是數十到數百台機器的艦隊管理，SuperBrain 是 3 台機器的單人操作場景。這代表**現階段不需要因為「前沿在討論更大規模架構」就過度設計 SuperBrain**（例如現在就上 Kubernetes 式的艦隊管理），標記 ⚪：現有簡單的三機分工設計已經足夠貼近這個規模該有的前沿做法，未來如果 Stephen 的機器數量真的擴大到十台以上量級才需要重新評估。

---

## 6. `0_JN1_AERIS_Local-computer-implementation` — 本機落地層

### 願景摘要
根據 AERIS `constitution.md` GATE-02（第 10 行）：「`Space653000/0_JN1_AERIS_Local-computer-implementation` 只落實已核准 Blueprint。禁止偷改目標、新增未核准架構、刪減必要功能、為通過測試降低驗收。」

**這個 repo 本身沒有獨立於 AERIS Core 的願景**——它的存在目的被 AERIS 自己的治理契約明確定義為「HOW 執行方式的權威」，願景（WHAT）完全繼承自 §1 的 AERIS Core 願景。本節不重複第 1 節的前沿研究，但補充一點 Implementation 層特有的前沿參考。

### 前沿檢索（2026-09-26）
Implementation 層的核心工程問題是「如何讓 Blueprint→Implementation→Local Runtime 三方保持一致、drift 立即可偵測」（GATE-06 四方版本一致 tuple）。這正好對應第 3 節 AIECP 找到的 **Proof of Execution / CAVA 動作級認證**研究方向——這些框架處理的正是「宣稱的狀態」與「實際執行的狀態」之間如何用密碼學可驗證的方式綁定，而不是靠人工核對 SHA。

Sources: 同第 3 節 [Proof of Execution (arXiv 2607.05397)](https://arxiv.org/pdf/2607.05397) · [CAVA (arXiv 2607.13716)](https://arxiv.org/pdf/2607.13716)

### 🟢 建議評估
AERIS GATE-06 目前用「Blueprint SHA、Implementation SHA、Local checkout HEAD/dirty digest、Running service loaded SHA」四方 tuple 手動核對 drift，這跟 Proof of Execution 這類 2026 年新框架想解決的問題**本質相同**，差別是後者想做到「執行當下自動產生可驗證證明」而不是「事後核對雜湊」。這是**低優先、但方向正確**的觀察項：AERIS 現有機制已經抓住問題核心（四方一致），若未來 drift 偵測需要自動化（目前靠人工/AI 核對），這類 runtime attestation 框架是具體的技術路徑參考。

---

## 7. `0_JN1_AERIS_Supervision`（未來 JN1-UOA 的前身）

### 願景摘要
`NOT VERIFIED / ACCESS DENIED`。這是私有 repo；本次任務執行時已透過 `add_repo` 嘗試以唯讀權限接入（回應顯示帳號對此 repo 有存取權），但實際讀取該 repo 內容的指令被本 session 的權限系統阻擋（Bash 分類器拒絕），依 CLAUDE.md「私有 repo 如果無法存取 → 標記 NOT VERIFIED / ACCESS DENIED，不要用其他公開資訊推測其內容」處理，未嘗試以其他方式繞過。

現有的、可驗證的資訊只有：本 repo（SuperSystem）自己在 [Blueprint/10_SUPERVISION_AND_EVIDENCE.md](10_SUPERVISION_AND_EVIDENCE.md) 記錄的說法——「`0_JN1_AERIS_Supervision` 目前仍叫這個名字」「擴大後的 Supervision 正式名稱為 JN1 Unified Oversight Authority（JN1-UOA），監管對象是 JN1 Unified Operations Domain（JN1-UOD）...兩個名稱都是本 repo 應 Stephen 要求獨立提案、由 Stephen 選定，尚未在任何來源 repo 落地」。**這段描述的是本 repo 自己的提案狀態，不是對 `0_JN1_AERIS_Supervision` 現有內容的驗證性描述**，不能當作該 repo 的「願景摘要」引用。

### 前沿檢索
不適用——沒有可驗證的願景基礎，無法做「對照願景」的前沿檢索。第 8 節（JN1-UOA）的前沿檢索是針對本 repo 自己提出的監管概念，跟這裡的 `NOT VERIFIED` 狀態是兩回事，不要混為一談。

---

## 8. JN1-UOA / JN1-UOD（未來統一監管概念）

### 「願景」的性質說明
JN1-UOD（JN1 Unified Operations Domain）與 JN1-UOA（JN1 Unified Oversight Authority）**不是任何來源 repo 自己提出的願景**，而是本 SuperSystem repo 應 Stephen 要求、於 2026-09-26 獨立提案並經 Stephen 選定的正式名稱（見 [Blueprint/18_DECISION_LOG.md](18_DECISION_LOG.md) 第 28 行、[Blueprint/09](09_SUPERBRAIN_COMPUTE_FABRIC.md) 第 25-27 行、[Blueprint/10](10_SUPERVISION_AND_EVIDENCE.md) 第 29-31 行）。範圍定義：**JN1-UOD = SuperBrain 三機硬體 + 跑在其上的 Voice Control/AIECP/AERIS/MEGIS 四個軟體層**；**JN1-UOA = 監管整個 JN1-UOD 的統一監督權威**（AERIS Supervision 擴大後的正式名稱，但尚未在任何來源 repo 落地執行）。

因為這是本 repo 自己的概念、不是既有工程系統，這裡的「前沿研究」問法略有不同：不是「JN1-UOA 現在的做法離前沿多遠」（因為它還不存在），而是「如果真的要建這樣一個監管七個異質系統的統一監督權威，全世界目前最嚴謹的同類架構長什麼樣子」。

### 前沿檢索（2026-09-26）——安全關鍵產業的統一監督架構

- **核能業界的「韌性治理」模式**：核能對「災難性風險 + 需要主動式風險管理」的處理方式，被認為是 frontier AI 治理最直接的類比對象——兩者都涉及「有益但具災難性意外傷害風險的技術，且失效模式可能超出傳統補償機制」。中國核能業界的 AI 驅動創新實踐中的「韌性治理(resilience governance)」模式（2025-2026）值得參考。
- **安全案例(safety case)方法論的侷限**：2026 年 OECD 對 20 個組織的分析發現一個關鍵警訊——**多數組織的安全框架「很少揭露完整方法論，也很少系統性驗證文件記錄的控制措施是否真的對應實際結果」**，即使是「有正式安全框架的領先廠商，往往也缺乏把實際事故連回安全文件」的系統。這對 JN1-UOA 未來設計是直接警示：如果只是把 AIECP/AERIS/MEGIS/Voice Agent/SuperBrain 各自的證據格式統一收集起來（見既有雷達 [#6](22_GLOBAL_TECH_RADAR.md)/[#33](22_GLOBAL_TECH_RADAR.md) 建議的 OpenTelemetry GenAI + Langfuse/Phoenix），**只解決了「格式統一」，沒有解決「文件記錄是否對應實際結果」這個更根本的驗證問題**。
- **監管瓶頸的 agent-to-agent 協定案例**：2026-06 一篇論文以核能為案例研究，探討如何用 agent-to-agent 協定克服監管瓶頸（Overcoming the Regulatory Bottleneck via Agent-to-Agent Protocols: A Nuclear Case Study）——這對 JN1-UOA 未來若要監管跨系統、跨 agent 的協作（例如 AIECP 派工給 AERIS/MEGIS，見既有雷達 [#26](22_GLOBAL_TECH_RADAR.md) A2A 協定）提供一個「監管框架如何跟 agent 協定共同設計」的具體參考案例。
- **Frontier AI 本身的災難性責任框架**：「Catastrophic Liability: Managing Systemic Risks in Frontier AI Development」把核能的「風險-責任」框架映射到 AI 開發本身，這對 JN1-UOA 未來若要處理「哪個 agent 的哪個決定造成了問題、誰該負責」這種歸責問題，是可以借鏡的框架起點，雖然目前仍是學術提案階段。

Sources: [AI is changing biological and nuclear risks; governance must change accordingly (Bulletin of the Atomic Scientists, 2026-06)](https://thebulletin.org/2026/06/ai-is-changing-biological-and-nuclear-risks-governance-must-change-accordingly/) · [Resilience Governance in Nuclear Safety: Insights from China's AI-Driven Innovative Practices (Nuclear Technology journal)](https://www.tandfonline.com/doi/abs/10.1080/00295450.2025.2610157) · [Catastrophic Liability: Managing Systemic Risks in Frontier AI Development (arXiv 2505.00616)](https://arxiv.org/pdf/2505.00616) · [Overcoming the Regulatory Bottleneck via Agent-to-Agent Protocols: A Nuclear Case Study (arXiv 2606.07866)](https://arxiv.org/pdf/2606.07866) · [How the NRC is Preparing for AI Technologies in the Nuclear Industry](https://www.nrc.gov/ai/externally-focused)

### 🟡 建議評估（給 Stephen 的方向參考，不是已執行的架構決策）
JN1-UOA 目前連提案都還只是命名選定（見 Blueprint/18），距離真正設計監管架構還很早。但這輪前沿檢索最重要的發現是：**既有雷達 [#6](22_GLOBAL_TECH_RADAR.md)/[#33](22_GLOBAL_TECH_RADAR.md) 建議的 OpenTelemetry GenAI + Langfuse/Phoenix 只解決了「監管資料格式統一」這一層，這是必要但不是最難的部分**。真正對標核能/航空級監管架構的前沿，是「如何驗證文件記錄的控制措施真的對應到實際結果」（2026 OECD 報告點名的普遍缺口）——這個問題目前全世界都還沒有收斂解法，屬於**🟡 值得長期關注、但現在無現成方案可採用**的前沿方向。建議 Stephen 未來設計 JN1-UOA 時，把「格式統一」跟「控制措施-實際結果對應驗證」明確拆成兩個獨立問題處理，不要誤以為採用 OpenTelemetry 就等於解決了核能/航空級的監管嚴謹度。

---

## 交叉引用索引

| 本節 | 對應既有雷達(#1-41) | 對應風險登錄 |
|---|---|---|
| §1 AERIS | [#4](22_GLOBAL_TECH_RADAR.md)（聲學模擬未知）、[#21](22_GLOBAL_TECH_RADAR.md)（非線性失真AI預測） | — |
| §2 MEGIS | [#3](22_GLOBAL_TECH_RADAR.md)（CadQuery AI生態）、[#22](22_GLOBAL_TECH_RADAR.md)（GD&T自動標註）、[#23](22_GLOBAL_TECH_RADAR.md)（拓樸優化+FEA Surrogate） | — |
| §3 AIECP | [#5](22_GLOBAL_TECH_RADAR.md)/[#16](22_GLOBAL_TECH_RADAR.md)/[#27](22_GLOBAL_TECH_RADAR.md)（SLSA/供應鏈）、[#10](22_GLOBAL_TECH_RADAR.md)/[#15](22_GLOBAL_TECH_RADAR.md)/[#36](22_GLOBAL_TECH_RADAR.md)（獨立驗證） | [17_RISK_GAP_CONFLICT_REGISTER.md](17_RISK_GAP_CONFLICT_REGISTER.md) G-04 |
| §4 Voice Agent | [#1](22_GLOBAL_TECH_RADAR.md)（語音輸入）、[#8](22_GLOBAL_TECH_RADAR.md)（喚醒詞）、[#40](22_GLOBAL_TECH_RADAR.md)（Computer Use GA） | — |
| §5 SuperBrain | [#2](22_GLOBAL_TECH_RADAR.md)/[#19](22_GLOBAL_TECH_RADAR.md)（本地LLM選型）、[#9](22_GLOBAL_TECH_RADAR.md)/[#20](22_GLOBAL_TECH_RADAR.md)（跨機推理） | [14_FAILURE_RECOVERY_AND_RESILIENCE.md](14_FAILURE_RECOVERY_AND_RESILIENCE.md)、[17](17_RISK_GAP_CONFLICT_REGISTER.md) R-01 |
| §6 AERIS Implementation | （繼承 §1） | — |
| §7 AERIS Supervision | `NOT VERIFIED` | — |
| §8 JN1-UOA/UOD | [#6](22_GLOBAL_TECH_RADAR.md)/[#33](22_GLOBAL_TECH_RADAR.md)（監管資料格式） | [10_SUPERVISION_AND_EVIDENCE.md](10_SUPERVISION_AND_EVIDENCE.md) |

要跑哪個專案的更深一層前沿檢索，或針對某個 🟡 項目重新檢索確認是否已有落地產品，直接跟 Claude 說「跑願景雷達：XX」即可。
