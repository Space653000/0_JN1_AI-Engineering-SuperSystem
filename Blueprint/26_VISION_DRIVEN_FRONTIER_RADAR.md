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

> **Stephen 補充定位（2026-09-27）**：AERIS 的目標是把「一個聲學工程師應該具備的全方位能力」完整盤點出來——不是只做現在 GATE 架構裡已經涵蓋的部分。現在盤點不出來、還做不到的能力項目，**未來會分別交給對應的子藍圖與工程專案去逐步達成**，不是 AERIS 自己一次做完。這代表本節「前沿還有多遠」的討論，除了現有兩軌，還要放進「AERIS 有沒有把能力範疇本身盤點完整」這個第三個問題——這是 Stephen 對 AERIS 定位的補充說明，不是來源 repo 文件裡目前寫出來的內容，本 repo 誠實標註來源為 Stephen 口頭確認，非 repo 文件引用。

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

> **Stephen 補充定位（2026-09-27）**：跟 AERIS 一樣，MEGIS 的目標是把「一個機構工程師應該具備的全方位能力」完整盤點出來，不是只做現有 Gate 架構已經涵蓋的部分。現在做不到的能力項目，**未來一樣交給對應的子藍圖與工程專案去逐步達成**。同上，這是 Stephen 口頭補充的定位說明，不是 `README.md`/v3.0 藍圖目前寫出來的文字，特此標註來源區別。

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

> **Stephen 補充定位（2026-09-27）**：AIECP 的目標是**跨領域**協調機構工程師（MEGIS）與聲學工程師（AERIS）的全方位能力——負責協調、溝通、接收指令、分配交付、以及銜接本地與雲端資源協助。也就是說 AIECP 不只是「通用工程控制平面」，而是**專門橋接 MEGIS 與 AERIS 兩個工程領域**的協調層。現在做不到的部分，一樣交給對應的子藍圖與工程專案去逐步達成。這是 Stephen 口頭補充的定位說明，比 `.ai/BLUEPRINT.md` 現有的「通用不綁定領域」文字更具體地指向 MEGIS/AERIS 這兩個特定領域，特此標註來源區別，也呼應 [Blueprint/17](17_RISK_GAP_CONFLICT_REGISTER.md) G-02（AIECP↔AERIS/MEGIS 派工介面缺失）的目標方向。

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

> **Stephen 補充定位（2026-09-27）**：`0_JN1_AERIS_Supervision` 未來會改名，變成監管**全系統的施工進度、監工、與運作狀況**——不再只是 AERIS 一個專案的發布快照監督。這跟 [Blueprint/10](10_SUPERVISION_AND_EVIDENCE.md)、[Blueprint/18 決策記錄](18_DECISION_LOG.md) 已經記錄的 JN1-UOA（JN1 Unified Oversight Authority）方向一致，這裡是 Stephen 再次口頭確認，非新決策，記錄下來以保持一致性。

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

## 9. AERIS 深挖：聲學工程師完整能力地圖對照

### 願景摘要
延續第 1 節已引用的兩份來源（`0_JN1_AERIS/README.md` 第 1 行「一人抵百人」、`docs/AERIS_BLUEPRINT_ZH_TW.md` 第 11-13 行），加上 Stephen 2026-09-27 的補充定位——「把一個聲學工程師應該具備的全方位能力完整盤點出來，不是只做現在 GATE 架構已經涵蓋的部分」（引用細節見第 1 節，此處不重複）。本節不重複那兩份引用，只針對這句補充定位本身，問一個新問題：**世界上有沒有一份「聲學工程師的完整能力地圖」可以拿來對照 AERIS 的 100 席位設計，看有沒有漏掉的能力範疇？**

> **Stephen 補充澄清（本輪任務中途）**：AERIS 的聲學工程範疇明確**同時包含揚聲器與麥克風**，不是只有喇叭/驅動器設計。因此本節的前沿檢索與後面的落差評估，麥克風工程（換能器設計、MEMS 麥克風、陣列/波束成形、麥克風端心理聲學、麥克風校準/量測標準）必須跟揚聲器工程並列處理，不能只當附註帶過。

### 前沿檢索（2026-09-26）

- **沒有單一、官方、涵蓋全部的「聲學工程師能力地圖」——能力被拆在至少三個性質不同的專業組織裡**：(a) INCE-USA（Institute of Noise Control Engineering）的 Board Certification 專業考試，涵蓋範圍明確寫著「fundamental acoustics、mechanical dynamics、psycho-physiological properties of the ear」，再加上「instrumentation and measurements、hearing conservation、noise problems in buildings、transportation systems、community、industry」——這是一份以「噪音控制」為核心、但涵蓋心理聲學與量測的實務能力清單。(b) ASA（Acoustical Society of America）不辦單一能力考試，而是用 **14 個 Technical Committee** 劃分整個聲學領域：Acoustical Oceanography、Animal Bioacoustics、Architectural Acoustics、Biomedical Acoustics、Engineering Acoustics、Musical Acoustics、Noise、Physical Acoustics、Psychological and Physiological Acoustics、Signal Processing in Acoustics、Speech Communication、Structural Acoustics and Vibration、Underwater Acoustics 等——這張清單比 INCE 考試範圍廣得多，涵蓋生醫、水下、動物生物聲學這些 AERIS 願景語言完全沒提到的次領域。(c) AES（Audio Engineering Society）則專注在電聲/訊號鏈這一段：electroacoustic transducer（麥克風、揚聲器陣列、輻射阻抗、聲學中心）、現代揚聲器音箱設計、sound reinforcement（擴聲系統工程）。
- **ABET 沒有把「聲學工程」當成獨立受認證學門**：查詢 ABET 2025-2026／2026-2027 工程認證準則，只在「ocean engineering」這個學門的準則裡明確要求涵蓋 underwater acoustics，沒有找到任何獨立的「Acoustical Engineering」認證準則——這代表**全世界高等教育體系本身也沒有把「聲學工程師」當成一個邊界清楚、有官方統一能力清單的獨立職業來認證**，聲學能力普遍被拆進機械/電機/建築工程系所裡當選修或次專業。
- **麥克風工程本身也有一套獨立於揚聲器的成熟標準/能力範疇，容易被「聲學工程」這個籠統說法蓋過去**：量測級麥克風的校準與規格，由 IEC 61094 系列標準明確定義——IEC 61094-1（互易法初校）、IEC 61094-4（實驗室用工作標準麥克風規格）、IEC 61094-5（現場用工作標準麥克風、以已知靈敏度麥克風比對校準）；麥克風陣列/波束成形這一塊，前沿做法要求陣列裡每顆麥克風的靈敏度與相位響應要緊密匹配，2026 年的具體技術包括差分波束成形演算法（differential beamforming）與均勻圓形陣列（Uniform Circular Array）搭配 DAS/EF-DAS 演算法做全向覆蓋。**這代表「麥克風工程」至少要單獨盤點三塊能力：換能器/MEMS 麥克風設計、量測校準（IEC 61094 系列）、陣列與波束成形演算法**——如果只把麥克風當成揚聲器能力地圖的附屬品，很容易漏掉校準標準與陣列信號處理這兩塊揚聲器工程完全不涉及的獨立能力。
- 綜合以上，「聲學工程師的完整能力地圖」這件事本身，在全世界的專業建制裡都是**碎片化、需要跨三個以上專業學會（外加麥克風領域自己的 IEC 標準體系）拼起來才勉強完整**的狀態，不存在一份 AERIS 可以直接拿來對照打勾的官方清單，而且這份清單必須明確拆出「揚聲器」與「麥克風」兩條平行的能力線，不能把麥克風當成揚聲器的附屬能力。

Sources: [INCE-USA Board Certification — Requirements](https://www.inceusa.org/board-certification/requirements/) · [Institute of Noise Control Engineering](https://www.inceusa.org/) · [Acoustical Society of America — Technical Committees and Administrative Committees](https://acousticalsociety.org/technical-committees-and-administrative-committees/) · [Acoustical Society of America — Scopes of Technical Committees and Technical Specialty Groups](https://acousticalsociety.org/scopes-of-technical-committees-and-technical-specialty-groups/) · [AES — Acoustics and Sound Reinforcement](https://aes.org/aes-acoustics-and-sound-reinforcement/) · [ABET — Criteria for Accrediting Engineering Programs 2025-2026](https://www.abet.org/accreditation/accreditation-criteria/criteria-for-accrediting-engineering-programs-2025-2026/) · [Measurement Microphone Guide: Types, Specs & How to Choose (CRYSOUND)](https://www.crysound.com/blog/measurement-microphone-guide/) · [MEMS measurement microphone compatible to P48 amplifiers (PMC/NCBI)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC11841207/) · [Beamforming using Digital Piezoelectric MEMS Microphone Array (arXiv 2111.10087)](https://arxiv.org/pdf/2111.10087) · [Microphone Array Beamforming with Optical MEMS Microphones (audioXpress)](https://audioxpress.com/article/microphone-array-beamforming-with-optical-mems-microphones)

### 🟡 建議評估
這輪檢索最重要的發現不是「找到一份 AERIS 該對照的能力清單」，而是**這份清單世界上根本不存在單一版本**——這對 AERIS「完整盤點聲學工程師全方位能力」的新定位是一個關鍵的方法論提醒：與其去找一份不存在的官方標準來對照打勾，不如**自己把 INCE（噪音控制/心理聲學/量測）＋ ASA 14 個技術委員會（涵蓋建築、電聲、結構振動、水下、生醫、音樂聲學等更廣的物理聲學範疇）＋ AES（電聲換能器/擴聲系統）＋ IEC 61094 麥克風校準/陣列標準體系，四份清單交集起來，自建一份 AERIS 專屬的能力地圖草稿**，再標註 AERIS 現有 100 席位設計對照到這份草稿的哪些格子、哪些格子目前是空的。可以立刻觀察到的三個明顯空格（僅供 Stephen 參考，`UNKNOWN`，因為本 repo 未取得 AERIS 100 席位清單本身的內容確認）：**ASA 的 Underwater Acoustics/Animal Bioacoustics/Acoustical Oceanography 這幾個次領域，幾乎確定不在 AERIS 消費性揚聲器/耳機/麥克風導向的願景範圍內，這應該是刻意排除而非疏漏；INCE 明確點名的 hearing conservation（職業聽力保護）與 noise problems in buildings/community（建築/社區噪音法規），如果 AERIS 願景以「消費性聲學產品的聲學工程」為主，這兩塊法規遵循能力有沒有被涵蓋值得核對；第三個空格是本輪因 Stephen 澄清才浮現的——**既然 AERIS 明確涵蓋麥克風，IEC 61094 麥克風校準/量測能力與陣列波束成形演算法這兩塊獨立於揚聲器的能力，AERIS 100 席位清單裡有沒有對應到專門的席位，而不是被籠統歸進「聲學模擬/量測」這種泛用分類裡，這是 Stephen 核對清單時值得特別留意的一格**。這是本節能提供最直接可執行的下一步，而不是繼續往外找更多論文。標記 🟡：能力地圖盤點方法已經找到（交集四份清單），但實際盤點結果需要 Stephen 對照 AERIS 自己的 100 席位清單才能完成，本 repo 無法代為判定。

---

## 10. AIECP 深挖：多學科設計優化（MDO）作為跨領域協調的前沿參照

### 願景摘要
延續第 3 節引用（`.ai/BLUEPRINT.md` 產品不可退讓原則）與 Stephen 2026-09-27 補充定位：AIECP 專門協調 MEGIS（機構工程）與 AERIS（聲學工程）兩個工程領域，負責「協調、溝通、接收指令、分配交付、銜接本地與雲端資源」（引用細節見第 3 節）。本節問：這個「兩個工程領域必須共用同一組設計變數、彼此的決定互相影響」的協調問題，航空/汽車產業有沒有現成的、跑了幾十年的方法論可以參照？

### 前沿檢索（2026-09-26）

- **MDO（Multidisciplinary Design Optimization，多學科設計優化）正是為這個問題而生的學門**：MDO 處理的正是「結構、氣動、聲學、熱管理等多個工程領域，不能各自關起門優化，因為某個領域改一個設計變數，其他領域的表現也會跟著變」這個結構性問題——這跟 AIECP 要協調 MEGIS（機構/結構面）與 AERIS（聲學面）、兩邊可能對同一個外殼/腔體幾何有不同要求，是同一種問題形狀。
- **MDO 架構具體怎麼分工/協調共用設計變數**：文獻把 MDO 架構分成「單體式（monolithic）」跟「多層式（multi-level）」兩大類。單體式包含 All-At-Once（AAO）、Individual Discipline Feasible（IDF）、Multidisciplinary Feasible（MDF）、Simultaneous Analysis and Design（SAND）——特徵是「中央系統一次性同時最佳化所有領域的變數，協調力強但需要更緊的組織控制」。多層式包含 Concurrent Subspace Optimization（CSSO）、Collaborative Optimization（CO）、Bi-Level Integrated System Synthesis（BLISS）——特徵是**「先讓每個領域（子系統）在自己的子問題裡各自最佳化局部設計變數，系統層級再用一個協調機制去管理領域之間共用變數的一致性」**，BLISS 具體做法是把局部設計變數指派給各領域的子問題，系統層級只處理跨領域共用的耦合變數。文獻明確指出：**集中式做法（AAO/SAND）協調效率高但需要更緊的組織控制，分散式做法（IDF/CO）保留了各團隊的自主權，但協調成本（coordination overhead）更高**——這句話幾乎是直接說給 AIECP 聽的：AIECP 若選擇讓 MEGIS/AERIS 各自保有工程自治權（呼應 CLAUDE.md「保持各專案自治」的同一種精神），就必然要付出更高的協調成本，這是 MDO 領域已經量化過的一個明確架構取捨，不是 AIECP 自己遇到的新問題。
- **AI/代理模型正在加速 MDO 的耦合計算，而不是取代協調架構本身**：2026 年 surrogate model（代理模型，例如 Kriging、Gaussian Process、神經網路）被用來取代昂貴的跨領域模擬呼叫，讓原本「每次跨領域迭代要跑好幾天高精度模擬」的瓶頸大幅縮短；2026 年一篇論文明確描述現代航太設計的特徵是「跨領域緊密耦合＋自動化設計空間探索＋大量使用代理模型、多精度策略與不確定性量化」。**這裡的關鍵洞察是：AI 在 MDO 裡扮演的角色是加速個別領域內的模擬/求解，協調架構（MDF/IDF/BLISS 這些）本身沒有被 AI 取代**——這對 AIECP 是一個具體提醒：AIECP 若想用 LLM/AI agent 讓 MEGIS↔AERIS 協調更快，AI 應該加速的是「MEGIS/AERIS 各自領域內部的模擬回饋」，而不是用 AI 去取代「誰的設計變數優先、怎麼收斂共用變數」這層協調邏輯本身——那層邏輯目前業界仍然是明確的數學架構（MDF/IDF/BLISS），不是靠 AI agent 對話「喬」出來的。
- **確實存在扮演協調層角色的軟體**：NASA Glenn Research Center 主導開發的 OpenMDAO，是一個開源的 Python MDAO（Multidisciplinary Design Analysis and Optimization）框架，明確定位為「讓使用者把多個領域、多種精度層級的分析程式碼串接起來」的協調層，用 Newton 類演算法求解耦合系統，並用「模組化分析與統一微分（MAUD）」架構讓大型最佳化問題可以被拆解成各領域的小元件、各自獨立維護——這正是 AIECP 想扮演的「銜接 MEGIS 與 AERIS 兩個領域求解器」的軟體角色的一個已存在多年、跑在真實航太/風機/CubeSat 專案上的具體參照對象，雖然 OpenMDAO 是給數值最佳化用的，不是給 AI agent 派工用的，但它解決「兩個領域的求解器如何交換共用設計變數並保持系統層級一致」的資料流架構，是 AIECP 應該研究的具體範本，而不是只在「agent orchestration」框架（LangGraph/CrewAI 等，見既有雷達 [#28](22_GLOBAL_TECH_RADAR.md)）裡找答案。

Sources: [Multidisciplinary Design Optimization: A Survey of Architectures (MIT)](https://fab.cba.mit.edu/classes/865.18/design/mdo/MDOSurvey.pdf) · [Solving Coordination Challenges — Multidisciplinary Design Optimization (MDO) Architectures (Block Science)](https://blog.block.science/multidisciplinary-design-optimization-architectures/) · [Extensions to the Design Structure Matrix for the Distributed IDF Architecture (Lambe & Martins, University of Michigan MDO Lab)](https://public.websites.umich.edu/~mdolaboratory/pdf/Lambe2012a.pdf) · [OpenMDAO: An open-source framework for multidisciplinary design, analysis, and optimization (Structural and Multidisciplinary Optimization, Springer)](https://link.springer.com/article/10.1007/s00158-019-02211-z) · [GitHub — nasa/OpenMDAO-Framework](https://github.com/nasa/OpenMDAO-Framework) · [OpenMDAO.org](https://openmdao.org/) · [A Machine Learning Enabled MDO for Bio-Inspired Autonomous Underwater Gliders (arXiv 2602.08508)](https://arxiv.org/pdf/2602.08508) · [Generative Artificial Intelligence in Aircraft Design Optimization (MDPI, 2026)](https://www.mdpi.com/2227-9717/14/4/719)

### 🟢 建議評估
這是本輪三條線裡**最值得 Stephen 認真看的一條**。AIECP 現在面對的「MEGIS 跟 AERIS 兩個工程領域，怎麼在同一個設計上協調、誰的變數優先、怎麼避免各做各的」這個問題，**不是一個全新問題，而是航太/汽車產業已經用 MDO 這套學門處理了三十年以上的標準問題**，而且已經有跑在真實專案上的開源協調框架（OpenMDAO）可以參照它的資料流設計，不用只在通用 AI agent 編排框架（LangGraph/CrewAI/AutoGen，見既有雷達 [#28](22_GLOBAL_TECH_RADAR.md)）裡找答案。具體建議：AIECP 不需要真的去裝 OpenMDAO（它是給連續型數值最佳化用的，AIECP 面對的是工程判斷/任務指派，不是逐次迭代求解），但**設計 AIECP 的 MEGIS↔AERIS 派工/協調介面時，可以直接借用 MDF vs IDF vs 分散式協調的架構取捨語言**：如果 AIECP 希望 MEGIS/AERIS 各自維持工程判斷自治權（呼應 CLAUDE.md「保持各專案自治」的精神，也呼應 Stephen 對兩專案自主治理的一貫要求），那麼分散式協調（類似 IDF/CO 的精神：各自在自己的子問題裡決策，只在系統層對齊共用變數）會比中央集權式（AAO）更符合 AIECP 現有的治理原則，但代價是協調成本更高、需要更明確的「共用設計變數」定義機制——這正好對應到既有風險登錄 [17_RISK_GAP_CONFLICT_REGISTER.md](17_RISK_GAP_CONFLICT_REGISTER.md) G-02（AIECP↔AERIS/MEGIS 派工介面缺失）：**MDO 架構理論給了一個現成的詞彙，可以拿來具體定義「共用設計變數是什麼、誰擁有它、怎麼收斂衝突」這幾個目前 G-02 還沒回答的問題**，這比從零發明一套協調協定效率高很多。

---

## 11. Voice Agent 深挖：雙向澄清對話的前沿

### 願景摘要
延續第 4 節引用（`README.md`「100% 離線」、`.ai/BLUEPRINT.md`「完全離線——正式運作階段禁止任何雲端 API」）與第 4 節已指出的落差：「雙向澄清對話」是 Voice Agent 目前規劃沒有涵蓋、但 2026 年輔助人機互動研究認為的下一個前沿。本節針對這一個落差本身往下挖，問兩個更具體的問題：(a) 2026 年「澄清式對話」的技術現況實際長什麼樣子（不只是「機器人應該要會問」這句話）；(b) 這件事有沒有可能在完全離線、不靠雲端 LLM 的情況下做到——這直接關係到 Voice Agent 自己「正式運作階段禁止任何雲端 API」的硬性限制。

### 前沿檢索（2026-09-26）

- **「澄清式對話」在 2026 年已經有具體的三段式技術路徑，不是抽象概念**：韓國高麗大學團隊的 CLARA（Classifying and Disambiguating User Commands for Reliable Interactive Robotic Agents，IEEE Robotics and Automation Letters 2024，持續被引用到 2026 年）提出一個具體流程：①用 LLM 的不確定性估計（uncertainty estimation）判斷一句指令是「清楚」還是「不確定」；②若不確定，再分類是「語意模糊（ambiguous）」還是「根本做不到（infeasible）」；③只有對「模糊」的指令才用 LLM 生成問題去反問使用者澄清——**這個分層設計本身就是重點：不是每一句聽不懂的話都要反問，要先判斷「聽不懂的原因」，只有語意模糊才值得花一輪對話去問，指令根本做不到就該直接回報做不到，而不是徒勞地反問**。
- **2026 年最新研究把「澄清對話」跟具體場景綁得更緊**：PARAssist（2026-08）是針對「使用者請求本身就模糊」的個人化/適應性機器人協助框架；「Take That for Me」（2025-08）處理的是「使用者說『拿那個給我』但沒指到具體是哪個東西」這種**指示詞消解（exophora resolution）**問題，做法是機器人主動用多模態（視覺＋語言）反問來縮小範圍——這比 CLARA 更進一步，把「反問」跟「看得到什麼」綁在一起判斷，這對 Voice Agent 未來若要加 Vision fallback（藍圖 P5 階段）有直接參考價值。
- **離線可行性是分岔的，關鍵在於「要不要用完整 LLM 做澄清判斷」**：2026 年的實務教學文章（非學術論文，但反映實際落地做法）明確指出，一套完全離線的語音助理現實配置是「Whisper（ASR）＋ 3B-4B 等級本地 LLM（如 Phi-4 Mini 或 Gemma 3 4B）＋ Piper（TTS）」，而且**明確提到「指示本地小型 LLM 在執行任何非簡單任務前先反問澄清問題」這個做法本身可行、且能改善小模型表現**——這代表「澄清對話」不是只有雲端大模型才做得到的能力，3B-4B 這個量級的本地小模型，只要用對的 prompt 策略（要求模型先判斷是否有缺漏參數、缺漏就先問），就有機會在完全離線的情況下做到 CLARA 論文描述的「不確定性判斷→分類→反問」這個流程的簡化版，不需要雲端等級的模型。
- **但學術論文級的「不確定性估計」方法本身，大多還是驗證在雲端等級 LLM 上**：CLARA、PARAssist 這類論文的實驗，用的是能做細緻不確定性估計/zero-shot 情境推理的大型 LLM，還沒有看到專門驗證「3B-4B 本地模型做這套三段流程準確度有多少」的對照實驗——這代表「離線澄清對話」目前是**「工程上可行、但學術驗證的準確度數字大多來自更大的模型」這種誠實的落差**，Voice Agent 若要做，需要自己在本地小模型上重新驗證這套流程的可靠度，不能直接照搬論文數字。

Sources: [CLARA: Classifying and Disambiguating User Commands for Reliable Interactive Robotic Agents (arXiv 2306.10376 / IEEE RA-L 2024)](https://arxiv.org/abs/2306.10376) · [CLARA project page](https://clararobot.github.io/) · [PARAssist: A Framework for Personalized and Adaptive Robotic Assistance from Ambiguous User Requests (arXiv 2608.24905)](https://arxiv.org/pdf/2608.24905) · [Take That for Me: Multimodal Exophora Resolution with Interactive Questioning for Ambiguous Out-of-View Instructions (arXiv 2508.16143)](https://arxiv.org/pdf/2508.16143) · [Local Voice Assistant Whisper + LLM Phone 2026 (PromptQuorum)](https://www.promptquorum.com/power-local-llm/voice-assistant-local-mobile-offline) · [Local LLMs perform so much better when you teach them to ask before they answer (XDA Developers, 2026)](https://www.xda-developers.com/local-llm-clarifying-questions-system-prompt/) · [Best Local Private Voice AI Assistant for PC in 2026 (InnerZero)](https://innerzero.com/blog/best-local-private-voice-ai-assistant-2026)

### 🟢 建議評估
這輪比第 4 節挖得更深的地方是：「雙向澄清對話」不再只是一個抽象的下一步方向，而是有 **CLARA 這套具體、可拆解實作的三段流程（判斷不確定→分類模糊/做不到→只對模糊的情況反問）**，而且有實務證據顯示**這套流程的簡化版，有機會在 Voice Agent 自己要求的完全離線、3B-4B 級本地小模型上執行**，不需要違反「正式運作階段禁止任何雲端 API」這條硬限制。誠實的落差是：CLARA/PARAssist 這類論文驗證用的模型量級比本地小模型大，「離線小模型做這套流程到底準不準」目前沒有現成的學術數字可以引用，需要 Voice Agent 自己做驗證。**具體建議：等 Voice Agent 完成 P0-P4（核心語音控制迴圈）之後，下一輪如果要評估雙向澄清對話，可以直接把 CLARA 的三段式判斷邏輯（而不是它的完整 LLM 實作）當作設計範本——用一個輕量的「必要參數是否齊全」規則判斷取代 CLARA 論文裡的 LLM 不確定性估計，只在真正缺參數時才觸發本地小模型生成一句反問**，這樣完全不需要引入更大的模型，也不違反離線限制，是一條務實、可以真正落地驗證的路徑，不是純理論方向。

---

## 12. MEGIS 深挖：機構工程師完整能力地圖對照

### 願景摘要
延續第 2 節已引用的來源（`0_JN1_MEGIS/README.md` 第 5 行「將機械、聲學與製造工程師的判斷轉化為可追溯、可驗證、可重現的引導式生成工程流程」、v3.0 藍圖第 56 行）與 Stephen 2026-09-27 的補充定位——「把一個機構工程師應該具備的全方位能力完整盤點出來，不是只做現在 Gate 架構已經涵蓋的部分」（引用細節見第 2 節，此處不重複）。與第 9 節（AERIS）用同一個問法，本節問：**世界上有沒有一份「機械工程師的完整能力地圖」可以拿來對照 MEGIS 現有的 G0-G9 Gate 架構，看有沒有漏掉的能力範疇？**

### 前沿檢索（2026-09-26）

- **機械工程跟聲學工程不同，它是 ABET 明確有獨立認證準則的主流學門**：ABET 對「Mechanical Engineering and Similarly Named Programs」有專屬的 Program Criteria，明確要求課程涵蓋「工程原理、基礎科學與數學（含多變量微積分與微分方程）；把這些原理應用到物理系統/元件/製程的建模、分析、設計與實現；同時涵蓋熱力系統與機械系統；並在熱力或機械系統其中一項做深入涵蓋」——這代表機械工程（不像聲學工程）**確實存在一份全球通用、有官方認證機構背書的最低能力範圍定義**，可以直接拿來當 MEGIS 能力地圖的骨架，而不需要像第 9 節那樣自己交叉拼湊。
- **ASME Vision 2030：業界對機械工程畢業生能力落差的實證調查**：ASME 2008 年成立的 Vision 2030 Task Force，目的是定義「機械工程畢業生要在 21 世紀保持全球競爭力，應該具備哪些知識與技能」，並向超過 1,470 位業界專業人士收集意見，找出機械工程畢業生的優劣勢——調查點名的**關鍵弱點是「實務經驗、溝通能力、系統性思維（systems perspective）」**，而不是任何單一技術領域的知識缺口。這對 MEGIS 是一個特別值得注意的參照點：MEGIS 的 Gate 架構本身處理的正是「確定性工程資料＋受限幾何＋驗證證據」這種**強調可驗證、可重現的紮實工程實務**，某種程度上正好對應業界點名機械工程畢業生普遍缺乏的「實務經驗」這塊，但「系統性思維」（跨系統/跨子系統的整體判斷能力）是否被 MEGIS 的 G0-G9 涵蓋，`UNKNOWN`，需要 Stephen 核對。
- **NSPE 的 Professional Engineering Body of Knowledge**：美國國家專業工程師學會（NSPE）發布的工程師專業能力知識體系文件，是另一份可以交叉比對的通用工程師能力框架，涵蓋範圍比 ABET 課程準則更偏向「執業能力」（例如工程倫理、公眾安全責任、簽證核可責任），這塊「專業執業責任」的能力範疇，跟 MEGIS 自己 Gate 架構裡「具名工程師驗證、可稽核追溯」的治理精神有直接對應，值得作為第二份骨架比對。

Sources: [ABET — Criteria for Accrediting Engineering Programs 2025-2026（含 Mechanical Engineering and Similarly Named Programs 準則）](https://www.abet.org/accreditation/accreditation-criteria/criteria-for-accrediting-engineering-programs-2025-2026/) · [ABET — Criteria for Accrediting Engineering Programs 2026-2027](https://www.abet.org/accreditation/accreditation-criteria/criteria-for-accrediting-engineering-programs-2026-2027/) · [ASME Vision 2030: Helping to inform mechanical engineering education](https://www.researchgate.net/publication/254048600_ASME_vision_2030_Helping_to_inform_mechanical_engineering_education) · [Vision 2030 — Creating the Future of Mechanical Engineering Education (ASEE)](https://strategy.asee.org/vision-2030-creating-the-future-of-mechanical-engineering-education) · [NSPE — Professional Engineering Body of Knowledge (PDF)](https://www.nspe.org/sites/default/files/resources/nspe-body-of-knowledge.pdf) · [A Semantic-Web Oriented Competency Model for Engineering Programs (arXiv 2605.20401)](https://arxiv.org/pdf/2605.20401)

### 🟢 建議評估
跟第 9 節 AERIS 的處境不同，**機械工程的「完整能力地圖」全世界確實有一份相對權威、可直接拿來對照的骨架**：ABET 的 Mechanical Engineering Program Criteria（官方認證準則，涵蓋熱力＋機械系統雙軌）加上 ASME Vision 2030（業界實證調查出來的能力落差清單），兩者交叉起來，比第 9 節 AERIS 要拼湊三個學會清單容易得多、也更有公信力。**具體建議：MEGIS 下次盤點自己 Gate 架構（G0-G9）的能力覆蓋範圍時，可以直接用 ABET Mechanical Engineering Program Criteria 列出的「熱力系統／機械系統雙軌」當第一層骨架，檢查 G0-G9 目前驗證的幾何/治具/聲學薄切片，覆蓋的是機械系統這一軌還是也涉及熱力系統這一軌**（`UNKNOWN`，本 repo 未取得 v3.0 藍圖是否明確處理熱傳/熱應力分析的確認，這是 Stephen 核對時第一個該問的問題）；再用 ASME Vision 2030 點名的「系統性思維」缺口，檢查 Gate 架構裡有沒有一個 Gate 明確負責「跨子系統整合判斷」而不只是逐一驗證單一零件/幾何。標記 🟢：因為 ABET/ASME 這兩份骨架確實存在、公信力高，這比第 9 節 AERIS 的處境更有機會產出具體可執行的落差清單，值得 Stephen 優先安排下一輪盤點。

---

## 13. JN1-UOA 深挖：安全關鍵產業的保證論證（Assurance Case）方法論

### 願景摘要
延續第 8 節已引用的核心發現——2026 年 OECD 對 20 個組織的分析發現「多數組織雖然結合量化指標與專家質性判斷，但很少揭露完整方法論，也很少系統性驗證『文件記錄的控制措施』是否真的對應到『實際結果』」（引用細節見第 8 節，此處不重複）。本節問一個更具體的問題：安全關鍵產業（航空、汽車、醫療器材、核能）幾十年來發展出的**保證論證（assurance case）方法論**——Goal Structuring Notation（GSN）、Claims-Arguments-Evidence（CAE），以及 DO-178C、IEC 61508、ISO 26262、IEC 62304 這幾套正式功能安全標準——有沒有提供一套「把文件記錄的控制措施主張，跟能證明它真的成立的證據，結構化地綁在一起」的具體做法，而不只是「證明控制措施的存在」？這是目前全世界對 JN1-UOA 核心問題最接近的部分解答，即使整體問題仍未解決，本節要挖出「多接近」到底是多接近。

### 前沿檢索（2026-09-26）

- **GSN 的核心結構就是為了不讓「文件存在」跟「證據成立」混為一談而設計的**：GSN 用 Goal（安全/正確性主張）、Strategy（推論方式的說明）、Solution（實際證據項目的引用）三種節點組成一張「論證結構圖」，明確要求把一個籠統的頂層主張（例如「這個系統夠安全」）逐層拆解成子主張，直到每個最底層的子主張都能**直接指向一項具體證據**才算完整——這代表 GSN 本身的設計哲學，就是拒絕「主張沒有連到證據就算數」，跟 AERIS/AIECP 既有的「Evidence Before DONE」精神高度一致，差別是 GSN 提供了一張**可視化、可審查的圖**，而不是一份扁平的日誌或勾選清單。2026 年最新研究（OntoGSN、LLM 自動生成/審查 GSN 相容論證案例）顯示這套方法論正在被進一步工具化，甚至嘗試用 LLM 當「論證案例審查員」自動檢查論證結構是否完整。
- **CAE（Claims-Arguments-Evidence）把「Argument」單獨拉出來當一個不可省略的節點，這是它跟單純「主張+證據」兩欄式做法最大的不同**：CAE 明確定義 Argument 是「連結證據跟主張的推理規則本身」，也就是說光有主張、光有證據都不夠，還必須**明確寫出「為什麼這個證據足以支持這個主張」的推理邏輯**，不能讓讀者自己腦補。ISO/IEC 15026-2:2011 為這套做法提供了標準化支撐。這對 JN1-UOA 很直接的啟示：如果只是把「控制措施存在」跟「某次測試通過」兩件事並排列出來，中間沒有一句話解釋「這次測試為什麼證明了那個控制措施真的在運作」，本質上仍然是 OECD 報告點名的「格式統一但邏輯斷裂」。
- **正式功能安全標準（DO-178C/IEC 61508/ISO 26262/IEC 62304）都要求「雙向可追溯性（bi-directional traceability）」，這是比 GSN/CAE 更早、更成熟、已被強制執行幾十年的做法**：所有這些標準都要求「需求 → 設計 → 實作 → 驗證結果」四者之間可以雙向追溯——不只是「這個需求有沒有被測試到」，還要「這個測試結果對應到哪個需求、有沒有矛盾」。DO-178C 特別明確：證據必須「支撐它被引用的那項驗證目標本身（而不只是沾得上邊）、經得起獨立審查、跟其他生命週期產出物不矛盾、由符合該保證等級（DAL）獨立性與嚴謹度要求的流程產出」——這已經是一套非常具體、可操作的「證據是否真的成立」判準，比 OECD 報告點名「多數組織缺乏的系統性驗證」精確得多，只是這套判準目前主要活在航空/汽車/醫療器材這些領域的正式驗證流程裡，還沒有被證明可以直接套用到「監管多個異質 AI 工程系統」這種 JN1-UOA 面對的場景。
- **誠實的落差**：即使是 DO-178C 這套全世界最嚴謹的軟體驗證追溯標準，處理的仍然是「單一系統、單一保證等級、事先定義好的需求集合」這種相對封閉的問題；JN1-UOA 要面對的是「監管五個異質、各自治理、持續演化的系統（Voice Agent/AIECP/AERIS/MEGIS/SuperBrain）」，這比任何單一 DO-178C/ISO 26262 專案的範圍都更開放、更動態。這正是 2026 年那篇《Fifty Years of Specification Completeness》論文（第 8 節已引用）指出的：航空認證五十年經驗告訴我們的是「規格完整性本身有結構性上限（epoch limits）」，不是「只要抄航空的做法就能解決 AI 治理的驗證問題」。

Sources: [Goal Structuring Notation - a short introduction (modeling-languages.com)](https://modeling-languages.com/goal-structuring-notation-introduction/) · [Graphical safety assurance case using Goal Structuring Notation (GSN) — challenges, opportunities and a framework for autonomous trains (ScienceDirect)](https://www.sciencedirect.com/science/article/pii/S0951832022005488) · [OntoGSN: An Ontology-Based Framework for Semantic Management and Extension of Assurance Cases (arXiv 2506.11023)](https://arxiv.org/pdf/2506.11023) · [LLMs as Judges: Toward The Automatic Review of GSN-compliant Assurance Cases (arXiv 2511.02203)](https://arxiv.org/pdf/2511.02203) · [CAE | Adelard](https://www.adelard.com/asce/cae/) · [Guidance on the Assurance of Machine Learning in Autonomous Systems (AMLAS)](https://www.york.ac.uk/media/assuring-autonomy/documents/AMLASv1.1.pdf) · [Functional Safety: ISO 26262, IEC 61508, ASIL, Safety Case (itemis)](https://www.itemis.com/en/compliance-intelligence/functional-safety/) · [Evidence-First Design: Traceability, Formal Properties, and Certification-Grade Simulation for Safety-Critical Systems (novedge.com)](https://novedge.com/blogs/design-news/evidence-first-design-traceability-formal-properties-and-certification-grade-simulation-for-safety-critical-systems) · [Fifty Years of Specification Completeness: What Aviation Certification Tells AI Governance About Epoch Limits, Proof Surfaces, and the Structural Gap (arXiv 2606.25120)](https://arxiv.org/pdf/2606.25120)

### 🟢 建議評估
這輪檢索找到本輪任務最具體、最可直接借用的一個設計圖案：**JN1-UOA 未來如果要呈現「這裡是我的主張、這裡是支持它的證據鏈」，可以直接借用 GSN 的論證結構圖案（Goal→Strategy→Solution 逐層拆解）當設計模板，而不是像既有雷達 [#6](22_GLOBAL_TECH_RADAR.md)/[#33](22_GLOBAL_TECH_RADAR.md) 建議的那樣，只把 OpenTelemetry/Langfuse 收集到的資料攤成一份扁平日誌**。扁平日誌回答的是「發生了什麼事」，GSN 式的論證圖回答的是「為什麼這件事證明了那項控制措施成立」——後者正是 OECD 報告點名多數組織做不到的那一層。同時要誠實標註：DO-178C/ISO 26262 這類正式標準的雙向追溯要求雖然是全世界最成熟的「把主張連到證據」做法，但都是設計給封閉、單一系統範圍用的，JN1-UOA 面對的是跨五個異質系統的開放式監管，不能直接照搬整套認證流程，只能借用它的「證據必須支撐它被引用的目標、經得起獨立審查、跟其他產出物不矛盾」這三條判準當設計原則。標記 🟢：這是目前全世界對 JN1-UOA 核心問題最接近的部分解答，值得 Stephen 未來設計 JN1-UOA 呈現層時，優先評估 GSN 圖案作為「主張-證據鏈」的具體視覺化模板，而不是繼續假設扁平日誌加監控儀表板就足夠。

---

## 14. SuperBrain 深挖：小規模混合艦隊管理與隔離運算節點模式

### 願景摘要
延續第 5 節已引用的核心願景（「One SuperBrain, Many Replaceable Workers」、「兩台 Spark 盡量不連網，只跑本地 AI」，`0_JN1_2AGAVE128-1MAERA64/.ai/BLUEPRINT.md` 第 10 行、第 1 節，引用細節見第 5 節）與第 5 節已指出的規模落差：前沿討論的 facility edge server 案例是數十到數百台機器的艦隊管理，SuperBrain 是 3 台機器。本節針對這個規模落差往下挖兩個更具體的問題：(a) 真正小規模（2-5台）的家用 AI/機器人實驗室、home-lab 玩家、小型邊緣 AI 部署，2026 年實際怎麼做設定管理、健康監控、故障轉移，而不會過度工程化去上企業級 orchestration？(b) SuperBrain「一台連網 Gateway + 隔離網路運算節點無直接對外連線」這個獨特的架構選擇，有沒有對應的前沿實務可以參考？

### 前沿檢索（2026-09-26）

- **真正小規模（10-20台以下）的邊緣部署，2026 年業界的實際建議是「不要上企業級 orchestration，人工/半自動化就夠」**：2026 年的邊緣裝置艦隊管理討論明確指出「小規模（10-20台）用人工配置就可以管理，只有部署到數百上千台、跨多個地點時，零觸控佈建（zero-touch provisioning）才變得必要」——這比第 5 節找到的「數十到數百台」門檻又更精確了一層，SuperBrain 的 3 台機器規模，連「10-20台用人工管理」這個更低的門檻都還沒到，代表**現階段連考慮 K3s/Kubernetes 這類輕量容器編排都可能是過度工程化**，比第 5 節原本的評估更保守。
- **2026 年的 home-lab AI 實務堆疊已經收斂出一組「小而夠用」的標準組合，不是企業級工具**：設定管理用 Ansible（可讀、冪等、透過 SSH 對目標主機操作、目標端不需要裝 agent）；健康監控的預設選擇是 Uptime Kuma（乾淨的介面加推播通知，是多數 home-lab 玩家每天早上第一個檢查的「服務是否都還活著」儀表板）；需要更細緻指標時才疊加 Prometheus + Grafana（VictoriaMetrics 是更省資源的 Prometheus 替代方案，適合較弱的硬體）；容器化選擇上，Docker Compose 給求簡單的人，K3s 給真的想要 Kubernetes 但不想要它的複雜度的人。**這組合直接對應 SuperBrain 3 台機器的規模：Ansible 做三機一致的設定同步，Uptime Kuma 做「三台機器是否都還活著」的第一層監控，暫時不需要 Prometheus/Grafana 這種更重的指標系統，除非 Stephen 真的需要細粒度的效能追蹤**。
- **SuperBrain「一台連網 Gateway + 隔離網路運算節點」這個架構，在資安/合規世界裡有一個成熟三十年以上的對應模式：Bastion Host / Jump Box**：這個模式的核心定義完全對應 SuperBrain 的設計——「私有節點只能透過 Gateway（jump-box）連線」「一個外部行為者連到 DMZ 裡的專用主機，再從那裡取得對內部網路運算資源的存取權」。這個模式在雲端（AWS/Azure/GCP 的 Bastion Host）與傳統企業內網資安領域行之有年，核心資安慣例是：**Bastion Host 本身盡量精簡（只跑最少必要服務以縮小攻擊面）、集中做存取紀錄與 session 稽核（proxy and log communications）、限制帳號權限**。這對 SuperBrain 的具體參考價值：Laptop 作為唯一連網 Gateway，可以直接借用 Bastion Host 的資安慣例——精簡 Laptop 上對外暴露的服務、對「Laptop→Spark」這段連線做集中的存取紀錄（誰在什麼時候透過 Laptop 對 Spark 下了什麼指令），而不只是把 Laptop 當一般工作機使用。
- **這個模式的規模適用性完全不受限——Bastion Host 本來就是給「內部只有少數幾台機器」的場景設計的，不是規模落差的問題，是 SuperBrain 現有架構本身已經抓對了一個成熟資安模式，只是還沒有明確借用它的具體慣例（精簡攻擊面、集中稽核 log）去強化。**

Sources: [2026 Fleet Device Management: Guide for IoT & Edge Teams (Portainer)](https://www.portainer.io/blog/fleet-device-management) · [The Homelab AI Stack in 2026: What Self-Hosters Are Actually Running (GeniusTechLab)](https://geniustechlab.com/posts/2026-04-28-homelab-ai-stack-2026) · [The 2026 Homelab Stack: What Self-Hosters Are Actually Running This Year (elest.io)](https://blog.elest.io/the-2026-homelab-stack-what-self-hosters-are-actually-running-this-year/) · [What is a Bastion Host? (StrongDM)](https://www.strongdm.com/what-is/bastion-host) · [Access a bastion host by using Session Manager and Amazon EC2 Instance Connect (AWS Prescriptive Guidance)](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/access-a-bastion-host-by-using-session-manager-and-amazon-ec2-instance-connect.html) · [Jumpbox vs Bastion Host vs Azure Bastion 2026 (Exodata)](https://exodata.io/understanding-jumpboxes-bastion-hosts-and-azure-bastion/) · [What is a Bastion Host or Jump Box? in AWS, Azure, GCP (Cloud Infrastructure Services)](https://cloudinfrastructureservices.co.uk/what-is-a-bastion-host-jump-box/)

### 🟢 建議評估
這輪檢索對 SuperBrain 有兩個具體、可直接落地的發現：**第一，SuperBrain 3 台機器的規模連「10-20台用人工管理」這個 2026 年業界認定的最低門檻都還沒到，代表現階段不只是「不用急著上 Kubernetes」（第 5 節已有此結論），連 K3s 這種輕量容器編排都可以先不用考慮，Ansible（設定同步）+ Uptime Kuma（健康監控）這組 home-lab 標準組合就足夠，比企業級方案更貼近 SuperBrain 實際規模，且都是免費開源、單人可維運的工具。第二，這是本輪最重要的新發現**：SuperBrain「Laptop 是唯一連網 Gateway、Spark 隔離不連網」這個獨特架構選擇，**不是 SuperBrain 自創的權宜設計，而是資安/合規世界行之有年的 Bastion Host（堡壘主機/跳板機）模式**，這個模式本來就是為「少數幾台機器、其中一台當唯一對外入口」的場景設計，規模完全不是問題。具體建議：Stephen 可以直接借用 Bastion Host 的兩條資安慣例強化 Laptop 這個 Gateway 角色——(1) 精簡 Laptop 上對外暴露的服務與帳號權限，縮小攻擊面；(2)對「Laptop 對 Spark 下達的每個指令」做集中紀錄與稽核，而不只是把 Laptop 當一般工作機隨意使用。這兩條慣例低成本、不需要新工具，只需要調整使用習慣，且直接呼應 AIECP「證據優於自我宣稱」的精神——建議列入下一輪 SuperBrain 安全設計的具體檢查項。

---

## 15. AERIS Local Implementation 深挖：可重現工程模擬的前沿

### 願景摘要
延續第 6 節已引用的 GATE-06「四方版本一致」要求（Blueprint SHA、Implementation SHA、Local checkout HEAD/dirty digest、Running service loaded SHA，引用細節見第 6 節）與第 6 節已指出的方向：目前靠人工核對 SHA，Proof of Execution / CAVA 這類 runtime attestation 框架是 2026 年研究方向但非現成工具。本節換一個更貼近 AERIS 實際領域的角度往下挖：AERIS 的核心工作是**聲學工程模擬**，不是通用軟體交付，所以真正該問的問題不是「軟體供應鏈怎麼證明沒被竄改」（那是 SLSA/build provenance 的範疇，第 3 節已涵蓋），而是**「給定完全相同的輸入、完全相同的模擬設定、完全相同的程式版本，能不能用密碼學/確定性的方式證明這次模擬結果是可重現的？」**——這是可重現計算科學（reproducible computational science）這個領域幾十年在處理的問題，2026 年的具體工具有哪些可以直接借用？

### 前沿檢索（2026-09-26）

- **可重現性工具鏈已經有三個層次分工明確的技術方向，不是單一工具能解決**：(a) **元資料/描述層**——RO-Crate（Research Object Crate）是把「一次分析執行的所有相關物件（輸入、輸出、程式碼、計算環境、研究者）」打包成一份標準化元資料的做法，2026-08 已發布 1.3 版；其中 **Workflow Run RO-Crate（WRROC）** 這個延伸規範專門處理「工作流程執行的可追溯性」，並進一步拆成三種顆粒度：Process Run Crate（單次工具執行，可以是手動或腳本跑的）、Workflow Run Crate（工作流程系統管理的整次執行）、Provenance Run Crate（工作流程內每個步驟的完整 provenance）——這正好對應 AERIS 可能需要的顆粒度選擇：一次完整聲學模擬跑批可以用 Workflow Run Crate，若要追蹤模擬內部每個步驟（網格劃分→邊界條件設定→求解→後處理）的細節則需要 Provenance Run Crate。(b) **通用 provenance 資料模型層**——W3C PROV 是 2013 年就標準化的 provenance 資料模型（PROV-DM），2026 年仍持續有新工具（例如圖形化 W3C PROV 建模工具）在降低使用門檻，許多模擬平台正在採用它來儲存與管理 provenance；但 W3C PROV 本身是通用資料模型，不是「執行環境」，需要搭配（a）或（c）才能真正落地。(c) **執行環境的位元級可重現（bit-reproducibility）層**——這是三層裡跟 AERIS「模擬結果是否真的可重現」最直接相關的一層：2026 年的做法是用 GNU Guix 建立「從原始碼 bootstrap 就可宣告式、可驗證」的軟體環境，打包成 Apptainer 容器後可以跨不同 HPC 系統執行且達到位元對位元（bit-for-bit）一致的結果（適用於執行緒安全的確定性演算法）；OpenGeoSys（地質工程模擬軟體，應用於地熱系統與放射性廢棄物處置評估）的案例顯示，**只需要對建置流程做幾個小調整（例如鎖定編譯器版本、關閉浮點數重排優化），就能讓建置過程產生跨機器完全相同的執行檔**，這是一個已經在真實工程模擬領域（不是通用軟體）跑過的具體案例。
- **判定「是否可重現」本身也有一個明確、可自動化判定的技術做法**：位元對位元重現的判定基礎是 IEEE-754 浮點數確定性運算，加上避免任何會重新排序浮點數運算順序的編譯器優化；當這兩個條件滿足時，**標準測試的容許誤差（tolerance）可以設為零，讓「是否可重現」變成一個機器可以直接判定 yes/no 的問題，不需要人工判斷「夠不夠接近」**——這對 AERIS GATE-06 現在的「人工核對 SHA」是一個具體的技術路徑升級參考：與其只核對版本號的雜湊值是否相同，若能進一步驗證「同一版本在不同機器上重跑，輸出結果是否位元對位元相同」，會是比對版本號更直接的可重現性證明。
- **這條路徑比 SLSA/generic build provenance 更貼近 AERIS 的實際需求，因為它處理的是「同一份程式碼在不同機器/不同次執行，結果是否真的一樣」，而不是「這份程式碼有沒有被竄改過」——這是兩個不同的問題，AERIS GATE-06 想解決的四方一致，其實同時涉及這兩者，但 Local Implementation 層目前的做法（人工核對 SHA）只碰到了「有沒有被竄改」這一半，沒有碰到「同一份程式碼重跑結果是否一致」這一半**，後者正是聲學模擬這種數值密集型工程領域特有的風險（浮點數運算順序、編譯器優化、硬體差異都可能讓「同一份程式碼」在不同機器上跑出微妙不同的結果，這在通用軟體交付驗證裡完全不會被考慮到）。

Sources: [Recording provenance of workflow runs with RO-Crate (arXiv 2312.07852)](https://arxiv.org/html/2312.07852v1) · [Research Object Crate (RO-Crate)](https://www.researchobject.org/ro-crate/) · [About RO-Crate](https://www.researchobject.org/ro-crate/about_ro_crate) · [Fusion of computational and experimental provenance in RO-Crate (PMC)](https://pmc.ncbi.nlm.nih.gov/articles/PMC13526443/) · [The W3C PROV family of specifications for modelling provenance metadata](https://dl.acm.org/doi/10.1145/2452376.2452478) · [Model-Driven Engineering for Data Provenance: A Graphical W3C PROV Modeling Tool (Springer, 2026)](https://link.springer.com/chapter/10.1007/978-3-031-96841-9_9) · [Unlocking the Potential of Containers in Scientific Computing to Achieve Bitwise Reproducibility, Portability and Performance (Springer)](https://link.springer.com/chapter/10.1007/978-3-031-86240-3_4) · [Reproducible HPC software deployments, simulations, and workflows — a case study for far-field deep geological repository assessment (Environmental Earth Sciences, Springer)](https://link.springer.com/article/10.1007/s12665-025-12501-z) · [Report on Challenges of Practical Reproducibility for Systems and HPC Computer Science (arXiv 2505.01671)](https://arxiv.org/pdf/2505.01671)

### 🟡 建議評估
這輪檢索找到一條**比第 6 節原本提到的 generic runtime attestation（Proof of Execution/CAVA）更貼近 AERIS 實際領域（工程模擬）的具體技術路徑**：可重現計算科學領域已經有「元資料打包（RO-Crate/WRROC）＋ 通用 provenance 模型（W3C PROV）＋ 位元級可重現執行環境（GNU Guix + Apptainer 容器）」這三層分工明確的工具鏈，而且**位元級可重現這一層已經有真實工程模擬案例（OpenGeoSys 地質工程模擬）驗證過可行**，不是純學術構想。具體建議：AERIS Local Implementation 未來若要把 GATE-06 的「四方版本一致」從人工核對 SHA 升級為自動化驗證，比起等待 Proof of Execution/CAVA 這類還在 2026 年才發表的通用 agentic AI 認證框架成熟，**優先評估「用容器鎖定聲學模擬的完整執行環境（編譯器版本、關閉浮點重排優化）+ 用 RO-Crate 的 Workflow Run Crate 打包一次模擬跑批的完整輸入/輸出/環境元資料」這條路徑更務實、更貼近 AERIS 的實際工程模擬需求**——這條路徑不需要等待任何還在研究階段的新框架，GNU Guix/Apptainer/RO-Crate 都是現在就能安裝使用的成熟工具。誠實的落差：這條路徑解決的是「同一版本重跑結果是否一致」，沒有解決「版本本身有沒有被竄改」（那仍然需要第 3/6 節談的 SLSA/build provenance），AERIS 若要完整的四方一致驗證，長期需要把這兩條路徑合起來看，不能只做其中一半。標記 🟡：技術路徑具體可行且有真實案例佐證，但目前 AERIS 聲學模擬工具鏈本身狀態仍是 `UNKNOWN`（見第 1 節），必須先確認 AERIS 實際用哪套模擬軟體，才能評估這條路徑的實際導入成本，屬於「方向已找到、落地時機待 AERIS 自己盤點清楚後再評估」。

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
| §9 AERIS 深挖：聲學工程師完整能力地圖 | 無直接對應（全新能力盤點方法論問題） | — |
| §10 AIECP 深挖：MDO 跨領域協調 | [#28](22_GLOBAL_TECH_RADAR.md)（Agent Harness 對照，層次不同） | [17_RISK_GAP_CONFLICT_REGISTER.md](17_RISK_GAP_CONFLICT_REGISTER.md) G-02 |
| §11 Voice Agent 深挖：雙向澄清對話 | [#1](22_GLOBAL_TECH_RADAR.md)/[#8](22_GLOBAL_TECH_RADAR.md)（延續，非取代） | — |
| §12 MEGIS 深挖：機械工程師完整能力地圖 | [#22](22_GLOBAL_TECH_RADAR.md)/[#23](22_GLOBAL_TECH_RADAR.md)（延續，非取代） | — |
| §13 JN1-UOA 深挖：Assurance Case 方法論 | 延續 §8（同一問題，往下挖具體方法論） | [10_SUPERVISION_AND_EVIDENCE.md](10_SUPERVISION_AND_EVIDENCE.md) |
| §14 SuperBrain 深挖：小規模艦隊管理與隔離節點模式 | 延續 §5（同一規模落差，往下挖具體實務） | [14_FAILURE_RECOVERY_AND_RESILIENCE.md](14_FAILURE_RECOVERY_AND_RESILIENCE.md)、[17](17_RISK_GAP_CONFLICT_REGISTER.md) R-01 |
| §15 AERIS Local Implementation 深挖：可重現工程模擬 | 延續 §6（同一問題，換聲學模擬領域角度往下挖） | — |

要跑哪個專案的更深一層前沿檢索，或針對某個 🟡 項目重新檢索確認是否已有落地產品，直接跟 Claude 說「跑願景雷達：XX」即可。
