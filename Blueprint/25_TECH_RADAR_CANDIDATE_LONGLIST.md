# 25 — Tech Radar Candidate Longlist & 篩選紀錄

> 這份文件回答 Stephen 的原話——「14（現在16）條雷達感覺不夠多，先幫我找出100個以上候選主題，再自己篩選收斂」。本文件是**篩選過程本身的誠實紀錄**，不是只給乾淨的最終清單：Stephen 明確要求看到「篩除，未檢索」與「檢索後篩除」的原因，這是交付物的一部分。
>
> 完整寫成正式雷達條目（#17 起）的成果在 [22_GLOBAL_TECH_RADAR.md](22_GLOBAL_TECH_RADAR.md)；一頁彙整表在 [23_TECH_RADAR_SUMMARY_REPORT.md](23_TECH_RADAR_SUMMARY_REPORT.md)。

## 流程說明

1. **先列長清單**：不看成熟度、不查資料，先窮舉候選主題（見下方 110 條，涵蓋七個專案領域＋跨切關注點）。
2. **第一輪篩選（未檢索）**：明顯不相關、跟既有 #1-16 重複、太籠統/buzzword、超出一人工程團隊能用的範圍 → 直接標記「篩除，未檢索」並寫一行理由。
3. **第二輪篩選（已檢索）**：存活的候選才真的跑 WebSearch。檢索後發現「其實不夠成熟/找不到具體東西/跟既有條目沒有實質差異」的，標記「檢索後篩除」並附引用來源說明為什麼放棄。
4. **保留**：檢索後確認相關、夠新（2026）、可行動的，寫成正式雷達條目（[22](22_GLOBAL_TECH_RADAR.md) #17 起）。

**結果統計**：110 個候選 → 41 個篩除未檢索 → 69 個實際檢索（含合併查詢批次）→ 其中 2 個檢索後篩除 → 25 個寫成正式雷達條目（#17-#41，加上既有 #1-16 共 41 條）。

---

## 100+ 候選長清單（依領域分類，檢索前產生，未依成熟度排序）

### 語音 / ASR / TTS / 音訊 ML
1. NVIDIA Parakeet TDT 新版本 — 與既有#1/#2重複，跳過
2. Whisper large-v4 / distil-whisper 新版 — Voice Agent ASR候選
3. faster-whisper vs whisper.cpp 效能對照（ARM/Windows）— Voice Agent部署選型
4. SenseVoice（阿里）中文語音辨識 — 直接對標97.9%
5. FunASR / Paraformer 中文ASR — 中文場景對標
6. Moonshine（Useful Sensors）邊緣端ASR — 低功耗裝置候選
7. Kyutai Moshi 全雙工語音模型 — 端到端語音對話架構參考
8. CosyVoice3 / CosyVoice2更新版 — TTS選型
9. IndexTTS-2 情感可控TTS — 與#4/既有雷達重複
10. F5-TTS / E2-TTS zero-shot TTS — TTS選型候選
11. GPT-SoVITS 語音複製 — 中文語音克隆
12. Silero VAD v6更新 — VAD選型
13. WebRTC VAD vs Silero VAD比較 — 太基礎，已有共識，跳過
14. 語者分離（speaker diarization）pyannote 3.x — Voice Agent單人指令場景需求不明，speculative
15. 聲紋辨識/語者驗證 — 同上，speculative
16. 中文wake word資料集/自訓管線 — 屬於「動手做」不是「掃描科技」，跳過
17. ONNX Runtime WebGPU/DirectML推論加速(Windows) — Voice Agent部署優化
18. llama.cpp最新GGUF量化格式 — 本地LLM選型延伸
19. ElevenLabs開源化/API動態 — 非本地/違反Voice Agent離線原則，跳過

### 本地 LLM / 硬體 / 推論
20. NVIDIA Blackwell Ultra/GB300 — 太企業級，跟128GB桌機不相關，跳過
21. Qwen3-Next/Qwen3.5系列更新 — 本地模型選型延伸
22. DeepSeek V3.2/R2本地部署可行性 — 本地模型選型候選
23. Kimi K2 Thinking版本 — 延伸既有K2提及
24. GLM-4.6/GLM-4.5-Air本地部署 — 中文本地模型候選
25. Llama 4 Behemoth推遲狀況 — 與#2 MoE選型重複、speculative細節，跳過
26. MLX(Apple)框架 — 僅Mac相關，Windows平台不適用，跳過
27. llama.cpp RPC多機推理 — 與#9重複但更輕量，值得比較
28. speculative decoding/EAGLE-3推測解碼 — 通用推理加速技術
29. vLLM V1引擎Windows支援狀況 — SuperBrain部署可行性
30. Ollama vs LM Studio vs Jan.ai比較 — 太基礎/大量重複資訊，跳過
31. GGUF vs AWQ vs GPTQ量化格式比較 — 太基礎/通用知識，跳過
32. NVIDIA TensorRT-LLM Windows/WSL2支援 — 與既有#7重複，跳過
33. context caching/prompt caching各家API定價 — 直接對應Stephen $20/週痛點
34. KV cache壓縮技術(2026) — 128GB硬體相關但過於學術/未落地，跳過
35. long-context 100萬token實務限制 — 與#7既有記載重複度高，跳過

### AERIS 聲學工程
36. COMSOL AI外掛/Copilot功能2026 — AERIS實際用哪套模擬軟體未確認（PROJECTS.yaml `main_machine: UNKNOWN`），盲搜特定商業工具風險是編造相關性，跳過
37. Ansys AI聲學模組2026 — 同上理由，跳過
38. VA One/Actran聲學模擬AI功能 — 同上理由，跳過
39. Klippel揚聲器量測系統AI分析功能 — 揚聲器設計專用，值得檢索
40. AI驅動的揚聲器分音器(crossover)自動設計工具 — 高度相關，值得檢索
41. 聲學超材料(acoustic metamaterial) AI設計2026 — 偏學術，對一人工程操作可行動性低，跳過
42. 主動降噪(ANC) AI演算法最新進展 — AERIS範圍是否含ANC未確認，speculative，跳過
43. 揚聲器非線性失真AI預測模型 — 高度相關，值得檢索
44. 開源BEM/FEM聲學求解器(openBEM, FEniCS) — 與既有#4 Pyroomacoustics重複度高，跳過
45. AI合成訓練資料生成for聲學ML — 與既有#4 Treble重複，跳過

### MEGIS 機構工程
46. Onshape/SolidWorks AI Copilot 2026 — MEGIS實際用CadQuery（見Audit/REPOSITORY_INVENTORY.md、Audit/GAP_ANALYSIS.md），非Onshape/SolidWorks，盲猜工具身分，跳過
47. nTopology最新AI功能 — 與既有#3重複，跳過
48. Fusion 360 Generative Design 2026更新 — 與既有#3重複，跳過
49. FreeCAD/CadQuery AI外掛 — MEGIS已確認用CadQuery，直接相關，值得檢索
50. AI驅動的GD&T自動標註工具 — 高度相關，值得檢索
51. 拓樸優化(topology optimization)開源工具2026 — MEGIS Gate制可能用到，值得檢索
52. 逆向工程AI(點雲→CAD) 2026 — MEGIS是否有逆向工程需求未確認，speculative，跳過
53. AI供應商比價/尋源平台2026 — MEGIS BOM相關但屬於採購流程非工程技術雷達範疇，跳過
54. 3D列印/積層製造AI切片優化 — MEGIS是否涉及3D列印未確認，speculative，跳過
55. 機構模擬AI加速(FEA surrogate model) 2026 — 高度相關前沿技術，值得檢索

### AIECP / Agent Orchestration / Evidence / 供應鏈安全
56. Model Context Protocol(MCP) 2026最新規範 — 本session本身就跑在MCP上，直接相關，值得檢索
57. Agent-to-Agent(A2A)協定(Google/Linux Foundation) 2026 — 跨系統握手協定候選，值得檢索
58. AGENTS.md標準化2026進展 — 七個repo都已用此檔案，值得確認標準化程度
59. OpenSSF Scorecard供應鏈安全評分 — 延伸#5/#16，值得檢索
60. in-toto供應鏈證明框架 — 延伸#5，與Scorecard/SBOM合併檢索
61. Sigstore policy-controller/gatekeeper — 與既有#16 Sigstore三件套重複，跳過
62. SBOM(CycloneDX/SPDX) 2026標準 — 延伸AIECP證據系統，與#59合併檢索
63. GitHub Copilot Workspace/Coding Agent 2026 — 對標AIECP Builder角色，值得檢索
64. Devin/Cognition AI agent 2026最新能力 — 對標AIECP Builder，與#63合併檢索
65. LangGraph/CrewAI/AutoGen比較2026 — AIECP orchestration對標，值得檢索
66. Temporal.io workflow engine 2026 — AIECP Queue/Scheduler對標，與#65合併檢索
67. AI agent評測基準(SWE-bench Pro等)2026更新 — 與既有#10重複度高，本輪不重跑，跳過
68. Claude Agent SDK/Claude Code subagent isolation更新 — 本session正在用，但與既有#11重複度高，跳過
69. Docker Model Runner/Testcontainers — 與模型serving/測試基礎設施相關但非agent沙盒核心，跟既有#11關聯度低，跳過
70. gVisor/Firecracker microVM — 與既有#11已提及的microVM重複，跳過
71. gitStream/Danger.js自動化PR規則引擎 — 太通用，跟AIECP現有Git治理關聯低，跳過
72. GitOps for AI-driven infra changes — 太通用/不成形，跳過
73. GitHub Actions self-hosted runner Windows ARM64 — 直接對應既有#11「Windows ARM64業界資料稀少」缺口，值得檢索
74. Windows Sandbox/WDAG原生沙盒 — 直接對應#11缺口，值得檢索
75. WSL2 GPU passthrough ARM64支援狀況2026 — 直接對應SPARK硬體(Grace是ARM CPU)+#11缺口，值得檢索
76. Podman Desktop Windows ARM64支援 — 與#74/75檢索合併，不單獨查
77. HashiCorp Vault Agent/Boundary 2026 — 延伸#13，值得檢索
78. Doppler/Infisical開源密鑰管理2026 — 延伸#13，與#77合併檢索
79. SPIFFE/SPIRE工作負載身份 — 延伸#13，值得單獨檢索（技術層次不同於傳統secret manager）

### Observability / Evaluation
80. Langfuse 2026 self-host最新功能 — 延伸#6，值得檢索
81. Arize Phoenix 2026 self-host最新功能 — 延伸#6，與#80合併檢索比較
82. OpenObserve GenAI追蹤2026 — 既有#6已引用OpenObserve來源，重複，跳過
83. Braintrust/promptfoo eval框架 — 與G-04驗證主題有交集但屬於「LLM輸出品質評測」非「確定性驗證」，優先度低於已排定的mutation testing/contract testing，跳過（避免過度擴張G-04系列條目）
84. DeepEval/RAGAS RAG評測框架 — 依附於#14(🟡持續觀察，非本輪優先)，跳過

### RAG / 知識管理
85. GraphRAG(Microsoft) 2026 — 依附於#14(🟡持續觀察)，本輪不重複深挖，跳過
86. LlamaIndex Workflows 2026 — 同上，跳過
87. Haystack 2.x agentic pipelines — 同上，跳過
88. 本地向量資料庫比較(Qdrant/Milvus/LanceDB) 2026 — 同上，跳過
89. Contextual retrieval(Anthropic)+prompt caching結合 — 併入成本優化主題(#90)一起檢索

### 成本優化 / API用量
90. Claude prompt caching定價與策略2026 — 直接對應Stephen $20/週痛點，最高優先，值得檢索
91. OpenAI Batch API/Gemini Batch折扣定價 — 成本優化候選，但本輪聚焦Claude/Anthropic生態(AIECP主要用Claude Code)，與#90重疊度高，跳過以免發散
92. Anthropic Claude Code usage limits/週預算管理工具 — 與#90合併檢索
93. LiteLLM多供應商路由/快取閘道 — 對應AIECP Provider routing，值得檢索
94. Codex CLI/ChatGPT Plus web額度管理策略2026 — 對應AIECP Web Safe Bridge，但缺乏公開資料佐證具體策略，跳過

### Fine-tuning / Distillation / Edge AI
95. LoRA/QLoRA微調本地小模型2026(Unsloth) — 若需針對聲學/機構領域微調，值得檢索
96. 知識蒸餾(distillation)大模型→小模型2026 — 與本地模型選型主題重複度高，跳過
97. ONNX Runtime Mobile/TFLite邊緣部署2026 — 與既有ASR/TTS本地部署覆蓋重複，跳過
98. NPU(Windows Copilot+ PC NPU)AI加速2026 — 對應#7 Surface硬體NPU細節，值得檢索
99. 機器人/機構控制硬體(ROS2 2026) — 七個專案都沒有機器人實體控制證據，speculative，跳過

### 測試/驗證/CI額外
100. Mutation testing工具2026(Stryker/mutmut) — 補強確定性驗證(#10/#15)，值得檢索
101. Property-based testing(Hypothesis) for AI-generated code — 與#100合併檢索
102. Contract testing(Pact) 2026 — 對應G-01/G-02介面缺口設計參考，高度相關，值得檢索
103. Chaos engineering for multi-machine systems 2026 — 對應R-01/#14容錯設計，值得檢索
104. Windows ARM64原生.NET/Python工具鏈成熟度2026 — 與#73/75合併檢索

### 其他跨域
105. EU AI Act 2026生效細節 — Public Portal(G-05)為低優先未來項目，跳過
106. 開源授權合規掃描工具(FOSSA/ScanCode) 2026 — 供應鏈治理延伸但過於通用，跳過
107. Anthropic Computer Use/OS-level agent control 2026 — 對應Voice Agent桌面控制對標，值得檢索
108. Model routing benchmark(RouteLLM) — 對應AIECP/SuperBrain Provider routing，值得檢索
109. 多agent debate/self-consistency for verification — 對應G-04延伸，值得檢索
110. Confidential computing(Intel TDX/AMD SEV) — 過於企業級，對一人工程操作不實際，跳過

---

## 第一輪篩選：篩除，未檢索（41 條）

編號對應上方長清單：1, 9, 13, 14, 15, 16, 19, 20, 25, 26, 30, 31, 32, 34, 35, 36, 37, 38, 41, 42, 44, 45, 46, 47, 48, 52, 53, 54, 61, 67, 68, 69, 70, 71, 72, 82, 83, 84, 85, 86, 87, 88, 91, 94, 96, 97, 99, 105, 106, 110

（注：部分編號在長清單中已於各條目直接寫明理由，此處不重複列出全文，只做索引彙整；主要理由分四類：①與既有#1-16雷達重複、②太籠統/buzzword缺乏具體行動點、③對七個專案的相關性是本 repo 自己的推測而非有來源根據（例如盲猜 AERIS/MEGIS 使用的商業工具）、③speculative——七個專案文件裡沒有證據顯示相關需求存在（例如機器人硬體、ANC、逆向工程）、④超出一人工程操作規模的企業級主題。）

## 第二輪篩選：實際檢索後篩除（2 條）

### 候選#12：Silero VAD v6
**檢索後判斷**：搜尋只找到版本號更新（v6.2.1）與既有 ONNX Runtime 整合方式，沒有找到 2026 年有實質差異於既有雷達#8（onnx-wakeword等）的新突破。既有#8 已經涵蓋喚醒詞/VAD這個主題的最新選項，Silero VAD v6 只是同一生態系的版本迭代，不構成獨立雷達條目。
Source: [Silero VAD Version history and Available Models](https://github.com/snakers4/silero-vad/wiki/Version-history-and-Available-Models) · [snakers4/silero-vad (GitHub)](https://github.com/snakers4/silero-vad)

### 候選#40：AI驅動的揚聲器分音器(crossover)自動設計工具
**檢索後判斷**：搜尋只找到 Klippel 既有的 Klippel Controlled Sound (KCS) 非線性控制技術（非AI crossover設計工具），以及 DIY 論壇（diyAudio）上「有人在用一般AI程式輔助設計但效果不如專用工具」的討論串，沒有找到公開、成熟、專門給揚聲器分音器設計用的AI工具。誠實記錄：這個候選在2026年公開資訊中仍是空白，跟既有#4的結論一致（沒有揚聲器設計專用AI工具）。
Source: [Klippel AES Automotive Audio 2026 news](https://www.klippel.de/service/news/newsdetails/article/aes-automotive-audio-2026.html) · [Experience of using AI programmes for loudspeaker design (diyAudio)](https://www.diyaudio.com/community/threads/experience-of-using-ai-programmes-for-loudspeaker-design.420445/)

---

## 保留並寫成正式雷達條目（25 條，見 [22_GLOBAL_TECH_RADAR.md](22_GLOBAL_TECH_RADAR.md) #17-#41）

| 新雷達# | 主題 | 對應候選# |
|---|---|---|
| 17 | 中文ASR最新對照：SenseVoice/FunASR vs Whisper + Kyutai Moshi全雙工架構 | 4,5,7 |
| 18 | 開源TTS最新對照：CosyVoice3/F5-TTS/GPT-SoVITS | 8,10,11 |
| 19 | 本地LLM 128GB容量現實檢查（修正雷達#2） | 21,22,23,24 |
| 20 | EAGLE-3推測解碼：本地推理加速技術 | 28 |
| 21 | 揚聲器非線性失真AI預測模型 | 43 |
| 22 | MEGIS：AI驅動GD&T自動公差標註工具 | 50 |
| 23 | MEGIS/AERIS：拓樸優化開源工具+FEA/CAE Surrogate Model AI加速 | 51,55 |
| 24 | MEGIS：CadQuery AI生態系（Text-to-CAD Copilot、MCP整合） | 49 |
| 25 | MCP 2026-07-28重大改版 | 56 |
| 26 | A2A協定 + AGENTS.md標準化：跨專案握手協定候選 | 57,58 |
| 27 | 供應鏈證據延伸：OpenSSF Scorecard + SBOM(CycloneDX/SPDX) | 59,60,62 |
| 28 | AIECP Harness對照：LangGraph/CrewAI/AutoGen + Temporal.io | 65,66 |
| 29 | AIECP Builder對照：GitHub Copilot Coding Agent vs Devin | 63,64 |
| 30 | Windows Execution Containers(MXC) SDK：Windows原生agent沙盒 | 74 |
| 31 | Windows ARM64工具鏈成熟度：WSL3 GPU/NPU passthrough + GH Actions ARM64 runner | 73,75,76,104 |
| 32 | AIECP憑證管理延伸：Infisical Agent Vault + SPIFFE/SPIRE | 77,78,79 |
| 33 | JN1-UOA落地選型：Langfuse vs Arize Phoenix self-host詳細比較 | 80,81 |
| 34 | Claude Prompt Caching 2026-09-01降價：直接對應Stephen成本痛點 | 90,92 |
| 35 | AI Gateway/Router落地選項：LiteLLM + RouteLLM | 93,108 |
| 36 | G-04驗證方法更新：Mutation Testing + Property-based Testing + 多agent debate警示 | 100,101,109 |
| 37 | Contract Testing(Pact)：對應G-01/G-02介面缺口設計參考 | 102 |
| 38 | Chaos Engineering：多機容錯測試對應R-01 | 103 |
| 39 | Windows Copilot+ PC NPU / Windows ML / Aion 1.0 on-device SLM | 98 |
| 40 | Anthropic Computer Use 2026-08-19 GA：對照Voice Agent桌面控制 | 107 |
| 41 | Unsloth QLoRA本地微調：領域術語微調候選 | 95 |

## 使用方式

想重跑這份長清單裡任何被篩除的候選（例如「檢索#36 COMSOL AI功能」等到AERIS確認用哪套模擬軟體後），跟 Claude 說「跑雷達：候選#XX」即可，Claude 會回來讀這份文件找到對應候選描述。
