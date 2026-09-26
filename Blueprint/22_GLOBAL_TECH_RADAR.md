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

## 下一批建議雷達方向（尚未執行檢索，供 Stephen 排序）

- AIECP 的 Provider Router／Evidence 分級機制，跟 2026 年業界的 Agent Control Plane 產品（見既有 [Audit/EXTERNAL_RESEARCH.md](../Audit/EXTERNAL_RESEARCH.md)）相比還缺什麼功能。
- MEGIS 的機構工程生成式流程（CadQuery 等），業界是否有更新的 AI-assisted CAD/生成式設計工具值得對照。
- SPARK-AGAVE-4 獨立驗證 SPARK-AGAVE-3（G-04）：業界 Verifier Pattern 的具體實作範例（不只是模式描述，找實際開源實作）。
- 本地大型語言模型（跑在 SPARK-AGAVE-3/4 128GB unified memory 上）目前最新一代開源模型的能力/成本對照，是否有比 SuperBrain 藍圖現有假設更好的選擇。

要跑哪一個，直接跟 Claude 說「跑雷達：XX」即可。
