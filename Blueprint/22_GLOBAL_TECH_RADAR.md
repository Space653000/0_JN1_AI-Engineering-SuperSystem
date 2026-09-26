# 22 — Global Tech Radar

> 這份文件是本 repo **建立初衷**的直接實踐（見 [README.md](../README.md)「建立初衷」段落，2026-09-26 Stephen 澄清）：Stephen 每週只有約 $20 額度可用在本地施工，追不上全球 AI 進展速度。本文件的工作是持續往雲端／全世界檢索最新技能、方法、工具、架構，跟本地七個專案現況對標，找出可以借用/仿製的落差，寫成建議——**完全在 cloud session 進行，不涉及修改任何本地或來源 repo**。
>
> **2026-09-26 第二輪擴充**：#1-16 是第一輪（14條後補到16條）；Stephen 要求「先列100+候選再自己篩選收斂」，完整候選長清單與篩除理由見 [25_TECH_RADAR_CANDIDATE_LONGLIST.md](25_TECH_RADAR_CANDIDATE_LONGLIST.md)，通過篩選並實際檢索的25條新條目是 #17-#41（見下方「2026-09-26 第二輪擴充」小節）。

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

## 雷達條目 #7：硬體對照 — 確認為 Microsoft Surface RTX Spark Dev Box（2026-09-26 由 Stephen 提供線索、本輪查證確認）

**檢索日期**：2026-09-26（首次：假設為 NVIDIA DGX Spark）／**2026-09-26 更新**（Stephen 提供 Microsoft Build 2026 中文報導，本輪查證確認為另一款更精準吻合的機型）。
**對照對象**：R-02（Voice Agent 用的「型號 RTX Spark」機器身分未確認）＋ SPARK-AGAVE-3/4 硬體規格確認。

### 外部現況（已查證確認，非推測）
- **產品名稱**：**Microsoft Surface RTX Spark Dev Box**，於 **2026-06-02 Microsoft Build 2026** 大會發表，執行長 Satya Nadella 稱為開發者的「夢中神機」。
- **正式上市日**：**2026-10-07**，美國率先開賣，僅在 Microsoft.com 獨家銷售——**本文撰寫時（2026-09-26）尚未正式上市，距上市僅約 11 天**。
- **晶片**：NVIDIA **RTX Spark** 系統級晶片（SoC）＝ 20 核 Grace（Arm）CPU ＋ Blackwell RTX GPU（6,144 CUDA 核心，與 RTX 5070 同核心數），透過 NVLink-C2C 連接，1 petaFLOP AI 算力。
- **記憶體**：128GB 統一記憶體，可在本地流暢執行 **120B+ 參數模型，支援最高 100 萬 token 超長上下文**。
- **散熱/功耗**：陽極氧化鋁一體成型機殼兼被動散熱器，1,000 個通風孔，100W 熱設計功耗（TDP），近乎零噪音。
- **軟體**：預裝客製化 Windows 11 Pro（深色主題、精簡工作列、開發者模式已開啟）、WSL2 已配置 GPU 直通＋CUDA、VS Code＋GitHub Copilot＋Git＋Python／Node.js 全部預裝、VS Code AI Toolkit 可做模型轉換/微調/評估、Windows ML＋Windows Copilot Runtime（內建TensorRT）做本地推論、Microsoft Foundry 做本地↔雲端無縫部署。
- **安全**：Secured-core PC 架構，BitLocker＋Microsoft Defender 標配，支援 Entra ID／Intune 企業裝置管理。
- **2026-09-26 三輪查證修正記錄**（誠實揭露本輪雷達自己的犯錯過程，而不是默默改掉）：
  1. 第一版：假設 SPARK-AGAVE-3/4 疑似是 NVIDIA 自家 DGX Spark。
  2. Stephen 提供 Build 2026 中文報導後，查證更正為 Microsoft Surface RTX Spark Dev Box，並誤判「Surface Laptop Ultra 用的 RTX Spark N1X 是規格較低的變體」。
  3. Stephen 指出「沒有搞混」，再次查證後更正：**N1X 是 NVIDIA 對 RTX Spark 晶片本身的代號**，不是獨立低規格款，Laptop Ultra 與 Dev Box 可能共用同一晶片家族——但**本 repo 一度因此進一步推論「兩者記憶體容量/算力等級接近」，這一步是本 repo 自己的過度引申，不是 Stephen 提供的資訊**。
  4. **Stephen 第三次糾正**：他實際持有的 ULTRA-MAERA-2＝Surface Laptop Ultra 是 **64GB**，不是 128GB；SPARK-AGAVE-3/4（Dev Box，未上市）才是 **128GB × 2台**。**以 Stephen 對自己實際硬體的第一手陳述為準**，撤回上一輪「兩者容量/算力接近」的推論。晶片家族命名相同，不代表記憶體規格或算力等級相同——這是本輪雷達最需要記取的教訓：**查到公開資料後，不能自己再往下引申超出資料本身講的事情**。
  5. **Stephen 進一步澄清（避免本 repo 誤解成不同機型）**：ULTRA-MAERA-2 的 Surface Laptop Ultra**是同一款產品線（RTX Spark版）**，不是舊款/不同型號——差別純粹是 Stephen 這台選的是**次規 64GB 配置**，不是頂規 128GB 配置。也就是說 Surface Laptop Ultra（RTX Spark）本身就有 64GB／128GB 兩種可選記憶體配置，ULTRA-MAERA-2 用的是低配那一版。上一版（本節第4點）「兩者是否同產品線」曾一度講得含糊，這裡明確澄清：**同產品線、不同記憶體配置等級**，不是不同機型。

修正後的事實認定：ULTRA-MAERA-2＝Surface Laptop Ultra，64GB unified（見 [Blueprint/03](03_MACHINE_ARCHITECTURE.md) 原始記載，正確）；SPARK-AGAVE-3/4＝Surface RTX Spark Dev Box（未上市），128GB unified，各自獨立兩台，規格相同。

Sources: [Microsoft Devices Blog: Building the next generation of devices for developers](https://blogs.windows.com/devices/2026/06/02/building-the-next-generation-of-devices-for-developers-surface-rtx-spark-dev-box/) · [VentureBeat: Microsoft debuts Surface RTX Spark Dev Box](https://venturebeat.com/ai/microsoft-debuts-surface-rtx-spark-dev-box-to-run-large-ai-models-without-cloud-costs) · [Microsoft Surface 官方產品頁 - Dev Box](https://www.microsoft.com/en-us/surface/devices/surface-rtx-spark-dev-box) · [Microsoft Surface 官方產品頁 - Laptop Ultra](https://www.microsoft.com/en-us/surface/devices/surface-laptop-ultra) · [Thurrott: Build 2026 - Surface RTX Spark Dev Box Coming Later this Year](https://www.thurrott.com/a-i/336931/build-2026-nvidia-powered-surface-rtx-spark-dev-box-is-coming-later-this-year) · [Tom's Hardware：Surface Laptop Ultra with Nvidia N1X chip prototype review](https://www.tomshardware.com/laptops/mystery-reviewer-finds-nvidia-rtx-spark-prototype-laptop-and-puts-it-through-its-paces-microsoft-surface-laptop-ultra-with-nvidia-n1x-chip-shows-promise-though-prototype-warts-are-still-quite-visible) · [wccftech：Passive-Cooled Surface RTX Spark Dev Box, 128GB Memory](https://wccftech.com/microsofts-nvidia-power-to-devs-passive-cooled-surface-rtx-spark-dev-box-late-2026-launch/) · [BigGo財經（Stephen 提供）：Surface RTX Spark Dev Box](https://finance.biggo.com.tw/news/sCS6i54BoQmpnl369WT6)（此連結本 session 網路存取被擋，內容依 Stephen 轉述與其他來源交叉確認）

### 🟢 建議評估（重要性從 #7 升級為本輪最高優先）
1. **這解釋了 SuperBrain P0 為什麼卡住**：P0（硬體盤點）需要的機器 2026-09-26 當下**根本還沒正式上市**（10/7 才上市），SuperBrain 藍圖標記的「P0 硬體盤點：未完成」極可能不是施工延遲，而是**硬體還買不到**。這是一個此前盤點都沒抓到的關鍵落差，已同步更新進 [Blueprint/19 總表](19_MASTER_PROGRESS_TRACKER.md) 與 [Audit/CONFLICT_ANALYSIS.md](../Audit/CONFLICT_ANALYSIS.md)。
2. 雷達 #2（本地LLM選型）與 #9（跨機推理）的建議現在可以**直接套用官方公開規格**（128GB、1 petaFLOP、100萬token context），不用再等 Stephen 自己 benchmark。
3. 預裝的 **WSL2 GPU直通/CUDA、VS Code AI Toolkit、Windows Copilot Runtime（TensorRT）、Microsoft Foundry** 這一整套官方 AI 開發工具鏈，剛好可以評估是否能直接拿來實作雷達 #9 提到的「本地推理暴露 OpenAI 相容 API」需求，甚至可能比自己組 EXO/vLLM+Ray 更省事——上市後值得優先評估用官方工具鏈 vs 開源方案。
4. **Secured-core PC + BitLocker + Entra ID/Intune** 這組安全基礎設施，剛好可以對照雷達 #13（AIECP 憑證管理），評估是否能用 Windows 原生的裝置層安全機制，取代/補強 AIECP 自己另外設計的一套。

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

## 雷達條目 #12：MEGIS 對照 — AI 公差堆疊分析／DFM 自動審查

**檢索日期**：2026-09-26

### 外部現況
- 2026 年主流做法是**分兩層**：建模階段用「In-CAD DFM」即時抓問題（拔模不足、薄壁、過緊公差），設計審查階段用更完整的工具評估整個零件（含製造流程、材料、組裝關係）。
- 「大型機構模型」（Large Mechanical Models，訓練自可信機構設計資料/工程標準/供應商資訊）能辨識實務製造風險（複雜工模需求、倒勾、困難公差堆疊）並給出可執行建議。
- **業界共識**：2026 年真正有效果的做法不是追求「全自動 DFM 審查」，而是用 AI **縮短資深/資淺工程師的知識落差**——把幾十年的製造經驗教訓即時提示給新手。

Sources: [AI for Tolerance Stack-Up Analysis](https://www.getleo.ai/blog/ai-tolerance-stack-up-analysis) · [DFM Analysis in 2026: Best AI Tools](https://www.getleo.ai/blog/dfm-analysis-ai-tools-manufacturing-feedback) · [DFM Is Broken: How AI Is Finally Making It Work](https://www.getleo.ai/blog/dfm-broken-ai-fixing-design-manufacturability-2026)

### 🟢 建議評估
MEGIS 的 Gate 制（尤其 G4 模組與限制條件組合）本質上就是在做「公差/組裝關係的正式審查」，跟業界「兩層 DFM」的**審查階段**精神一致。可評估的落差是**建模階段的即時回饋**——如果 MEGIS 工程師是先建模、後面才在 Gate 審查抓到公差問題，導入 in-CAD 即時 DFM 檢查可以把問題往前移，減少 Gate 打回重做的成本。

---

## 雷達條目 #13：AIECP 對照 — AI Agent 憑證/密鑰管理與沙盒安全

**檢索日期**：2026-09-26

### 外部現況
- 2026 年最佳實踐：憑證應該**短效、限定任務範圍、綁定授權者、可單獨撤銷**（不影響其他 agent/使用者）；避免把密鑰寫死在程式或環境變數，改用 Vault（HashiCorp Vault/AWS Secrets Manager/Azure Key Vault）簽發臨時、限定範圍的憑證。
- **縱深防禦**：沙盒隔離＋監控＋核准關卡＋簽章產出物，多層疊加；高風險動作（金融交易、刪除資料）強制人工核准；所有程式執行/工具呼叫/API 請求都要留不可竄改的稽核紀錄。
- Anthropic 自己的 **Managed Agents 平台**（2026-04-08 上線）是一個參考架構：推理引擎本身**永遠不直接持有原始憑證**，憑證由獨立的 broker 短效簽發。

Sources: [AI Agent Credential and Secret Management in Production (Zylos Research)](https://zylos.ai/research/2026-05-07-ai-agent-credential-secret-management-production/) · [How to manage API keys, tokens, and secrets for AI agents (WorkOS)](https://workos.com/blog/ai-agent-secrets-management) · [Practical Security Guidance for Sandboxing Agentic Workflows (NVIDIA)](https://developer.nvidia.com/blog/practical-security-guidance-for-sandboxing-agentic-workflows-and-managing-execution-risk/)

### 🟢 建議評估
AIECP 現有的「RED 等級操作需 exact-action digest 核准」「agent 無法自我核准」已經符合「高風險動作強制人工核准」這條業界原則。值得補強評估的是**「推理引擎永遠不直接持有原始憑證」**這個模式——AIECP 的 Codex OFFICIAL/PEGA 若目前是直接拿到 GitHub token/API key 本身（而非透過短效 broker 簽發），這是一個可以對齊 2026 業界最佳實踐的具體改善點，也呼應雷達 #5 的 SLSA 對齊建議。

---

## 雷達條目 #14：跨專案知識庫對照 — 本地 RAG／多專案檢索工具

**檢索日期**：2026-09-26　**對照對象**：JN1-UOD／JN1-UOA 未來可能需要跨七個專案文件做檢索的情境。

### 外部現況
- 2026 年 RAG 工具分兩類：**企業知識管理平台**（Guru、Notion AI、Confluence+Atlassian Intelligence、Glean）解決「檢索介面」問題；**RAG 基礎設施工具**（LangChain、LlamaIndex、Haystack）解決「檢索管線工程」問題。
- 多專案檢索的企業做法：混合搜尋（向量＋BM25）＋ reranking ＋ LLM-based 知識圖譜做跨文件關聯。
- **LlamaIndex**、**Haystack** 都是開源、可本地部署，適合「版本感知搜尋＋組織範圍限定＋技術文件語意理解」這類需求——剛好符合七個工程 repo 各自版本演進快、術語不統一的情況。

Sources: [Best LLM Knowledge Base Tools in 2026 (Atlan)](https://atlan.com/know/llm-knowledge-base-tools/) · [15 Best Open-Source RAG Frameworks in 2026](https://www.firecrawl.dev/blog/best-open-source-rag-frameworks) · [Best Enterprise RAG Platforms for 2026](https://onyx.app/insights/enterprise-rag-platforms-2026)

### 🟡 持續觀察
本 SuperSystem repo 目前是靠人工/Claude 逐一 clone 七個 repo 來盤點（見 [Audit/](../Audit/)），如果未來盤點頻率提高（例如你要求的「每次完成一段工作就回來更新」變得很頻繁），**用 LlamaIndex/Haystack 建一個跨七個 repo 的本地向量索引**，可以讓「重新盤點」從「重新 clone+人工讀」變成「查詢式檢索」，加快速度。但目前盤點頻率還不高，這是**觀察項目**，不是急迫需求。

---

## 雷達條目 #15：G-04 深挖 — 具體開源獨立驗證實作範例

**檢索日期**：2026-09-26　**對照對象**：雷達 #10 的延伸——上次只找到「LLM Judge 不可靠」的警示，這次找具體可參考的實作。

### 外部現況
- **N0 Verify**（GitHub `HmZ9874/n0-verify-ai-code-verification`）：開源 CLI + GitHub Action，專門做「獨立驗證 AI 寫的程式碼與其證據」，內建專案偵測與確定性指令，涵蓋 JS/TS、Python、Go、Rust。
- **Behavioral Equivalence Harness**：先記錄原始程式碼的**實際行為**，再拿 AI 改寫後的版本重放比對，證明行為沒變——**驗證器本身是確定性的，裡面沒有模型**。
- **Contract-Driven Adversarial Verification**：一個 agent 依編譯過的合約做實作，另一個 agent 依同一份合約寫測試、**看不到實作內容**；獨立性是結構性強制的——分開的 agent、分開的 job payload、分開的執行佇列、不共用對話歷史。
- **Verification-First Multi-Agent Harness**（`wenxiangyuan611-hash/verification-first-harness`）：有 VerifierRegistry 跟 bounded CommandVerifierPlugin，對「canonical claim envelope」做確定性外部檢查。

Sources: [n0-verify-ai-code-verification (GitHub)](https://github.com/HmZ9874/n0-verify-ai-code-verification) · [verification-first-harness (GitHub)](https://github.com/wenxiangyuan611-hash/verification-first-harness) · [Meta-Engineering Harnesses for AI-Native Software Production (arXiv 2605.25665)](https://arxiv.org/pdf/2605.25665)

### 🟢 建議評估（給 Stephen 設計 G-04 時的具體起手式）
這次找到的都印證雷達 #10 的警示——2026 年真正被認可的做法是**「驗證器本身不含模型、只做確定性比對」**（Behavioral Equivalence Harness）或**「結構性隔離兩個 agent，一個依合約寫、另一個依合約測，互相看不到對方」**（Contract-Driven Adversarial Verification）。這兩個模式可以直接對應到 SPARK-AGAVE-4 驗證 SPARK-AGAVE-3 的設計：
1. SPARK-AGAVE-3（FAST）的輸出應該先被轉成「行為紀錄」（做了什麼、產出什麼），而不是直接把它的推理過程交給 SPARK-AGAVE-4。
2. SPARK-AGAVE-4（DEEP）的驗證邏輯應該優先用**確定性重放/比對**，只有比對不出來的部分才用 LLM 輔助判斷。
3. 兩台機器的網路隔離（現有 SuperBrain 設計就有：Spark 之間互不連線，只能連 ULTRA-MAERA-2）剛好天生符合「結構性隔離」的要求，這是 SuperBrain 現有架構的優勢，值得在 G-04 設計時明確保留。

---

## 雷達條目 #16：SLSA 落地細節 — 給 AIECP 的具體實作路徑（延伸雷達 #5）

**檢索日期**：2026-09-26

### 外部現況
- **SLSA Level 2 vs 3 的關鍵差異**：Level 2 只要求「建置產生證明」；Level 3 要求「證明由建置本身無法影響的可信元件產生」——具體對應：一個你不直接操作的 CI 平台、一個從建置內部無法竄改的簽章身分、一個每次執行都重置的建置環境。
- **如果用 GitHub Actions，直接跳去做 Level 3**：主要工具是 `slsa-framework/slsa-github-generator`，跑在獨立的 reusable workflow，其身分無法被受駭的建置步驟冒用。
- **簽章機制**：整合 Sigstore 三件套——**Cosign**（簽章請求/簽署/儲存）、**Fulcio CA**（用 GitHub Actions OIDC 身分核發短效憑證）、**Rekor**（不可竄改的公開透明日誌記錄簽章事件）。
- **更輕量的新選項**：**GitHub Artifact Attestations**——只要幾行 YAML，不用自己管理密鑰或額外基礎設施，就能幫建置產物加上可驗證的建置歷程證明。

Sources: [slsa-github-generator (GitHub)](https://github.com/slsa-framework/slsa-github-generator) · [Achieving SLSA 3 Compliance with GitHub Actions and Sigstore (GitHub Blog)](https://github.blog/security/supply-chain-security/slsa-3-compliance-with-github-actions/) · [Enhance build security and reach SLSA Level 3 with GitHub Artifact Attestations (GitHub Blog)](https://github.blog/enterprise-software/devsecops/enhance-build-security-and-reach-slsa-level-3-with-github-artifact-attestations/) · [How to Implement SLSA Level 3 Practically (2026)](https://safeguard.sh/resources/blog/how-to-implement-slsa-level-3-practical-guide)

### 🟢 建議評估
AIECP 若要落地雷達 #5 的建議（對齊 SLSA Level 2/3），最低成本的路徑是**先上 GitHub Artifact Attestations**（幾行 YAML，不用管密鑰）替現有的 SHA256SUMS + RELEASE_PROVENANCE.json 加上官方可驗證的建置證明；如果要衝 Level 3，`slsa-github-generator` 這個現成工具鏈可以直接用，不用自己重新發明簽章機制——這剛好也回答了雷達 #13（憑證管理）的「短效、broker 簽發」原則，因為 Fulcio 核發的就是短效憑證。

---

## 2026-09-26 第二輪擴充（#17-#41）

> 這批條目回應 Stephen 的要求——「先列100+候選再自己篩選收斂」。完整的候選長清單、篩除理由（含「篩除未檢索」與「檢索後篩除」）見 [25_TECH_RADAR_CANDIDATE_LONGLIST.md](25_TECH_RADAR_CANDIDATE_LONGLIST.md)。這裡只放通過篩選、真正檢索過的 25 條正式雷達條目。

---

## 雷達條目 #17：語音對照延伸 — 中文ASR最新選項 SenseVoice/FunASR + 全雙工架構 Kyutai Moshi

**檢索日期**：2026-09-26　**對照對象**：延伸雷達#1，補強「Voice Agent中文ASR具體引擎未知」這個缺口的外部對標基準。

### 外部現況
- **SenseVoice/FunASR vs Whisper**：2026-07-15 一次基準測試（184個中文長音訊檔、共192.3分鐘、單張H100）顯示 SenseVoice 中文字元錯誤率(CER) 7.81%、Paraformer 10.18%，對比 Whisper-large-v3 的 20.02%（turbo版21.71%）——FunASR系模型錯誤率約為Whisper的一半或更低。SenseVoice-Small 推論速度達169.6倍即時，比Whisper-large-v3快約12倍；純CPU上SenseVoice仍有17.2倍即時，比Whisper用GPU還快。原因是SenseVoice/Paraformer是非自回歸架構（一次前向推論出全文），加上訓練資料本身針對中文/亞洲語言調校。
- **Kyutai Moshi**：CC-BY 4.0授權的語音-文字基礎模型，全雙工對話框架（同時建模「Moshi說話」與「使用者說話」兩條音訊流），理論延遲160ms、實務延遲低至200ms（L4 GPU）。提供PyTorch(bf16/int8)、MLX(int4/int8/bf16)、Rust/Candle(int8/bf16)多種本地部署格式，2026-04發布MoshiRAG讓Moshi能結合文字LLM回答複雜問題。

Sources: [FunASR vs Whisper: Real Chinese ASR Benchmark](https://www.funasr.com/en/blog/funasr-vs-whisper-benchmark.html) · [Which FunASR Model? Nano vs MLT-Nano vs SenseVoice vs Paraformer (2026 Guide)](https://www.funasr.com/en/blog/which-funasr-model.html) · [FunAudioLLM/SenseVoiceSmall (Hugging Face)](https://huggingface.co/FunAudioLLM/SenseVoiceSmall) · [GitHub - kyutai-labs/moshi](https://github.com/kyutai-labs/moshi) · [Why Moshi STT Could Replace Whisper](https://scalastic.io/en/moshi-stt-vs-whisper/)

### 🟢 建議評估
Voice Agent 現有97.9%中文辨識率是實測數字，但目前盤點未取得其底層ASR引擎的具體名稱（見[Registry/PROJECTS.yaml](../Registry/PROJECTS.yaml) `voice-agent`條目）。**SenseVoice/FunASR的公開中文benchmark（CER 7.81% vs Whisper 20%+）是目前找到最具體、最貼近中文場景的對標對象**，建議Voice Agent專案自己拿SenseVoice跑一次A/B測試，而不是只跟通用多語言ASR（如雷達#2提到的Parakeet TDT）比較。Kyutai Moshi的全雙工架構（160-200ms延遲）則呼應雷達#1「串接式管線→端到端音訊模型」的架構演進方向，且是CC-BY 4.0可本地部署，值得列入下一輪「如果要重新設計語音管線」的候選清單，但**不建議在沒有A/B測試前替換現有管線**（同雷達#1既有原則）。

---

## 雷達條目 #18：語音對照延伸 — 開源TTS最新選型 CosyVoice3 / F5-TTS / GPT-SoVITS

**檢索日期**：2026-09-26　**對照對象**：延伸雷達#1的TTS選型建議（原本是Kokoro-82M/Qwen3-TTS）。

### 外部現況
- **CosyVoice3**（阿里FunAudioLLM團隊，0.5B參數，2025-12更新）：支援18種中文方言＋9種語言，是評測集中唯一能覆蓋Multilingual Voice Cloning benchmark全部語言的系統，支援串流、zero-shot語音克隆、情感控制。
- **F5-TTS**：非自回歸、基於Diffusion Transformer的flow-matching系統，zero-shot克隆不需複雜音素對齊，受控benchmark下WER表現更強，但**長文本處理較弱**（CV3-Hard-EN樣本大量失敗）。
- **GPT-SoVITS**：少樣本(few-shot)語音克隆能力強，是中文/日文/韓文開源選項中最強之一，但**英文合成品質有明確文獻記載的限制**（跨語言合成到英文會有瑕疵與韻律問題）。
- 2026年實務建議：語音克隆需求選F5-TTS或GPT-SoVITS，多語言需求選CosyVoice3。

Sources: [CosyVoice 3: Towards In-the-wild Speech Generation via Scaling-up and Post-training](https://arxiv.org/html/2505.17589v2) · [9 Best Open Source Text to Speech Models in 2026 (Bland AI)](https://www.bland.ai/blog/best-open-source-text-to-speech-model) · [Best TTS Models 2026 (CodeSOTA)](https://www.codesota.com/guides/tts-models)

### 🟢 建議評估
若Voice Agent目前TTS是純中文場景，**GPT-SoVITS的少樣本語音克隆或CosyVoice3的18種中文方言支援**都比雷達#1原先建議的Kokoro-82M（英文為主）更貼近中文場景需求，值得下載跑一次實測比較，尤其如果未來有「客製化語音人格」的需求（語音克隆）。**與雷達#1一樣，不建議在沒有實測前直接替換。**

---

## 雷達條目 #19：本地LLM選型現實檢查 — 修正雷達#2的容量假設

**檢索日期**：2026-09-26　**對照對象**：延伸並部分修正雷達#2（SPARK-AGAVE本地LLM選型）。

### 外部現況（重要：對容量的具體修正）
- **DeepSeek-V3.2**（671B參數）在多數量化等級下都會超過128GB容量，**不適合SPARK-AGAVE-3/4這類128GB機器**。
- **GLM-5.2**（744B總參數、40B啟用）即使1-bit量化也需要223GB，2-bit版本才勉強塞進256GB Mac，**同樣不適合128GB**。
- **Mistral Medium 3.5**（128B dense多模態模型）在4-bit量化下需80GB、3-bit需64GB，**是128GB系統可行的dense模型選項**。
- **Qwen3系列（Apache 2.0授權）**被評為「本地自架授權最寬鬆、部署門檻最輕」的選項，2026年建議路徑是「先從Qwen3或Devstral開始，等基礎設施夠了再考慮DeepSeek-V4等級」。

Sources: [Best Open Source and Open-Weight LLM Models to Run Locally in 2026 (Hugging Face)](https://huggingface.co/blog/daya-shankar/open-source-llm-models-to-run-locally) · [Local LLM 2026: Every Major Model Release + Ollama Status](https://www.promptquorum.com/local-llms/local-llm-model-updates-2026) · [GLM-5.2 vs DeepSeek V4 vs Qwen3: The Open-Weights Coding Model Showdown (2026)](https://www.developersdigest.tech/blog/glm-5-2-vs-deepseek-v4-vs-qwen3-open-weights-coding-showdown)

### 🟢 建議評估（修正雷達#2的具體風險）
雷達#2原本建議「優先評估MoE架構」是對的方向，但**這輪檢索找到具體反例**：如果SuperBrain未來考慮跑GLM-4.6/DeepSeek V3.2這類熱門中文模型，**必須先確認量化後是否真的塞得進128GB**——本輪查到的具體數字顯示這兩個常被提及的模型系列在多數量化等級下都超出128GB。建議SuperBrain選型時把「4-bit量化後實際記憶體佔用」列為硬性篩選條件，而不是只看「MoE vs dense」這個維度；Qwen3系列（雷達#2已提及的Qwen3-Coder-Next同系）目前看起來是128GB硬體上限最寬鬆的路線。

---

## 雷達條目 #20：本地推理加速 — EAGLE-3 推測解碼(Speculative Decoding)

**檢索日期**：2026-09-26　**對照對象**：延伸雷達#2/#9，本地推理速度優化技術。

### 外部現況
- EAGLE-3已成為2026年推測解碼的業界標準，2026年初已合併進vLLM、SGLang、TensorRT-LLM主線。
- 原理：一個輕量draft模型在混合特徵層級提出多個候選token，目標模型一次前向推論驗證所有候選，平均每次目標模型推論可換取約3個被接受的token——因為token生成在低batch size下是**記憶體頻寬瓶頸**（GPU每生成一個token都要重新從VRAM讀取權重），這正好對應雷達#2提到SPARK硬體273GB/s頻寬瓶頸的問題。
- 實測效果：LLaMA-3.3-70B上達到最高4.79倍加速，且不損失品質。

Sources: [Eagle-3 Speculative Decoding on GPU Cloud: 3-4x Faster LLM Inference (2026)](https://www.spheron.network/blog/eagle-3-speculative-decoding-gpu-cloud/) · [Fly Eagle(3) fly: Faster inference with vLLM & speculative decoding (Red Hat Developer)](https://developers.redhat.com/articles/2025/07/01/fly-eagle3-fly-faster-inference-vllm-speculative-decoding)

### 🟢 建議評估
這是直接針對雷達#2發現的「SPARK硬體273GB/s頻寬瓶頸」的**具體緩解技術**，不是選另一個模型，而是同一個模型跑得更快。如果SuperBrain未來用vLLM或SGLang部署本地模型（雷達#9已建議評估vLLM+Ray），**EAGLE-3推測解碼應該一併納入評估**，因為它已是這些推理引擎的內建選項，理論上不需要額外開發成本就能拿到最高4.79倍的加速——特別值得注意的是這個技術專門解決「頻寬瓶頸下的低batch size推理」，剛好是SPARK硬體的痛點場景。

---

## 雷達條目 #21：AERIS 對照延伸 — 揚聲器非線性失真AI預測模型

**檢索日期**：2026-09-26　**對照對象**：延伸雷達#4，聚焦揚聲器設計特有的技術缺口。

### 外部現況
- 學術界已有多篇2025-2026論文用深度學習預測揚聲器非線性失真（THD/IMD），例如針對參數陣列揚聲器(parametric array loudspeakers)的深度學習非線性失真辨識與補償研究，估測THD/IMD與實測值平均誤差僅1.08%與0.34%。
- 傳統Thiele-Small模型是線性低頻近似；學術界正在發展「有限維、功率平衡、可保證被動性」的非線性port-Hamiltonian系統來擴充Thiele-Small模型處理非線性現象（音圈位置/電壓的非線性電感、順性、動態力因子）。
- COMSOL官方部落格本身也有「如何對揚聲器驅動器做非線性失真分析」的教學，顯示商業CAE工具本身已內建這類分析能力（非AI專屬，是既有數值方法）。

Sources: [Deep Learning-Based Approach for Identification and Compensation of Nonlinear Distortions in Parametric Array Loudspeakers](https://arxiv.org/pdf/2412.01092) · [Passive modelling of the electrodynamic loudspeaker: from the Thiele–Small model to nonlinear port-Hamiltonian systems](https://acta-acustica.edpsciences.org/component/article?access=doi&doi=10.1051%2Faacus%2F2019001) · [How to Perform a Nonlinear Distortion Analysis of a Loudspeaker Driver (COMSOL Blog)](https://www.comsol.com/blogs/how-to-perform-a-nonlinear-distortion-analysis-of-a-loudspeaker-driver)

### 🟡 持續觀察
這批文獻目前仍是**學術論文/碩士論文層級**（例如Lund University 2025年的碩論"Modeling Loudspeaker Nonlinearities with Deep Learning"），還沒有找到打包成可直接使用的開源工具或商業套件。AERIS若已有自己的非線性失真分析流程（本輪未取得AERIS實際用哪套模擬工具，見[25](25_TECH_RADAR_CANDIDATE_LONGLIST.md)候選#36-38的篩除說明），**這批AI預測方法值得列入觀察名單**，但目前還停留在「論文證明可行」而非「有現成工具可以馬上用」的階段，不建議現在投入時間自建。

---

## 雷達條目 #22：MEGIS 對照 — AI驅動GD&T自動公差標註工具

**檢索日期**：2026-09-26　**對照對象**：延伸雷達#12（DFM/公差堆疊分析）。

### 外部現況
- 2026年AI GD&T工具已從「檢查有沒有標公差」進化到「理解公差方案是否真的表達了設計意圖」。
- **CoLab AutoReview**：內建GD&T完整性/一致性檢查，偵測缺失基準(datum)、標示超出公司標準的公差、辨識可能造成檢驗或製造歧義的標註違規；更進階的AI agent能讀取原生幾何、跨視圖比對標註、解讀工程意圖，抓出靜態規則抓不到的問題。
- **公差資料擷取**：頂尖AI平台已能直接從掃描件/PDF自動擷取尺寸與公差，透過API把CAD/PDF圖面裡的公差、尺寸、GD&T框架自動化擷取出來。
- 發展方向：AI工具正在權衡公差鬆緊與成本/品質/可製造性，並用自然語言處理把工程需求翻譯成正確的GD&T標註，朝向符合ASME Y14.5/ISO GPS標準的智慧化系統前進。

Sources: [AI Tools for CAD Standards Enforcement: Your Complete Guide to Automated Compliance (2026)](https://www.colabsoftware.com/guides/ai-tools-for-cad-standards-enforcement-your-complete-guide-to-automated-compliance-2026) · [The Best AI Tools for Better GD&T (CoLab)](https://www.colabsoftware.com/post/the-best-ai-tools-for-better-gd-t) · [Best AI Solution for GD&T in 2026 (Energent.ai)](https://www.energent.ai/use-cases/en/compare/ai-solution-for-gdt)

### 🟢 建議評估
MEGIS的Gate制（尤其G4的公差堆疊/組裝關係審查）目前是人工審查性質。**CoLab AutoReview這類工具示範了「AI讀原生幾何＋跨視圖比對＋抓設計意圖不一致」的具體做法**，可以評估是否能加裝在G4審查階段之前做「第一輪自動掃描」，把明顯的GD&T完整性問題（缺基準、標註不一致）在人工Gate審查前先攔掉，跟雷達#12既有建議（in-CAD即時DFM檢查）互補，一個是建模階段即時提示，一個是審查前的自動掃描層。

---

## 雷達條目 #23：MEGIS/AERIS 對照 — 拓樸優化開源工具 + FEA/CAE Surrogate Model AI加速

**檢索日期**：2026-09-26　**對照對象**：跨MEGIS（拓樸優化）與AERIS/MEGIS共通（模擬加速）。

### 外部現況
- **開源拓樸優化工具2026年趨勢**：`topoptlab`（2026-02發布，模組化benchmarking框架）、`SOPTX`（2026-05，基於FEALPy，解耦分析與優化，支援NumPy/PyTorch/JAX多後端）、`STORX`（MATLAB物件導向框架）、`OpenPicso`（模組化GUI+CLI+Python函式庫）。趨勢是走向「模組化、多後端、更好的軟體工程實踐」而非單體工具。
- **FEA Surrogate Model**：AI加速有限元分析已從學術界走向工業級IP——Bosch、Pratt & Whitney、X Development都已申請專利，明確用來在設計優化迴圈中「取代或消除FEM solver呼叫」。實例：Abaqus熱傳模擬（22MnB5熱沖壓製程）訓練出的深度學習surrogate模型，溫度場預測平均誤差僅約3°C，換來約10^4倍的速度提升。

Sources: [topoptlab: An Open and Modular Framework for Benchmarking and Research in Topology Optimization](https://joss.theoj.org/papers/10.21105/joss.09105) · [SOPTX: A Modular and Extensible Framework for Topology Optimization](https://www.global-sci.com/cicp/article/view/24166) · [AI-accelerated FEA technology landscape 2026 (Patsnap)](https://www.patsnap.com/resources/blog/articles/ai-accelerated-fea-technology-landscape-2026/) · [AI in engineering 2026: How simulation, digital twins and surrogate models are redefining CAE](https://www.tgm.solutions/en/top-technologies-in-engineering/ai-in-engineering-2026-how-simulation-digital-twins-surrogate-models-are-redefining-cae/)

### 🟢 建議評估
如果MEGIS的Gate制流程中有需要跑拓樸優化或重複性高的FEA模擬（例如反覆迭代的結構驗證），**這兩個技術方向都值得評估**：拓樸優化開源工具（尤其`SOPTX`的多後端Python生態，跟MEGIS已用的CadQuery/Python工作流相容性可能較高）可以評估取代/補充商業工具；FEA surrogate model則是「先花時間訓練一次，之後大量迭代設計時省下重跑FEM的成本」的策略，適合MEGIS/AERIS若有需要跑大量相似結構的模擬迭代場景。**這輪只找到工業案例存在，沒有直接證據顯示MEGIS/AERIS目前的模擬工作量是否大到值得投入建置surrogate model**，屬於「值得評估，但先確認自己的模擬迭代量」的建議。

---

## 雷達條目 #24：MEGIS 對照 — CadQuery AI生態系（Text-to-CAD Copilot、MCP整合）

**檢索日期**：2026-09-26　**對照對象**：MEGIS已確認使用CadQuery（見[Audit/REPOSITORY_INVENTORY.md](../Audit/REPOSITORY_INVENTORY.md)、[Audit/GAP_ANALYSIS.md](../Audit/GAP_ANALYSIS.md)）。

### 外部現況
- CadQuery本身是成熟的開源Python參數化CAD腳本框架（基於OCCT），生態系正在快速長出AI輔助工具：**「CAD/CAE Copilot」**——一個AI原生的CAD/CAE/CAX工作台，主打給AI agent用，具備Text-to-CAD、text-to-CAE、真實build123d/OpenCASCADE幾何、可編輯參數、穩定拓撲指標(stable topology pointers)、確定性critique，並且**暴露MCP server工具**。
- 另有「Text23D Mechanical CAD Explorer」——從對話式輸入生成/精煉3D參數化CAD模型，一個自我校正的text-to-CAD agent能把英文描述轉成經驗證的CadQuery 3D模型。
- 也已出現公開的「CadQuery Skill for Claude Code」（skillselion.com），顯示CadQuery + Claude Code agent工作流已經有現成的整合範例可參考。

Sources: [CadQuery – a Python module for building parametric 3D CAD models](https://blog.adafruit.com/2026/04/21/cadquery-a-python-module-for-building-parametric-3d-cad-models/) · [Cadquery Skill for Claude Code (Skillselion)](https://skillselion.com/skills/fandhe-ai/agent-reference-skills/cadquery) · [GitHub - cadquery/cadquery](https://github.com/cadquery/cadquery)

### 🟢 建議評估（直接可行動）
這是本輪對MEGIS最直接可行動的發現之一：MEGIS已經用CadQuery，而**CadQuery生態系本身已經長出「暴露MCP server工具」的AI copilot**——這剛好跟本SuperSystem目前的AIECP/AI agent技術棧（MCP-based）天然相容。如果MEGIS未來想讓G4模組的幾何草案生成階段（雷達#3已提到的痛點）加速，**先評估CadQuery生態系自己的AI copilot工具，會比評估Zoo.dev/AdamCAD這類獨立商業Text-to-CAD產品（雷達#3既有建議）更貼合MEGIS現有的技術棧**，整合成本可能更低。這點更新了雷達#3的建議方向：不是「要不要導入Text-to-CAD」，而是「優先看CadQuery自己生態系裡的AI工具」。

---

## 雷達條目 #25：跨專案協定對照 — MCP 2026-07-28 重大改版

**檢索日期**：2026-09-26　**對照對象**：本session本身即透過MCP呼叫工具，AIECP的Provider整合架構直接相關。

### 外部現況
- **2026-07-28版MCP規格**是官方稱「自發布以來最大幅度的修訂」：協定核心變成**無狀態(stateless)**，移除session概念與初始化交握，新增Multi Round-Trip Requests、header-based routing、可快取的list結果、授權機制強化，並引入正式的擴充框架(extensions framework)。
- 原本核心功能的Tasks功能被移出核心協定、改成一個獨立extension。
- 這次改版被The Register形容為「MCP準備跟它的有狀態過去決裂」。

Sources: [The 2026-07-28 Specification (Model Context Protocol Blog)](https://blog.modelcontextprotocol.io/posts/2026-07-28/) · [Specification - Model Context Protocol 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28) · [The biggest MCP spec update ships July 28 (WorkOS)](https://workos.com/blog/mcp-2026-spec-agent-authentication) · [Model Context Protocol prepares to break with its stateful past (The Register)](https://www.theregister.com/devops/2026/07/23/model-context-protocol-prepares-to-break-with-its-stateful-past/5276722)

### 🟢 建議評估
AIECP若未來要正式把「MCP-compatible Provider」納入自己的Provider路由架構（呼應[Blueprint/12](12_PROVIDER_AND_MODEL_ROUTING.md)），**必須注意這不是舊版MCP的小補丁，而是協定核心語意改變**（有狀態→無狀態、移除初始化交握）。如果AIECP或SuperBrain現有任何MCP整合（或未來規劃）是依照舊版spec假設寫的，這次改版是**必須重新確認相容性的斷點升級**，不是可以忽略的版本號迭代。這也回應了雷達#9/#13裡「暴露OpenAI/Claude相容API」這個方向——MCP作為協定層的地位在2026年更加確立，值得AIECP把它當成跨系統介面的候選標準之一。

---

## 雷達條目 #26：跨專案協定對照 — A2A協定(Agent2Agent) + AGENTS.md標準化

**檢索日期**：2026-09-26　**對照對象**：直接對應[17](17_RISK_GAP_CONFLICT_REGISTER.md)的 G-01/G-02/G-03（AIECP↔AERIS/MEGIS/SuperBrain介面完全缺失）。

### 外部現況
- **A2A（Agent2Agent Protocol）**：Google於2025-04發起、現已移交Linux Foundation治理的開放協定，讓不同框架/廠商/領域的自治AI agent能互相發現能力、委派任務、協調複雜工作流，走HTTP/JSON-RPC/Server-Sent Events等既有web標準，並提供安全/稽核/合規防護。定位是與MCP互補：MCP給agent工具與上下文，A2A給agent與agent之間的溝通協定。
- **AGENTS.md**：已被超過60,000個開源專案使用，是Codex/Cursor/Copilot/Gemini CLI/Aider/Windsurf/Zed/Factory/Jules等20多個工具原生支援讀取的格式，現由Linux Foundation旗下的Agentic AI Foundation管理。格式刻意極簡（無強制欄位、標準Markdown），2026年的定位是「業界事實標準的agent context檔案慣例」而非正式規格。

Sources: [Agent2Agent Protocol (Google Developers Blog)](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/) · [Linux Foundation Launches the Agent2Agent Protocol Project](https://www.linuxfoundation.org/press/linux-foundation-launches-the-agent2agent-protocol-project-to-enable-secure-intelligent-communication-between-ai-agents) · [A Comparative Study of MCP and A2A for Inter-Agent Coordination in LLM-Based Systems](https://arxiv.org/pdf/2607.23884) · [What Is AGENTS.md? How to Write One in 2026 (Tembo)](https://www.tembo.io/blog/agents-md) · [AGENTS.md Complete Guide 2026: Spec, Tools, Examples](https://codersera.com/blog/agents-md-complete-guide-2026/)

### 🟢 建議評估
G-01/G-02/G-03這幾個「AIECP↔其他系統介面完全缺失」的高優先缺口，**目前是各專案自己定義任務schema（例如AIECP的`aecp.task/v1`），沒有共通協定**。A2A協定剛好是為了解決「不同廠商/框架的自治agent怎麼互相發現能力、委派任務」這個確切問題而生，且已有Linux Foundation治理背書，值得AIECP在設計跨系統介面時列為候選標準——**不是要求AIECP現在就採用**，而是提醒：與其自創一套handshake格式，先看A2A是否已經解決了同樣的問題。另外，七個repo已經不約而同都在用AGENTS.md這個事實標準（本repo自己的CLAUDE.md也是同類角色），這件事本身值得記錄：**跨專案至少已經有一個非正式但高度一致的慣例存在**，可以作為未來設計正式跨專案協定時的參考起點。

---

## 雷達條目 #27：AIECP 對照 — 供應鏈證據延伸：OpenSSF Scorecard + SBOM

**檢索日期**：2026-09-26　**對照對象**：延伸雷達#5/#16的SLSA對齊建議。

### 外部現況
- 2026年供應鏈安全工具鏈標準組合：**OpenSSF Scorecard**（自動化評分repo的安全實踐）、**SBOM**（CycloneDX或SPDX格式的軟體物料清單）、**in-toto attestations**，三者已經是現代供應鏈安全工具（Scorecard、GitHub Dependency Graph、Chainloop、in-toto）預期互相搭配的組合。
- 實務作法：用`Syft`（Anchore開源工具）從容器映像/檔案系統產生CycloneDX/SPDX格式SBOM，用`Parlay`（Apache授權CLI）為SBOM加註授權/漏洞/維護者/scorecard資料，OpenSSF Scorecard的評分結果可以記錄成CycloneDX SBOM裡元件的屬性。

Sources: [Software supply chain security tools guide (2026, Minimus)](https://www.minimus.io/post/software-supply-chain-security-tools) · [SPDX CycloneDX standards (OpenSSF)](https://openssf.org/tag/spdx-cyclonedx-standards/) · [CycloneDX Tool Center](https://cyclonedx.org/tool-center/)

### 🟢 建議評估
延伸雷達#16的建議路徑（先上GitHub Artifact Attestations，要衝Level 3用`slsa-github-generator`）：**AIECP現有的SHA256SUMS + RELEASE_PROVENANCE.json可以再補一層SBOM**（用Syft自動產生CycloneDX格式），並搭配OpenSSF Scorecard做repo層級的自動化安全評分——這兩者都是「幾行CI設定就能加上」的低成本項目，跟雷達#16的Artifact Attestations建議是同一個「先上免費/低成本官方工具」的邏輯，可以一起排進同一輪落地評估。

---

## 雷達條目 #28：AIECP Harness 對照 — LangGraph/CrewAI/AutoGen + Temporal.io

**檢索日期**：2026-09-26　**對照對象**：AIECP自己的Mission/Task/Queue/Scheduler Harness（見[08](08_AIECP_ORCHESTRATION_ARCHITECTURE.md)）。

### 外部現況
- **LangGraph**：把agent工作流建模成有型別狀態的有向圖，適合需要最大控制權、合規、正式生產級狀態管理的企業場景。
- **CrewAI**：把多agent協作建模成「團隊(crew)」角色扮演模式，2026年v1.10.1起支援串流、A2A協定相容、MCP整合，v1.14起完全移除LangChain依賴、變成獨立框架。
- **AutoGen**（微軟）：對話式agent團隊，AG2引入GroupChat作為主要協調模式。
- **Temporal.io**：開源工作流編排引擎，2026年2月獲3億美元融資明確用於建置AI agent基礎設施，2026年3月與OpenAI Agents SDK正式整合(GA)，提供**Durable Execution**（自動失敗復原、內建重試、狀態持久化）與**Time-Travel Debugging**（完整事件歷史回放）。業界認為工作流引擎是「生產級agent的必要基礎設施」已成共識。

Sources: [LangGraph vs CrewAI vs AutoGen: Which AI Agent Framework Should Your Enterprise Use in 2026?](https://pub.towardsai.net/langgraph-vs-crewai-vs-autogen-which-ai-agent-framework-should-your-enterprise-use-in-2026-3a9ebb407b09) · [CrewAI vs LangGraph vs AutoGen vs OpenAgents — Best AI Agent Framework (2026)](https://openagents.org/blog/posts/2026-02-23-open-source-ai-agent-frameworks-compared) · [Agentic AI Workflows: Why Orchestration with Temporal is Key](https://intuitionlabs.ai/articles/agentic-ai-temporal-orchestration) · [From agent zoo to agent orchestra: The benefits of Temporal as your enterprise agentic control plane](https://temporal.io/blog/from-agent-zoo-to-agent-orchestra-temporal-agentic-control-plane)

### 🟡 持續觀察
AIECP現有的Mission→Task→Queue→Scheduler架構方向跟業界的workflow engine邏輯一致，**不需要現在就換架構**。但如果未來AIECP的Queue/Scheduler遇到「失敗重試、跨步驟狀態持久化、長時間執行任務的可觀測性」這類問題時，**Temporal.io的Durable Execution模式值得評估**——它解決的正是「agent任務執行到一半失敗要怎麼恢復」這個問題，跟[17](17_RISK_GAP_CONFLICT_REGISTER.md)的R-01（無跨機故障轉移設計）也有間接關聯。三個multi-agent框架（LangGraph/CrewAI/AutoGen）則更適合「agent之間怎麼分工協作」這個問題，跟AIECP目前「Codex Builder + Claude Reviewer」的固定角色分工模式不完全對應，屬於觀察項目而非急迫建議。

---

## 雷達條目 #29：AIECP Builder 對照 — GitHub Copilot Coding Agent vs Devin

**檢索日期**：2026-09-26　**對照對象**：AIECP的Codex OFFICIAL/PEGA Builder角色。

### 外部現況
- **Devin**（Cognition Labs）：完全自主，從任務描述獨立完成規劃、架構、寫程式、測試、除錯到部署的整個流程，在隔離虛擬機容器中異步執行；適合「可以完全委派」的任務，但**運作時不會每步都要求核准**。
- **GitHub Copilot Coding Agent**：異步在沙盒中工作，指派GitHub issue後開draft PR給人類審查，2026年評測中在「即時完成、IDE整合、社群生態」這些日常開發指標上領先（8.79/10 vs Devin 7.64/10），但Devin在「自主執行、大型context window、企業功能」上仍有優勢。
- 定價：GitHub Copilot Pro $10/mo起；Devin Free/Pro $20/mo/Max $200/mo。

Sources: [Devin vs GitHub Copilot Workspace in 2026](https://www.mgsoftware.nl/en/vergelijking/devin-vs-github-copilot-workspace) · [Devin 2.0 vs. GitHub Copilot Agent Mode: 2026 Comparison](https://weavai.app/blog/en/2026/05/19/devin-2-0-vs-github-copilot-agent-mode-2026-comparison/) · [Devin vs Claude Code vs Copilot Workspace (2026)](https://pristren.com/blog/devin-vs-claude-code-vs-github-copilot-workspace/)

### 🟡 持續觀察
AIECP現有原則「Neither OFFICIAL nor PEGA may self-declare engineering completion」（[00](00_MASTER_BLUEPRINT.md)問題24）比Devin的「完全自主、不逐步要求核准」模式更保守，這**符合AIECP自己的治理哲學（Builder不能自我核准），不建議往Devin的完全自主模式靠攏**。GitHub Copilot Coding Agent的「異步沙盒＋開draft PR給人審查」模式其實跟AIECP現有Codex worktree隔離＋人工merge的流程更接近，可以作為「業界怎麼包裝類似流程給更廣泛使用者用」的介面設計參考，但不構成需要採用外部工具的理由——這條主要是確認AIECP自己的既有方向沒有走偏。

---

## 雷達條目 #30：AIECP/#11 沙盒對照 — Windows Execution Containers(MXC) SDK

**檢索日期**：2026-09-26　**對照對象**：直接回應雷達#11「Windows ARM64業界資料稀少」這個明確缺口。

### 外部現況（重要：官方直接回應了這個技術缺口）
- 微軟在**2026-06-02（與RTX Spark Dev Box同一場Build 2026發表會）** 公開了 **Microsoft Execution Containers (MXC) SDK**的早期預覽——一個跨平台、政策驅動(policy-driven)的agent執行層，涵蓋Windows與WSL，開發者定義要限制什麼，Windows在執行期一致地強制套用這些限制。
- MXC提供一個跨隔離原語的抽象層，讓開發者不用自己管理底層隔離細節；同一套政策模型與SDK可以對應到不同的隔離結構（依工作負載風險程度而定，例如coding agent跟企業資料處理agent不需要一樣的防護等級）。
- 對照：Codex在Linux上用Bubblewrap做namespace隔離、macOS上用Seatbelt(`sandbox-exec`)、Windows上用受限token行程(restricted-token processes)——這正是既有雷達#11提到「Windows沙盒隔離業界資料稀少」的現況，而MXC是微軟自己在同一時間點推出的官方解法。

Sources: [Windows platform security for AI agents (Windows Developer Blog)](https://blogs.windows.com/windowsdeveloper/2026/06/02/windows-platform-security-for-ai-agents/) · [Comparing Sandboxing Approaches for AI Agents (Docker Blog)](https://www.docker.com/blog/comparing-sandboxing-approaches-ai-agents/) · [Harness Engineering: Anatomy, Architecture, and Evolution of Coding Agents](https://arxiv.org/pdf/2609.00006)

### 🟢 建議評估（升級為本輪高優先，直接回應既有缺口）
雷達#11指出「Windows/ARM64沙盒隔離業界資料稀少，AIECP可能要自己試出來」，**這輪找到微軟官方在同一時間點（2026-06-02）針對這個確切問題推出的MXC SDK**——這改變了雷達#11的結論：不是「業界沒有答案」，而是「答案剛剛在2026年中期才出現，是早期預覽階段」。AIECP的Codex OFFICIAL/PEGA若要在Windows/ARM64（考慮到SPARK-AGAVE用Grace ARM CPU，見雷達#31）上做沙盒隔離驗證，**MXC SDK應該是第一個要評估的官方選項**，而不是繼續假設「要自己組合底層隔離原語」。由於是早期預覽，需注意穩定性與功能完整度可能還不到生產級。

---

## 雷達條目 #31：硬體/CI 對照 — Windows ARM64工具鏈成熟度：WSL3 + GitHub Actions ARM64 Runner

**檢索日期**：2026-09-26　**對照對象**：延伸雷達#7（確認SPARK硬體用ARM CPU）與雷達#11（Windows ARM64缺口）。

### 外部現況（關鍵確認：SPARK-AGAVE機器是ARM64）
- 雷達#7已確認RTX Spark晶片＝20核**Grace(Arm) CPU** + Blackwell RTX GPU。這代表**SPARK-AGAVE-3/4實際上是ARM64架構的Windows機器**，不是x86——這個推論此前的雷達條目沒有明確點出，本輪補上。
- **WSL 3**：Build 2026（2026-06-02）預覽，用更輕量的準虛擬化(paravirtualized)機器取代WSL2沿用至今的Hyper-V VM後端，GPU/NPU存取改走DirectML 2.0。此前Snapdragon X Elite這類ARM機器上Ollama在WSL2裡因缺乏GPU/NPU後端只能CPU運算，**WSL 3的新架構理論上能消除這個障礙**（但需支援的硬體）。微軟明確表示Build 2026的訊息是「開發者應該把Arm64當成Windows的一級目標」。
- **GitHub Actions ARM64 Runner**：2026-01-29起，Linux/Windows arm64標準GitHub-hosted runner已支援私有repo（此前只有公開repo）；Windows ARM硬體的**self-hosted runner支援自2022年就有，但2026年現況仍是public preview/beta狀態**，尚未GA。

Sources: [Build 2026: Native Windows, Arm, Local AI & Agent-First Hardware](https://windowsforum.com/news/build-2026-native-windows-arm-local-ai-and-agent-first-hardware-explained.423790/) · [WSL 3 at Build 2026: Near-Native GPU and NPU Passthrough Brings Local AI to Windows](https://www.techtimes.com/articles/317598/20260602/wsl-3-build-2026-near-native-gpu-npu-passthrough-brings-local-ai-windows.htm) · [arm64 standard runners are now available in private repositories (GitHub Changelog)](https://github.blog/changelog/2026-01-29-arm64-standard-runners-are-now-available-in-private-repositories/) · [Actions: Self-hosted runners now support Windows ARM64 (GitHub roadmap Issue #616)](https://github.com/github/roadmap/issues/616)

### 🟢 建議評估（更新雷達#7/#11的解讀）
這是本輪對SuperBrain/AIECP最重要的一條交叉確認：**SPARK-AGAVE-3/4是ARM64機器，這件事本身此前的雷達條目沒有講清楚**。這意味著：①AIECP若要在SPARK-AGAVE上驗證Codex OFFICIAL/PEGA worktree隔離（雷達#11卡住的ENVIRONMENT gate），驗證環境必須是**Windows on ARM64**，不是一般假設的x86 Windows；②WSL 3才是SPARK-AGAVE上跑本地AI工作負載的正確目標（不是WSL2），但WSL 3截至本輪檢索仍是Build 2026剛預覽的新東西，穩定性未知；③GitHub Actions的self-hosted ARM64 runner支援仍是preview/beta，**如果AIECP的CI要在SPARK-AGAVE本機跑self-hosted runner，這條路線目前業界本身都還不算成熟**，這解釋了雷達#11「業界資料稀少」的部分原因——不是沒人做，是這條路線2026年整體都還在早期階段。建議：等SPARK-AGAVE 10/7上市、Stephen實機到手後，第一個要做的相容性測試就是「WSL 3能否在RTX Spark上正常跑GPU/NPU passthrough」，這比自己假設WSL2堪用更保險。

---

## 雷達條目 #32：AIECP 對照延伸 — 憑證管理：Infisical Agent Vault + SPIFFE/SPIRE

**檢索日期**：2026-09-26　**對照對象**：延伸雷達#13的「推理引擎不該直接持有原始憑證」建議。

### 外部現況
- **Infisical Agent Vault**：開源、專門為AI agent設計的credential proxy——重點是「光有secret manager不夠，因為任何能通過驗證的東西都能拿到密鑰本身，要把選定的secret store放在一個credential proxy後面，讓agent永遠拿不到密鑰本身」。2026年業界定位：Vault適合100人以上工程團隊/受監管產業/複雜多雲環境；Doppler上手最快；Infisical是開源自主可控選項。
- **SPIFFE/SPIRE**：SPIFFE是規格，SPIRE是開源實作，定義「這個workload是什麼」的密碼學可驗證身分標準，是AI agent身分的合適基礎——agent是會呼叫其他agent/工具/下游模型供應商的非人類身分。2026年生產架構：每個agent容器啟動時透過SPIRE的attestation API取得SVID(SPIFFE ID)憑證，每小時輪替。**已知限制**：SPIRE要求每個workload要先在SPIRE server預先註冊，對於動態產生的sub-agent這件事需要額外自動化管線。

Sources: [Secrets for AI Agents: Vault vs Doppler vs Infisical + ESO (2026)](https://callsphere.ai/blog/vw6h-secrets-vault-doppler-infisical-eso-ai-agents-2026) · [SPIFFE: Securing the identity of agentic AI and non-human actors (HashiCorp)](https://www.hashicorp.com/en/blog/spiffe-securing-the-identity-of-agentic-ai-and-non-human-actors) · [SPIFFE/SPIRE for AI Agents: Cryptographic Workload Identity Instead of Long-Lived Service Account Tokens](https://bex.co/blog/2026/07/10/spiffe-spire-ai-agent-workload-identity)

### 🟢 建議評估
雷達#13已指出AIECP的Codex OFFICIAL/PEGA若目前直接持有GitHub token/API key本身（而非透過短效broker簽發），這是可以對齊業界最佳實踐的具體改善點。這輪找到兩個**具體可以評估的開源落地選項**：規模較小、單機/單人使用場景（目前AIECP現況）更適合先評估**Infisical Agent Vault**（開源、專為AI agent credential proxy設計、上手成本較低）；如果未來SuperBrain三機＋多個agent worker的場景變複雜到需要「每個agent有自己密碼學可驗證身分」，**SPIFFE/SPIRE**是更完整但也更重的方案（需要預先註冊機制，不適合現階段一人維運的AIECP）。建議順序：先評估Infisical Agent Vault這種輕量選項，SPIFFE/SPIRE留到系統規模明顯變大時再考慮。

---

## 雷達條目 #33：JN1-UOA 落地選型 — Langfuse vs Arize Phoenix Self-Host詳細比較

**檢索日期**：2026-09-26　**對照對象**：延伸雷達#6的「下一步」建議（具體評估Langfuse vs Phoenix落地成本）。

### 外部現況
- **架構**：Phoenix是單一process、OpenTelemetry原生、Elastic License 2.0（非OSI核准的source-available授權），可以單一Docker容器直接跑（預設SQLite，正式環境用Postgres 14+）；Langfuse拆分成交易資料(Postgres)、分析(ClickHouse)、queue/cache(Redis)、事件內容(S3相容儲存)四個服務，需要web+worker容器分開跑。
- **授權**：Langfuse核心程式碼MIT授權；Arize Phoenix是ELv2（source-available但非OSI核准）。
- **強項分工**：Langfuse領先於tracing規模、多agent可觀測性、有版本控管的prompt管理；Phoenix領先於評測深度、RAG專用tracing、一行程式碼自動裝配(auto-instrumentation)。
- **2026年新發展**：ClickHouse於2026年1月收購Langfuse，OLAP引擎與出品公司現在是同一家。

Sources: [Arize Phoenix vs Langfuse (2026): Self-Host, OTel, and Event Caps Settled](https://www.morphllm.com/comparisons/arize-phoenix-vs-langfuse) · [Langfuse vs. Arize AX and Arize Phoenix (Langfuse)](https://langfuse.com/resources/engineering/best-phoenix-arize-alternatives) · [Langfuse vs Arize Phoenix: License, Self-Hosting (2026)](https://www.agenticwire.news/article/langfuse-vs-arize-phoenix)

### 🟢 建議評估（回答雷達#6的懸而未決問題）
對JN1-UOA這種**一人維運、需要監管七個異質系統**的場景，**Phoenix的「單一process、MIT-like但實際是ELv2授權、SQLite可跑」部署模式比Langfuse的四服務架構更輕量、維運成本更低**——這點對Stephen這種資源受限的操作特別重要。但如果JN1-UOA未來真的需要「多agent可觀測性」與「跨系統prompt版本管理」（考慮到要監管AIECP/AERIS/MEGIS/Voice Agent/SuperBrain這麼多子系統），**Langfuse的MIT授權與更完整的LLM engineering platform功能**可能在長期更合適。建議：先用Phoenix做最小可行的可觀測性驗證(PoC)，因為部署成本低；如果之後發現需要更完整的跨系統prompt/tracing管理，再評估遷移到Langfuse。

---

## 雷達條目 #34：成本優化 — Claude Prompt Caching 2026-09-01 降價75%

**檢索日期**：2026-09-26　**觸發原因**：直接對應本repo建立初衷——Stephen每週約$20額度的預算限制（見[README.md](../README.md)「建立初衷」段落）。

### 外部現況（直接影響Stephen預算的具體變化）
- Anthropic prompt caching定價機制：5分鐘cache write為base價格1.25倍、1小時cache write為2倍，**cache read只要base價格的0.1倍**。
- **2026-09-01起，Claude prompt caching定價再降75%**：cache read降到每百萬token $0.25。實作得當的prompt caching可以讓輸入token成本降低60-90%，尤其是長system prompt、大型RAG上下文、或session內累積的對話歷史這類重複性高的場景，能省下70-90%的輸入端費用。
- 經濟效益前提：要有足夠多次的cache read才能攤銷cache write的溢價成本——換句話說，**單次性、不重複的prompt不會從快取受益**。

Sources: [Prompt Caching for Claude: Cut Your API Bill 60% in Production](https://www.aimagicx.com/blog/prompt-caching-claude-api-cost-optimization-2026) · [Prompt caching - Claude Platform Docs](https://platform.claude.com/docs/en/build-with-claude/prompt-caching) · [Claude Prompt Caching Pricing: 75% Cheaper Cache Reads (2026)](https://aimoneylabjuliangoldie.com/blog/claude-prompt-caching-pricing/) · [Prompt Caching in 2026: Cut Azure OpenAI and Claude Costs](https://technspire.com/en/blog/prompt-caching-2026-real-cost-wins)

### 🟢 建議評估（最直接對應本repo存在理由的一條）
這條直接回答本repo「建立初衷」段落點出的痛點：Stephen每週~$20額度追不上全球AI進展。**2026-09-01這次75%降價（cache read降到$0.25/M token）意味著：任何重複性高的呼叫模式（例如AIECP的Codex/Claude Reviewer反覆讀同一份長CLAUDE.md/BLUEPRINT.md context、或本SuperSystem repo自己每次雷達更新都要重新讀七個repo的長文件）現在用prompt caching的成本效益比降價前更高**。具體建議：檢查AIECP、本SuperSystem repo自己的Claude Code使用模式，是否有「同一份長system prompt/CLAUDE.md/BLUEPRINT.md反覆被當作輸入」的場景——如果有，**確保這些場景有命中cache（而不是每次都當成新輸入計費）**，可能是目前所有雷達建議裡「不花時間開發、純粹調整使用方式」就能拿到立即效益最大的一條。

---

## 雷達條目 #35：成本優化 / AIECP Provider路由 — AI Gateway/Router落地選項：LiteLLM + RouteLLM

**檢索日期**：2026-09-26　**對照對象**：延伸雷達#34的成本主題，並對應AIECP/SuperBrain的Provider路由設計（[12](12_PROVIDER_AND_MODEL_ROUTING.md)）。

### 外部現況
- **LiteLLM**：開源LLM閘道，Python SDK將多供應商（Anthropic/OpenAI/Cohere等）統一成OpenAI相容介面，自架為預設模式；生產架構是無狀態LiteLLM Proxy + 雙層快取(記憶體L1、Redis L2) + 依健康度/延遲/剩餘rate-limit選供應商的router，並支援語意快取(semantic cache)進一步降低重複請求成本。
- **RouteLLM**：ICLR 2025研究成果，用訓練過的分類器判斷簡單查詢是否可以交給輕量模型處理。實測在MT-Bench上比商業路由產品(Martian、Unify AI)便宜40%以上、同等效果；標準benchmark上可省下超過85%花費同時保留95%頂級模型的回應品質。

Sources: [Cut AI API Costs 14x With a LiteLLM Router (2026)](https://tech-insider.org/litellm-multi-model-ai-router-2026/) · [LLM Gateway in Production: Multi-Provider Routing + Fallbacks with LiteLLM](https://devopsboys.com/blog/llm-gateway-litellm-multi-provider-routing-production-2026) · [LLM Router 2026: RouteLLM Benchmarks, Cut Costs 30-85%](https://klymentiev.com/blog/llm-router)

### 🟢 建議評估
AIECP目前的Provider路由（[12](12_PROVIDER_AND_MODEL_ROUTING.md)）與SuperBrain的golden-set分流設計，都是在解決「什麼任務該用哪個供應商/模型」這個問題，但**目前沒有看到自架的統一閘道層**。LiteLLM可以評估作為「AIECP呼叫Claude/GPT/Gemini各家API的統一入口」，把快取、路由、重試/fallback、預算控管都收斂到一層；RouteLLM則是更進一步——**用分類器自動判斷簡單任務降級到便宜模型**，這對Stephen每週$20預算的場景特別有意義：如果AIECP的Task有明確難度分級（例如inspect-workspace這類簡單任務vs需要深度推理的任務），RouteLLM式的路由可以把「不需要用最貴模型做的事」自動導去便宜路徑。建議與雷達#34一起評估，因為兩者都是「不改架構本質、純粹省錢」的低風險項目。

---

## 雷達條目 #36：G-04 驗證方法更新 — Mutation Testing + Property-based Testing + 多Agent Debate警示

**檢索日期**：2026-09-26　**對照對象**：延伸雷達#10/#15，G-04（SPARK-AGAVE-4獨立驗證SPARK-AGAVE-3協議設計）。

### 外部現況
- **Mutation Testing對AI生成程式碼的必要性**：2026年業界觀察是「AI生成的測試在追求覆蓋率指標，但覆蓋率是執行的代理指標，不是驗證的代理指標」。具體案例：一個有20個測試方法、覆蓋率不錯的測試檔案，跑mutation testing只拿到70%分數，3個mutant存活，測試漏掉了邊界條件與功能旗標行為這類關鍵案例。2026年共識明確指出：**不該讓同一個AI模型同時寫程式碼跟寫測試**。
- **Property-based Testing**：結合mutation testing用來斷言「不管輸入怎麼隨機變化都該成立的不變量」（例如排序約束、單調性），這種組合被認為「AI幾乎無法投機取巧地繞過」。
- **多Agent Debate的警示（呼應既有雷達#10）**：2026年研究進一步證實「多數決在高度相關的LLM錯誤下會系統性鎖定錯誤答案」（稱為Tyranny of the Majority）；另有研究發現「無引導的同質agent debate，交換非結構化推理後多數決，考慮token成本後並沒有顯著優於單純的自我修正(self-correction)」；業界目前發現「用一個更小的模型當專門驗證者、搭配精心設計的評分標準，持續優於用更大的模型臨時檢查自己的作業」。

Sources: [Mutation Testing for AI-Generated Code: A Practical Guide (Augment Code)](https://www.augmentcode.com/guides/mutation-testing-ai-generated-code) · [Your AI-Generated Tests are Lying to You (Medium)](https://singhpr.medium.com/your-ai-generated-tests-are-lying-to-you-and-what-to-do-about-it-57fb0e5f2783) · [Minority Sentinel: When to Overturn Majority Voting in Multi-Agent LLM Debates](https://arxiv.org/html/2606.29270v1) · [The Cost of Consensus: Isolated Self-Correction Prevails Over Unguided Homogeneous Multi-Agent Debate](https://arxiv.org/html/2605.00914v1)

### 🟢 建議評估（G-04設計的具體補充，延伸雷達#10/#15）
這輪找到的證據**進一步強化雷達#10/#15已經給的警示，並提供更具體的落地技術**：①如果G-04設計中SPARK-AGAVE-4的驗證邏輯包含「跑測試」這一步，**mutation testing應該是驗證測試品質本身夠不夠格的標準做法**（不只是看SPARK-AGAVE-3寫的測試覆蓋率，而是主動變異程式碼看測試會不會抓到）；②Property-based testing可以補強「邊界案例」這種傳統範例測試容易漏掉的地方；③2026年最新研究**再次確認「多個LLM互相debate」不是可靠的驗證手段**（甚至可能因為相關錯誤而系統性放大共同盲點），這跟雷達#10「LLM Judge與執行式驗證器有32.4%分歧率」的警示方向完全一致，進一步佐證G-04設計時**應該優先讓SPARK-AGAVE-4做「跑mutation testing/property-based testing這類確定性檢查」，而不是讓它用另一個LLM去讀SPARK-AGAVE-3的推理過程做「debate式」驗證**。

---

## 雷達條目 #37：跨專案介面設計參考 — Contract Testing (Pact)

**檢索日期**：2026-09-26　**對照對象**：直接對應[17](17_RISK_GAP_CONFLICT_REGISTER.md)的G-01/G-02（AIECP↔Voice Agent、AIECP↔AERIS/MEGIS介面完全缺失，兩者都標記「高」嚴重度）。

### 外部現況
- **Pact**是消費者驅動契約測試(consumer-driven contract testing)的主流開源框架：消費者(consumer)定義自己需要provider提供什麼、寫成機器可讀契約，provider端獨立驗證自己是否還能滿足這個契約——解決的正是「整合測試很貴，但服務又需要互相相容」這個問題，讓每個團隊可以獨立開發部署，同時對「不會破壞消費者、也不會被provider端變更破壞」有高度信心。
- 2026年AI相關應用：對AI驅動的團隊，contract testing被認為是驗證AI coding agent產出的API是否正確、向後相容、安全的關鍵手段。

Sources: [Pact Contract Testing: The Complete 2026 Guide (Pact JS)](https://qaskills.sh/blog/contract-testing-pact-complete-guide.html) · [Ultimate Guide - The Best API Contract Testing Tools of 2026](https://www.testsprite.com/use-cases/en/the-top-api-contract-testing-tools) · [Pact Testing Explained: Contract Testing for Reliable Microservices](https://www.baserock.ai/blog/pact-testing)

### 🟢 建議評估（直接對應G-01/G-02，高優先）
G-01（Voice Agent↔AIECP介面缺失）與G-02（AIECP↔AERIS/MEGIS派工介面缺失）都是本repo登錄的「高」嚴重度缺口，本質上都是「介面契約不存在」的問題。**Pact的消費者驅動契約測試模式提供一個具體的設計起手式**：與其等AIECP和AERIS/MEGIS哪天真的要對接時才發現雙方假設不一致，可以先讓「消費者」（例如未來若AIECP要呼叫AERIS/MEGIS）用Pact寫下「我預期呼叫這個介面會得到什麼」的契約，AERIS/MEGIS端（如果將來真的要暴露介面）獨立驗證能否滿足。**這不是要求AERIS/MEGIS現在就開放API**（違反CLAUDE.md「不修改其他repo」與「保持各專案自治」的鐵律），而是給Stephen或任何一方未來真的要設計這個介面時，一個「不用整合測試就能先把雙方預期講清楚」的具體方法論參考。

---

## 雷達條目 #38：容錯設計參考 — Chaos Engineering for Multi-Node AI Clusters

**檢索日期**：2026-09-26　**對照對象**：對應[17](17_RISK_GAP_CONFLICT_REGISTER.md)的R-01（ULTRA-MAERA-2單點故障風險）與[14](14_FAILURE_RECOVERY_AND_RESILIENCE.md)（跨機故障轉移未定義）。

### 外部現況
- 2026年AI叢集的chaos engineering已有機器可檢驗的標準：2026-08發布的一套「AI叢集chaos engineering保真度標準」，包含八層故障模型(fault model)、chaos實驗的spec schema、會在CI中擋下錯誤分層實驗的linter、以及20個參考實驗的目錄。
- **ReliabilityBench**（2026年1月）：針對LLM agent的chaos-engineering式故障注入框架，涵蓋一致性/穩健性/容錯性三維度的可靠性介面，故障注入涵蓋逾時、rate limit、部分回應、schema drift。
- **LitmusChaos**：CNCF託管的開源Kubernetes原生chaos平台，2026年已成熟到生產可用等級，含ChaosHub（預建實驗庫）與ChaosCenter（編排介面）。

Sources: [Adaptive Fault Injection Planning for Multi-Layer Self-Healing AI Infrastructure](https://arxiv.org/pdf/2607.16161) · [ai-cluster-chaos-fidelity (PyPI)](https://pypi.org/project/ai-cluster-chaos-fidelity/) · [Chaos Engineering for AI Agent Systems: Fault Injection, Resilience Testing, and Production Hardening (Zylos Research)](https://zylos.ai/research/2026-04-09-chaos-engineering-ai-agent-systems/)

### 🟡 持續觀察
SuperBrain目前規劃了「Spark斷線30秒判定OFFLINE、收回lease重新派工」的單機層級容錯機制，但**尚未實作、也不是三機互為備援的完整架構**（見[14](14_FAILURE_RECOVERY_AND_RESILIENCE.md)）。ReliabilityBench這類「故障注入框架」提供了一個具體的驗證方法論：**等SuperBrain的容錯機制真的實作出來後，可以用故障注入的方式主動測試（例如刻意讓其中一台Spark斷線、注入逾時/rate limit），而不是等真的故障才發現設計有漏洞**。但目前SuperBrain連基本容錯機制都還沒實作（P0未完成），這個主題屬於「等基礎機制做出來之後才用得上」的觀察項目，不是現階段優先事項。

---

## 雷達條目 #39：#7硬體對照延伸 — Windows Copilot+ PC NPU / Windows ML / Aion 1.0 On-Device SLM

**檢索日期**：2026-09-26　**對照對象**：延伸雷達#7對Surface RTX Spark Dev Box官方軟體工具鏈的記載。

### 外部現況
- **Windows ML**是微軟建議的NPU推論介面（取代DirectML的定位），提供CPU/GPU/NPU的硬體加速推論；Copilot+ PC上的NPU是針對「小型、持續運作模型」（約40億參數以下）調校的固定功能加速器。
- **Build 2026(2026-06-02)公告**：微軟開放更多Windows AI API，**Copilot+ PC上的免費本地推論現在是Windows開發的一級目標，不需要雲端依賴**；同時發布**Aion 1.0**——內建於系統的裝置端小型語言模型(SLM)家族，14B參數、32K上下文，隨附在支援的裝置上。微軟的開發者訴求從「Copilot+獨佔功能」轉向「Windows ML、本地模型、跨CPU/GPU/NPU的異質加速」。

Sources: [Windows AI Models at Build 2026: Free On-Device Inference Is Now a First-Class Build Target](https://chatforest.com/builders-log/microsoft-build-2026-windows-ai-models-aion-local-inference-builder-guide/) · [Build 2026: Windows AI Shifts to Local Agents on Any Hardware](https://windowsforum.com/news/build-2026-native-windows-arm-local-ai-and-agent-first-hardware-explained.423790/) · [Develop AI applications for Copilot+ PCs (Microsoft Learn)](https://learn.microsoft.com/en-us/windows/ai/npu-devices/)

### 🟢 建議評估
延伸雷達#7既有記載（SPARK Dev Box預裝Windows ML＋Windows Copilot Runtime）：**Aion 1.0這個微軟官方隨附的14B/32K-context裝置端SLM，是一個此前雷達沒有提到的具體選項**——如果SuperBrain未來有「輕量、常駐、不需要動用整台Spark的128GB模型」的任務（例如簡單分類、路由判斷），Aion 1.0跑在NPU上可能比動用GPU跑更大模型更省電、更快啟動，值得跟雷達#2/#19的MoE模型選型一起評估「哪些任務適合丟給NPU上的輕量常駐模型，哪些才需要動用GPU大模型」這個分層策略。

---

## 雷達條目 #40：Voice Agent 對照延伸 — Anthropic Computer Use 2026-08-19 GA

**檢索日期**：2026-09-26　**對照對象**：延伸雷達#1，Voice Agent的「語音控制Windows桌面」核心能力對標。

### 外部現況
- **2026-08-19**，Anthropic的computer use、browser use工具、Files API、Agent Skills API同一天一起脫離beta、正式GA（`computer_toolset_20260801`）。重要改進：**現在支援批次動作(batch actions)**，agent可以在同一輪次執行多個動作，不用每步都等待確認。
- 新增**browser use工具**（`browser_toolset_20260801`）：讀取accessibility tree、操作表單、管理分頁、處理檔案上傳，跟computer use（給agent一個可控制的虛擬桌面）互補。
- 部署範圍：在支援的macOS/Windows系統上，Claude Desktop、Cowork、Claude Code可以在「監督式研究預覽」下控制經核准的應用程式（Pro/Max用戶）。

Sources: [Anthropic Makes AI Agent Tools Production-Ready (Enterprise DNA)](https://enterprisedna.co/resources/news/anthropic-browser-use-computer-use-skills-api-enterprise-ga-august-2026/) · [Anthropic's Claude Computer Use Agent (Tech Insider)](https://tech-insider.org/anthropic-claude-computer-use-agent-2026/) · [Claude Code Can Now Run Your Desktop (DevOps.com)](https://devops.com/claude-code-can-now-run-your-desktop/)

### 🟡 持續觀察
Voice Agent的核心能力是「完全離線的語音控制Windows桌面」，Anthropic Computer Use則是「雲端Claude透過螢幕截圖控制桌面，需要網路連線」——**兩者的離線/雲端前提完全相反**，這跟雷達#1既有結論一致：不建議為了「雲端方案功能聽起來更完整」就放棄Voice Agent的離線設計初衷。但**batch actions這個新功能**（一輪執行多個動作而非每步等確認）這個介面設計思路，如果Voice Agent未來要優化「語音下達多步驟指令」的執行效率，值得參考其批次執行的介面設計模式，而不是採用Anthropic Computer Use本身（違反離線原則）。

---

## 雷達條目 #41：微調技術對照 — Unsloth QLoRA 本地微調

**檢索日期**：2026-09-26　**對照對象**：新主題——若AERIS/MEGIS未來需要針對領域術語/工作流微調小模型。

### 外部現況
- **QLoRA + Unsloth + Ollama**組合可以在單張消費級GPU（8-16GB VRAM）微調7B-8B等級的專用模型。
- Unsloth支援LoRA、QLoRA、全微調、預訓練、RL(GRPO/DPO)、FP8；QLoRA搭配Unsloth微調Gemma 4時，27B模型可塞進22GB VRAM以下，訓練速度比標準HuggingFace快1.6倍、記憶體少用60%。
- LoRA用16-bit精度、稍快稍準但VRAM用量是QLoRA的4倍；QLoRA用4-bit、稍慢稍不準但VRAM省4倍。2026年建議：大部分場景直接從QLoRA開始。

Sources: [Unsloth and Training Hub: Lightning-fast LoRA and QLoRA fine-tuning (Red Hat Developer)](https://developers.redhat.com/articles/2026/04/01/unsloth-and-training-hub-lightning-fast-lora-and-qlora-fine-tuning) · [GitHub - unslothai/unsloth](https://github.com/unslothai/unsloth) · [Fine-Tuning LLMs in 2026: LoRA, QLoRA, Unsloth, and Everything In Between](https://pub.towardsai.net/fine-tuning-llms-in-2026-lora-qlora-unsloth-and-everything-in-between-929eaf94aea2)

### 🟡 持續觀察
本輪盤點沒有找到AERIS/MEGIS/AIECP任一個來源repo提出過「需要微調自己的專屬模型」這個需求（屬於speculative候選，見[25](25_TECH_RADAR_CANDIDATE_LONGLIST.md)候選#95）。但如果Stephen未來發現「通用模型在AERIS聲學術語或MEGIS機構工程術語上表現不夠好，且靠prompt/RAG補不齊」，**QLoRA+Unsloth是目前成本最低的本地微調路徑**（單張消費級GPU即可，不需要SPARK-AGAVE等級硬體）。這是一個**先記錄起來、等真的出現需求再評估**的候選，不是現在就該投入的項目。

---

## 下一批建議雷達方向（尚未執行檢索，供 Stephen 排序）

- SPARK-AGAVE-3/4 真實機型確認後（見雷達 #7），重跑一次精確對照。
- AERIS 若已在用特定聲學模擬軟體，針對那個軟體找 2026 年的 AI 外掛/競品對照（本輪只查了通用開源工具，未鎖定特定商業軟體）。
- Voice Agent 現有 ASR 引擎的具體名稱確認後，跟雷達 #2 的 Parakeet TDT／Qwen3-Coder-Next 這類最新模型做直接 benchmark 對照。
- JN1-UOA 若真的要採用 OpenTelemetry GenAI（雷達 #6），下一步應該具體評估 Langfuse vs Arize Phoenix 兩個自架方案的落地成本。

要跑哪一個，或想跑全新主題，直接跟 Claude 說「跑雷達：XX」即可。
