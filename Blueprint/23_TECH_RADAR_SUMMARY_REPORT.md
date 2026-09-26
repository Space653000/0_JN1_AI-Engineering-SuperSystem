# 23 — Tech Radar 彙整比較報告

> 這是 [22_GLOBAL_TECH_RADAR.md](22_GLOBAL_TECH_RADAR.md) 41 條雷達的**彙整版**：一張表看完「本地現況 vs 2026 最新科技 vs 本 repo 建議」。長話短說版，完整引用來源請點對應雷達條目。#1-16 是第一輪；#17-41 是 2026-09-26 第二輪（Stephen 要求「先列100+候選再篩選收斂」後的成果，篩選過程見 [25_TECH_RADAR_CANDIDATE_LONGLIST.md](25_TECH_RADAR_CANDIDATE_LONGLIST.md)）。

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
