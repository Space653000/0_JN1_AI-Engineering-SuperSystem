# External Research — 業界參考架構（僅供參考，不代表任何來源 repo 或本 repo 的規範）

依 Stephen 要求，針對本 SuperSystem 已識別的關鍵分工問題（C-04/D-01：AIECP vs SuperBrain 重疊；G-04：SPARK-AGAVE-4 獨立驗證 SPARK-AGAVE-3），檢索 2026 年業界對「多 Agent 系統控制平面」與「Builder/Reviewer/Verifier 分離」的公開資料，作為 Stephen 後續自行決策/設計時的參考輸入。**這裡只是外部研究摘要，不是本 repo 對任何來源 repo 下的指令或決策。**

## 1. Agent Orchestration Layer vs Control Plane vs Compute/Knowledge Fabric

2026 年業界（IBM、VentureBeat、Gartner 等口徑）把「Agent Orchestration」與「Agent Control Plane」視為相關但不同的兩層：

- **Orchestration（協作）**：執行期的動態協調——規劃任務、路由工作、協調多個 agent、管理 workflow 狀態。
- **Control Plane（控制平面）**：提供 orchestration 之外的治理、可觀測性、身分、生命週期管理的完整平台；具體控制點包括 routing、scoped identities、tool permissions、approval gates、state mediation、observability、rollback。
- **Knowledge Fabric（知識織理）**：把企業內各種資料來源整合成一層，讓所有 agent 都能存取一致、非過時的資訊，避免各自為政。

**與本 SuperSystem 現況的對照**：這個三層劃分，跟 Stephen 2026-09-25 裁定的「AIECP = 控制平面（Queue/Router/Worker/Evidence/Approval）、SuperBrain = 跨機統籌規劃層」方向**是一致的**——業界確實把「控制平面」與「執行期協調/資源調度」視為可以分離、但又需要協同的兩個角色，而不是要求合併成一套。這算是外部佐證，不是新增規則。

Sources: [Agent Orchestration 101 (Lyzr)](https://www.lyzr.ai/blog/agent-orchestration/) · [Multi-Agent Orchestration as the New ITOps Control Plane (Happiest Minds)](https://www.happiestminds.com/blogs/multi-agent-orchestration-as-the-new-itops-control-plane/) · [AI Agent Orchestration in 2026 (Viston)](https://viston.tech/ai-agent-orchestration-in-2026-moving-from-pilots-to-enterprise-wide-execution/)

## 2. Builder/Reviewer/Verifier 分離（跟 G-04 相關的參考模式）

- **Verifier Pattern（獨立驗證者模式）**：獨立的 verifier agent 在**沒有** generator 的 context、推理過程、中間步驟存取權限的前提下，只憑「原始需求 + 產出物」做判定，回傳結構化 pass/fail。目的是避免用同一個模型寫又驗證，保留同樣的偏誤。
- **Maker-Checker / Separation of Duties**：源自銀行業內控（至少兩人才能完成一筆交易），Clark-Wilson model 給出正式理論基礎，NIST AC-5 把「職責分離」規範化，目的是不需共謀就能降低惡意/錯誤風險。
- **2+N Team Pattern**：兩個人類監督角色（HI-CTRL）+ N 個專職 agent，producer 模式不能碰 merge 工具，reviewer 模式不能碰 edit 工具——形式化「生產者不能自我核准」。
- Anthropic 官方對 subagent 的建議也是類似方向：獨立的 context window、system prompt、工具權限，讓一個 agent 負責實作、另一個只做唯讀審查。

**與本 SuperSystem 現況的對照**：這剛好對應到你原始問題 #23（SPARK-AGAVE-4 如何獨立驗證 SPARK-AGAVE-3）與 #24（如何避免 Builder 自己 Review 自己）。業界的共同要件是：**驗證方不能看到執行方的推理過程/中間狀態，只能看「原始需求 + 最終產出」**，而且驗證方在工具權限上要跟執行方物理隔離（不同 context、不同 credential、不能互相修改對方的輸出）。這點可以作為 Stephen 之後設計 G-04 協議時的起手式參考，但**具體協議設計仍由 Stephen 自行決定**，本 repo 不代為設計。

Sources: [What Is the Verifier Pattern in Multi-Agent Systems? (MindStudio)](https://www.mindstudio.ai/blog/verifier-pattern-multi-agent-systems-independent-review) · [Adversarial Code Review: Why the Maker Shouldn't Grade the Checker (Augment Code)](https://www.augmentcode.com/guides/adversarial-code-review) · [Specifying AI-SDLC Processes: A Protocol Language for Human-Agent Boundaries (arXiv 2606.20615)](https://arxiv.org/pdf/2606.20615)

## 3. System-of-Systems 架構框架（跟本 repo 自己的定位相關）

學界對「System of Systems」的既有框架（如 SoSAF、GERAM）強調的共同原則：

- 讓各個成分系統維持**管理與操作上的自治**（loosely coupled），同時建立跨系統的介面/溝通協定。
- 架構文件應關注 stakeholder perspectives、跨系統介面、通訊與協作機制，而不是把各系統合併成單一系統。

**與本 SuperSystem 現況的對照**：這正是本 repo 從一開始就採用的定位（第 10 節「不要過度整合」）——AERIS/MEGIS/AIECP/Voice Agent/SuperBrain 各自保持自治，本 repo 只定義 Ownership/Interface/Handoff/Data Flow。業界既有框架佐證這個方向本身是合理的系統工程做法，不是本 repo 自創。

Sources: [A System of Systems Focused Enterprise Architecture Framework (ResearchGate)](https://www.researchgate.net/publication/262244140_A_system_of_systems_focused_enterprise_architecture_framework_and_an_associated_architecture_development_process) · [System of Systems Architecture Framework (SoSAF) for production industries (ResearchGate)](https://www.researchgate.net/publication/261152210_System_of_Systems_Architecture_Framework_SoSAF_for_production_industries) · [Generalised Enterprise Reference Architecture and Methodology (Wikipedia)](https://en.wikipedia.org/wiki/Generalised_Enterprise_Reference_Architecture_and_Methodology)

## 使用方式與限制

- 這份文件是 2026-09-25 的一次性外部檢索快照，**不是持續追蹤的產業雷達**，之後若要更新需重新檢索。
- 所有對照都只是「這個方向跟業界某個模式相似」，**不構成本 repo 對任何來源 repo 的架構指令**；來源 repo 是否採納，完全由該專案自己的治理流程決定。
- 未包含付費/機構內部資料，僅為公開網路搜尋可得的資料，可信度以其原始來源（部落格、arXiv 預印本、Wikipedia）為準，非同儕審查等級的證據。
