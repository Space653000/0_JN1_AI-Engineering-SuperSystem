# 22 — Global Tech Radar

> 這份文件是本 repo **建立初衷**的直接實踐（見 [README.md](../README.md)「建立初衷」段落，2026-09-26 Stephen 澄清）：Stephen 每週只有約 $20 額度可用在本地施工，追不上全球 AI 進展速度。本文件的工作是持續往雲端／全世界檢索最新技能、方法、工具、架構，跟本地七個專案現況對標，找出可以借用/仿製的落差，寫成建議——**完全在 cloud session 進行，不涉及修改任何本地或來源 repo**。

## 怎麼用這份文件

- 每一條雷達紀錄都是一次性的檢索快照，標註檢索日期。技術演進很快，**舊條目不會自動更新**，要重新對標時直接跟 Claude 說「更新雷達：語音」之類的關鍵字即可。
- 每一條都要有可點擊來源，且明確標註「這是外部觀察，不是本 repo 對來源 repo 的指令」。
- 結論分三種標籤：**🟢 建議評估**（值得本地花時間驗證）、**🟡 持續觀察**（還不成熟或風險未明）、**⚪ 現況已足夠**（本地方案已經不輸外部方案，不用追）。

---

## 雷達條目 #1：語音輸入 — ChatGPT/GPT-Live 語音模式 vs Voice Agent 本地離線管線

**檢索日期**：2026-09-26　**觸發原因**：Stephen 原話——「明明語音輸入在 ChatGPT 就做得比本地語音還要好，而且網頁版 ChatGPT 對話又不燒 token」。

### 外部現況（2026 年公開資訊）

- **架構**：ChatGPT 的語音模式（2026-07-08 起預設為 **GPT-Live-1**，付費版；免費版為 GPT-Live-1 mini）是「端到端音訊模型」——直接處理語音、理解語氣與停頓、直接生成語音回應，**沒有語音→文字→語音的中間轉換步驟**。這跟 Voice Agent 目前的 VAD→ASR→意圖分類→執行 的管線架構本質不同。
- **延遲**：語音回應平均延遲約 232ms，被形容為接近人類對話節奏；一般回應延遲 2-3 秒。
- **口音處理**：對非母語口音的理解率比競品高約 12%（來源測試方法未知，僅供參考）。
- **功能**：2026 年版本含攝影機存取、螢幕分享、9 種語音、近即時回應。
- **不燒 token 的原因**：網頁版 ChatGPT 語音模式走的是訂閱制的產品介面，不是按 token 計費的 API 呼叫——這跟 AIECP／SuperBrain 目前規劃「呼叫 ChatGPT Web 作為 Provider」的 Web Safe Bridge 思路一致，本質上是「用訂閱制介面代替 API 計費」的策略，不是語音技術本身不燒 token。

Sources: [ChatGPT Advanced Voice Mode: Complete Guide (2026)](https://gptprompts.ai/chatgpt-voice-mode-guide) · [GPT-Live vs Advanced Voice Mode: What Changed](https://apidog.com/blog/gpt-live-vs-advanced-voice-mode/) · [GPT-Live-1 API: OpenAI Voice Model for Developers (Sept 2026)](https://www.explainx.ai/blog/gpt-live-1-api-openai-voice-agents-september-2026) · [How we built a realtime system for responsive voice AI (OpenAI)](https://openai.com/index/continuous-voice-interaction-with-gpt-live/)

### 可以仿製到本地的開源方案（2026 年公開資訊）

- **Hugging Face Speech-to-Speech Pipeline**（與 Cerebras 合作，2026-07-01 開源）：`Silero VAD v5`（語音偵測）→ `NVIDIA Parakeet TDT`（轉錄，多語言 ASR 排行榜吞吐量最高，平均 WER ~6.34%）→ 任意本地 LLM（`llama.cpp`／`vLLM` 跑）→ `Qwen3-TTS` 或 `Kokoro-82M`（語音輸出）。一行 `pip install` 安裝，**暴露 OpenAI Realtime 相容的 WebSocket API**，全部可以在自己的硬體上跑。
- **open-gpt-live**（GitHub `study8677/open-gpt-live`）：已經有「本地模式」，用 `Ollama` + `faster-whisper` + `Kokoro`，不需要 OpenAI API Key 就能做語音對話，並提供延遲遙測（latency telemetry）與可中斷 TTS。
- **TTS 選型**：`Kokoro-82M`（82M 參數，Apache 2.0，2-3GB VRAM 甚至 CPU 可跑）適合輕量朗讀；`Qwen3-TTS`（0.6B/1.7B，Apache 2.0）支援語音複製與指令式語音設計，已是 HF pipeline 的預設 TTS。

Sources: [The Best Open-Source Text-to-Speech Models in 2026 (BentoML)](https://www.bentoml.com/blog/exploring-the-world-of-open-source-text-to-speech-models) · [open-gpt-live (GitHub)](https://github.com/study8677/open-gpt-live) · [Best Local TTS Models 2026 (LocalAIMaster)](https://localaimaster.com/blog/best-local-tts-models) · [Local Speech-to-Speech: Build a Voice Assistant (2026)](https://localaimaster.com/blog/local-speech-to-speech-assistant)

### 對照 Voice Agent 現況（引用自本 repo 既有盤點，見 [Audit/REPOSITORY_INVENTORY.md](../Audit/REPOSITORY_INVENTORY.md)、[Blueprint/21](21_VOICE_SUPERBRAIN_INTEGRATION_PROPOSAL.md)）

Voice Agent 現有管線是「VAD → 喚醒詞 → ASR → 意圖分類 → 安全分級 L0-L3 → 執行」，中文辨識率 97.9%、意圖辨識率 97.1%，這是**已經跑通、已經驗證過的真實數字**，比上面任何外部方案的公開 benchmark 更可信（外部數字多為官方或第三方部落格宣稱，未經 Voice Agent 那樣的本地實測）。

### 🟢 建議評估（僅供參考，不代表任何來源 repo 已採納）

1. **架構層面**：Voice Agent 目前是「串接式管線」（VAD→ASR→意圖→執行），外部趨勢是往「端到端音訊模型」（GPT-Live 這類）移動，延遲更低、語氣理解更好。本地要走到這一步，`Parakeet TDT`（ASR，6.34% WER、多語言吞吐量最高）可作為評估對象，看是否比目前 Voice Agent 用的 ASR 引擎（`.ai/BLUEPRINT.md` 未在本輪列出具體引擎名稱，需要 Voice Agent 專案自己確認）有優勢——**這不是要重寫 Voice Agent，只是多一個可以拿來 benchmark 的候選**。
2. **TTS 若目前沒有或效果不夠自然**：`Kokoro-82M` 或 `Qwen3-TTS` 都是輕量、Apache 2.0、可離線跑的選項，值得直接下載跑一次 benchmark 比較。
3. **不燒 token 的網頁體驗**：如果本地 AIECP／SuperBrain 未來真的要接 ChatGPT 當 Provider，`open-gpt-live` 這類專案示範了「本地 Ollama + faster-whisper + Kokoro，暴露 OpenAI Realtime 相容 API」的作法，可以作為「怎麼包裝本地方案讓上層（AIECP）用起來像在呼叫雲端 API」的架構參考——這剛好呼應 [Blueprint/20](20_PROPOSED_INTERFACE_CONTRACTS.md) G-03 提案中「SuperBrain 註冊為 AIECP 的 local Provider」的介面設計方向。

### ⚪ 現況已足夠，不用追

- 中文語音辨識準確率本身：Voice Agent 的 97.9% 已經是實測數字，外部方案的多語言 benchmark 未必在中文場景上更好，**不建議只因為 ChatGPT 語音「聽起來」比較好就重寫整套辨識引擎**，應該先做真的 A/B 測試再決定。

---

## 雷達條目 #2：本地大型語言模型選型 — SPARK-AGAVE-3/4（推測為 DGX Spark／GB10 128GB 硬體）

**檢索日期**：2026-09-26　**對照對象**：SuperBrain 藍圖對 SPARK-AGAVE-3/4 的本地 LLM 假設。

### 外部現況
- 128GB unified memory 的記憶體頻寬只有 273 GB/s（約桌上型 GDDR7 顯卡的六分之一），**decode 是頻寬瓶頸**，跑 dense 70B 模型會明顯偏慢。
- 這類硬體最適合 **Mixture-of-Experts（MoE）模型**（每個 token 只啟用一部分參數），例如 Llama 4 Scout/Maverick、Kimi K2；128GB 大約對應「4-bit 量化下 ~200B 參數」的容量上限。
- **Qwen3-Coder-Next** 被評為 2026 年最佳整體本地程式碼模型：SWE-bench Verified 58.7%，256K context，MoE 架構甚至能塞進單張 24GB GPU（完整權重雖 130GB，但 MoE 特性讓推理端負擔小很多）。
- **Laguna S 2.1** 被特別點名適合 DGX Spark／多 GPU 工作站，是「你能在本地跑的最強開源 agentic coder」，SWE-bench Pro 分數在其量級中最高。
- 2026 CES 更新後，TensorRT-LLM 優化＋投機解碼讓 DGX Spark 推理效能提升到 2.5 倍。

Sources: [Best LLM for 128GB RAM (2026)](https://openclawdc.com/blog/best-local-llms-128gb-ram/) · [Best Local LLM for Coding in 2026 (Ranked by VRAM)](https://www.layer3labs.io/guides/best-local-llm-for-coding) · [NVIDIA DGX Spark Review: Local LLM Performance Benchmarks](https://www.startuphub.ai/ai-news/artificial-intelligence/2026/nvidia-dgx-spark-local-llm-performance-benchmarks) · [Measured inference benchmarks on a single DGX Spark (NVIDIA Developer Forums)](https://forums.developer.nvidia.com/t/measured-inference-benchmarks-on-a-single-dgx-spark-same-harness-across-ollama-llama-cpp-and-vllm-notes-data-published/379766)

### 🟢 建議評估
若 SPARK-AGAVE-3/4 真的是 DGX Spark／GB10 等級硬體（見雷達 #7），SuperBrain 選型本地模型時應**優先評估 MoE 架構**（Qwen3-Coder-Next、Laguna S 2.1），而非直接假設「參數量越大越好」——dense 70B 在這個記憶體頻寬下實測會很慢，這點目前 SuperBrain 藍圖（`config/routing.yaml`）尚未看到針對頻寬瓶頸的量化評估。

---

## 雷達條目 #3：MEGIS 對照 — Text-to-CAD／生成式機構設計工具

**檢索日期**：2026-09-26

### 外部現況
- 2026 年 Text-to-CAD 市場兩大陣營：**Zoo.dev**（開發者導向，暴露幾何核心 API，適合自建 CAD 應用）與 **AdamCAD**（YC W25，融資 $4.1M，消費/工程者導向、體驗更完整的參數化 3D）。
- **Autodesk Fusion 360 Generative Design、Altair Inspire、nTopology** 走的是傳統生成式設計路線：工程師定義負載/材料/製造方式，工具生成優化幾何。
- **誠實的限制**：自然語言→CAD 目前仍是實驗性質，能生成簡單參數化幾何（托架、外殼、簡單結構件），但**無法處理生產工程需要的製造公差、組裝關係**這類要求。

Sources: [Text-to-CAD Tools Compared: Zoo vs Adam vs Spectral Labs SGS-1](https://www.getleo.ai/blog/text-to-cad-tools-comparison-guide) · [AdamCAD Review 2026](https://pasqualepillitteri.it/en/news/3372/adamcad-text-to-cad-ai-review-2026) · [AI CAD in 2026: Generative CAD, Review, and ROI](https://www.colabsoftware.com/post/ai-cad-in-2026-why-design-review-is-delivering-roi-while-generative-design-catches-up)

### 🟡 持續觀察
這類工具目前的成熟度（「簡單托架/外殼可以」vs「量產級公差/組裝關係不行」）跟 MEGIS 目前的 Gate 制（G4 模組與限制條件組合）性質不同——MEGIS 做的是更嚴謹的工程驗收流程，Text-to-CAD 工具目前比較適合「快速產生初始幾何草案」這種前段用途，**不建議取代 MEGIS 現有的 Gate 驗收邏輯**，但可以評估是否能加速 G4 早期的幾何草案產出階段。

---

## 雷達條目 #4：AERIS 對照 — AI 聲學模擬工具

**檢索日期**：2026-09-26

### 外部現況
- Python 生態的 **Pyroomacoustics**、**Acoular** 是開源房間聲學模擬/波束成形工具，適合可重現實驗、演算法原型、餵給 ML pipeline。
- **Treble Technologies** 這類公司在做「大規模高保真聲學模擬生成合成聲學資料，用來訓練 ML 模型」——這代表業界正把聲學模擬當成 ML 訓練資料的來源，而不只是設計驗證工具。
- 語音合成/音訊生成的開源模型推薦是 **Fish Speech V1.5、CosyVoice2-0.5B、IndexTTS-2**，但這些是語音/音訊生成，**不是揚聲器設計專用**——目前沒有找到公開的、專門給揚聲器/喇叭機構設計用的 AI 工具。

Sources: [8 best acoustic simulation software for 2026](https://www.guideflow.com/blog/acoustic-simulation-software) · [Best Open Source Models for Sound Design in 2026 (SiliconFlow)](https://www.siliconflow.com/articles/best-open-source-models-for-sound-design) · [Enabling AI-Driven Audio Innovation Through Scalable Acoustic Simulations](https://noisenewsinternational.net/enabling-ai-driven-audio-innovation-through-scalable-acoustic-simulations/)

### ⚪ 現況已足夠，不用追
揚聲器設計本身（AERIS 的核心領域）目前沒有找到比既有工程模擬工具（如 COMSOL 一類，本輪未深查 AERIS 是否已用）更好的 AI 專用替代品。**Pyroomacoustics/Acoular** 若 AERIS 目前沒有用來做房間聲學/陣列模擬，值得評估，但這是「多一個工具選項」而非「取代現有做法」。

---

## 雷達條目 #5：AIECP Evidence 對照 — SLSA 軟體供應鏈證明框架

**檢索日期**：2026-09-26

### 外部現況
- **SLSA**（Supply-chain Levels for Software Artifacts，Google 發起、現由 OpenSSF 維護）是業界標準的「建置證明」分級框架：Level 0（無控制）→ Level 1（證明存在）→ Level 2（受管建置平台＋數位簽章證明）→ Level 3（強制平台隔離＋不可偽造證明，簽章金鑰使用者行程無法存取）。
- 2026 年最新規格 v1.2（2025-11）新增了 Source track（原本只有 Build track）。

Sources: [SLSA Framework Guide 2026](https://www.practical-devsecops.com/slsa-framework-guide-software-supply-chain-security/) · [What is the SLSA Framework? (Wiz)](https://www.wiz.io/academy/application-security/slsa-framework) · [What is the SLSA framework? (Cloudsmith)](https://cloudsmith.com/blog/slsa-a-route-to-tamper-proof-builds-and-secure-software-provenance)

### 🟢 建議評估
AIECP 現有的五級證據（STATIC/TESTED/CI/ENVIRONMENT/OWNER-EXTERNAL）跟 SLSA 的建置證明分級**精神一致但關注點不同**——AIECP 關注「工程任務有沒有真的做完」，SLSA 關注「這個建置產物有沒有被竄改、來源可不可信」。AIECP 現有的 SHA256SUMS + RELEASE_PROVENANCE.json 已經是 SLSA Level 1-2 的雛形，**值得評估是否要正式對齊到 SLSA Level 2/3**，尤其是「數位簽章證明」與「平台隔離」這兩項——這對應到 Codex OFFICIAL/PEGA 的隔離 worktree 機制，可能只差「簽章」這一步就能宣稱對齊 SLSA。

---

## 雷達條目 #6：JN1-UOA 監管層設計參考 — OpenTelemetry GenAI 可觀測性標準

**檢索日期**：2026-09-26　**對照對象**：未來 JN1-UOA（見 [Blueprint/10](10_SUPERVISION_AND_EVIDENCE.md)）的技術實作參考。

### 外部現況
- **OpenTelemetry GenAI 語意慣例**已成為 2026 年業界標準化的可觀測性規範，涵蓋 LLM client span（直接 API 呼叫）、agent span（多步驟工作流）、事件（prompt/completion 內容）、metrics（聚合量測）四大領域。
- 開源 LLM 可觀測性工具比較：**OpenObserve、Langfuse、Confident AI、Arize Phoenix**；商業平台 Datadog 已原生支援 v1.37+ 的 GenAI 語意慣例。

Sources: [OpenTelemetry GenAI Semantic Conventions: A Practical Guide](https://openobserve.ai/blog/opentelemetry-genai-semantic-conventions/) · [Inside the LLM Call: GenAI Observability with OpenTelemetry](https://opentelemetry.io/blog/2026/genai-observability/) · [AI Agent Observability 2026: Tracing & Monitoring Stack](https://www.digitalapplied.com/blog/ai-agent-observability-2026-tracing-monitoring-stack-guide)

### 🟢 建議評估
JN1-UOA 要監管「三機＋Voice+AIECP+AERIS+MEGIS」這麼多異質系統，**與其發明一套全新的監督資料格式，不如直接採用 OpenTelemetry GenAI 語意慣例作為統一的事件/證據交換格式**——這樣 JN1-UOA 收到的每個子系統事件都遵循同一套 schema，而不用為 AIECP、AERIS、MEGIS、Voice Agent 各自的證據詞彙（見 [Blueprint/10](10_SUPERVISION_AND_EVIDENCE.md) 現有的詞彙對照表）各寫一套轉換邏輯。開源選項中 **Langfuse／Arize Phoenix** 都可以自架在本地，符合離線/隱私考量。

---

## 雷達條目 #7：硬體對照 — NVIDIA DGX Spark／GB10（SPARK-AGAVE-3/4 疑似真身）

**檢索日期**：2026-09-26　**對照對象**：R-02（Voice Agent 用的「型號 RTX Spark」機器身分未確認）。

### 外部現況
- **NVIDIA DGX Spark**（2025-03 發表、2025-10 開始出貨，$3,999–$4,699）：GB10 Grace Blackwell Superchip，128GB unified memory，1 petaFLOP FP4 算力。
- 效能特性：Prefill（載入/處理初始 prompt）表現優秀，適合中小型模型（≤20B）；**70B 以上模型的 Decode（逐字生成）效能有硬性瓶頸**，因為記憶體頻寬只有 273GB/s（約桌上型 GDDR7 顯卡的 1/6）。
- 適用場景評估：本地原型開發 70B-200B 模型、CUDA 原生開發、不能離開建築物的敏感資料、單人微調、便攜性——**這些描述跟 SuperBrain 藍圖對 SPARK-AGAVE-3/4 的定位高度吻合**。

Sources: [NVIDIA DGX Spark: Best Local LLM Hardware in 2026](https://www.explainx.ai/blog/nvidia-dgx-spark-local-llm-best-setup-2026) · [NVIDIA DGX Spark In-Depth Review (Hacker News)](https://news.ycombinator.com/item?id=45575127) · [Nvidia says it's more than doubled the DGX Spark's performance since launch](https://www.theregister.com/2026/01/05/nvidia_dgx_spark_speed/)

### 🟢 建議評估
SuperBrain 藍圖裡「Surface RTX Spark，128GB unified」的機器規格描述，跟 NVIDIA DGX Spark／GB10 的公開規格（128GB unified memory）**高度疑似是同一顆晶片的不同品牌包裝**（Microsoft Surface 系列與 NVIDIA 都有推出搭載 GB10 的機型）。若確認屬實，上面雷達 #2 的頻寬瓶頸/MoE 選型建議、以及官方公開的 benchmark 數字，可以直接拿來當 SPARK-AGAVE-3/4 的效能基準，不用自己從頭 benchmark。**這件事值得請 Stephen 本人確認一下機器的實際型號/規格**，因為這會讓本輪雷達 #2 的建議從「業界通例」變成「直接適用的實測數字」。

---

## 雷達條目 #8：Voice Agent 對照 — 開源喚醒詞／關鍵字偵測模型

**檢索日期**：2026-09-26

### 外部現況
- 2026 年主要開源選項：**openWakeWord／livekit-wakeword**（免費開源，DIY 訓練管線，輸出 ONNX）、**microWakeWord**（專為微控制器設計，TFLite Micro）、**onnx-wakeword**（模型 <130KB，推理 <10ms，支援 Android/ESP32/Linux/Web，v9.3 於 2026-06 支援多關鍵字與降低誤觸發）。
- 技術架構共識：**兩階段架構**——一個極輕量、永遠開著的「喚醒偵測」模型（通常是小型 CNN/RNN，極低功耗），偵測到後才啟動完整的 ASR。

Sources: [Wake Word Detection Guide 2026: Complete Technical Overview](https://picovoice.ai/blog/complete-guide-to-wake-word/) · [onnx-wakeword (GitHub)](https://github.com/voicute/onnx-wakeword) · [Wake Word Detection in 2026](https://www.clawnify.com/resources/wake-word-detection)

### 🟡 持續觀察
Voice Agent 現有的喚醒詞機制（本輪盤點未取得具體引擎名稱）如果還沒有做到「<130KB、<10ms」這個等級的輕量化，`onnx-wakeword` 值得評估——尤其如果之後要在功耗受限的裝置上跑（例如整合進 SPARK 系列機器的常駐監聽功能）。但既然 Voice Agent 現有 97.9% 中文辨識率已經是實測驗證過的數字，**不建議在沒有實際 A/B 測試前，只因為新工具規格好看就替換**。

---

## 雷達條目 #9：SuperBrain 對照 — 跨機分散式本地推理框架

**檢索日期**：2026-09-26　**對照對象**：SuperBrain 三機（ULTRA-MAERA-2 + SPARK-AGAVE-3/4，USB-C 2.5GbE 隔離網路）的跨機推理設計。

### 外部現況
- **EXO**：開源本地 AI 叢集框架，自動裝置探索、拓撲感知模型分配、支援 MLX、暴露 OpenAI/Claude 相容 API——會自動探索同網路上其他跑 exo 的裝置，把模型層數切開分散到叢集上。
- **Petals**（BigScience）：公開 P2P 網路的「BitTorrent 式」分散推理，2026 年評測認為「有其侷限」（原文標題即「Why It Struggles」）。
- 業界建議：家用/實驗室規模用 **vLLM + Ray**（2-4 節點，10GbE 起跳）；正式生產規模用 **NVIDIA Dynamo**（InfiniBand）。**2026 年多節點推理已「生產就緒」**，主流框架（vLLM、llama.cpp RPC、NVIDIA Dynamo）都原生支援。

Sources: [EXO Framework Guide: Distributed Local AI in 2026](https://toolhalla.ai/blog/exo-framework-distributed-inference-guide-2026) · [Petals Distributed LLM Inference — Why It Struggles](https://explainx.ai/blog/petals-distributed-llm-inference-revisited-july-2026) · [How to Set Up Multi-Node Local LLM Inference: The Complete 2026 Guide](https://fungies.io/multi-node-local-llm-inference-guide-2026/)

### 🟢 建議評估
SuperBrain 目前的設計是「兩台 Spark 各自獨立跑 FAST/DEEP」，不是跨機切分同一個模型的分散式推理。**如果未來想跑比單台 128GB 記憶體裝得下更大的模型**，`EXO`（自動裝置探索＋OpenAI 相容 API，剛好符合 SuperBrain 已經想暴露相容 API 的方向）或 `vLLM+Ray`（業界目前對 2-4 節點小型叢集的標準建議）值得評估；但 SuperBrain 的隔離網路只有 2.5GbE（低於業界建議的 10GbE 門檻），**這是採用前需要先確認的頻寬限制**。

---

## 雷達條目 #10：G-04 對照 — 獨立驗證／LLM-as-Judge 的真實風險案例

**檢索日期**：2026-09-26　**對照對象**：G-04（SPARK-AGAVE-4 獨立驗證 SPARK-AGAVE-3，Stephen 已認領待設計）。

### 外部現況（重要警示，不是單純的「好消息」）
- 業界最新一個獨立 LLM Judge 驗證管線案例（GPT-5.5 xhigh reasoning，跑在 Codex CLI 上，審核每個任務軌跡＋參考解答＋驗證器輸出，自己下判決）：**這個 Judge 跟原本的 SWE-Bench Pro 執行式驗證器（executable verifier）在 789 個樣本中有 32.4% 不一致**（67 個偽陽性、189 個偽陰性）。
- 更嚴重：2026 年 4 月 Berkeley RDI 研究團隊證實，**SWE-bench、WebArena、OSWorld、GAIA 等主流 benchmark 都存在可被利用的評測漏洞**，可以在沒有真正解決任務的情況下拿到接近滿分。OpenAI 已因確認的評測集洩漏而**停止公布 SWE-bench Verified 分數**。

Sources: [SWE-Bench Pro Verified: A Reliable Benchmark for Software Engineering Agents](https://arxiv.org/pdf/2609.08149) · [Code Review Agent Benchmark](https://arxiv.org/pdf/2603.23448) · [Dissecting the SWE-Bench Leaderboards](https://arxiv.org/pdf/2506.17208)

### 🟡 持續觀察（這是本輪雷達最重要的一條警示）
這條給 Stephen 設計 G-04 協議時的具體提醒：**「用另一個 LLM 當 Judge 去驗證第一個 LLM 的輸出」這個做法，業界最新數據顯示本身就有 32.4% 的分歧率，而且主流評測基準本身也被證實有造假漏洞**。這意味著 SPARK-AGAVE-4 若只是「用另一個模型重新看一遍」，**不足以構成可信的獨立驗證**——真正可信的驗證應該優先依賴**確定性測試（executable verifier，例如跑真的單元測試/整合測試）**，LLM Judge 只能當輔助審核，不能是唯一權威。這跟 AIECP 既有原則「A model response is never sufficient proof」（[Blueprint/00](00_MASTER_BLUEPRINT.md) 問題24）完全一致，本次是找到了 2026 年最新的量化證據支持這個原則。

---

## 雷達條目 #11：AIECP／Codex Worktree 對照 — 2026 平行 AI Coding Agent 沙盒生態

**檢索日期**：2026-09-26

### 外部現況
- **Git worktree 隔離**已成為 2026 年「跑 4 個以上並行 AI session」團隊的預設協作層——Claude Code、OpenAI Codex、Cursor 都已原生支援，讓每個 agent 有自己獨立的工作目錄與 git index，共用同一個 object store，避免檔案衝突/context 污染/鎖競爭。
- Claude Code 現在原生支援在 subagent frontmatter 裡加 `isolation: worktree`，worktree 預設放在 `.claude/worktrees/`。
- 更進一步的沙盒方案：`packnplay`（Docker 沙盒＋worktree）、`agentbox`（容器化＋權限降級＋防火牆）；Claude Code 本身的 bash 工具用 `@anthropic-ai/sandbox-runtime`（Seatbelt/bubblewrap＋網路代理），雲端版更是跑在完整 microVM 裡。
- 搜尋結果**沒有找到 Windows ARM64 原生支援的具體資料**——這正好對應 AIECP 目前「10 個 ENVIRONMENT 關卡未結案」裡「真實 Windows/ARM64 環境上跑通 Codex OFFICIAL+PEGA 並行執行」這個卡住的驗收項。

Sources: [How to Use Git Worktrees for Parallel AI Agent Execution](https://www.augmentcode.com/guides/git-worktrees-parallel-ai-agent-execution) · [Sandboxes and Worktrees: My secure Agentic AI Setup 2026](https://mikemcquaid.com/sandboxed-agent-worktrees-my-coding-and-ai-setup-in-2026/) · [List of coding agent sandboxes 2026-05](https://gist.github.com/wincent/2752d8d97727577050c043e4ff9e386e)

### 🟢 建議評估
AIECP 現有的 Codex OFFICIAL/PEGA 隔離 worktree 機制，架構方向**跟 2026 年業界主流做法一致**（不是走偏了），但業界的沙盒隔離已經進化到「容器化＋權限降級＋防火牆」這個層級（`packnplay`/`agentbox`），而 AIECP 目前卡住的正是「Windows/ARM64 環境驗證」這個具體技術缺口——**這是外部研究也沒能直接回答的問題**，值得誠實記錄：這個特定卡點（Windows ARM64 沙盒隔離）目前看起來是相對前沿、公開資料稀少的領域，可能需要 Stephen 自己在真實硬體上試，而不是等業界標準方案出現。

---

## 下一批建議雷達方向（尚未執行檢索，供 Stephen 排序）

- SPARK-AGAVE-3/4 真實機型確認後（見雷達 #7），重跑一次精確對照。
- AERIS 若已在用特定聲學模擬軟體，針對那個軟體找 2026 年的 AI 外掛/競品對照（本輪只查了通用開源工具，未鎖定特定商業軟體）。
- Voice Agent 現有 ASR 引擎的具體名稱確認後，跟雷達 #2 的 Parakeet TDT／Qwen3-Coder-Next 這類最新模型做直接 benchmark 對照。
- JN1-UOA 若真的要採用 OpenTelemetry GenAI（雷達 #6），下一步應該具體評估 Langfuse vs Arize Phoenix 兩個自架方案的落地成本。

要跑哪一個，或想跑全新主題，直接跟 Claude 說「跑雷達：XX」即可。
