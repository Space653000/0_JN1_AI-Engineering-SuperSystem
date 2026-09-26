# 23 — Tech Radar 彙整比較報告

> 這是 [22_GLOBAL_TECH_RADAR.md](22_GLOBAL_TECH_RADAR.md) 14 條雷達的**彙整版**：一張表看完「本地現況 vs 2026 最新科技 vs 本 repo 建議」。長話短說版，完整引用來源請點對應雷達條目。

## 一頁看完

| # | 主題 | 本地現況 | 2026 最新科技 | 建議 |
|---|---|---|---|---|
| 1 | 語音輸入 | Voice Agent 離線管線，中文辨識97.9%（實測） | ChatGPT GPT-Live 端到端音訊模型，232ms延遲；開源可替代方案：Parakeet TDT + Kokoro/Qwen3-TTS | 🟢 評估開源方案補強架構思路，但**不要因為外部聽起來好就重寫**——先 A/B 測試 |
| 2 | 本地LLM選型 | SuperBrain 規劃跑在128GB機器上 | 頻寬瓶頸(273GB/s)，MoE模型(Qwen3-Coder-Next/Laguna S 2.1)比dense模型適合 | 🟢 選型優先看 MoE，不是參數量 |
| 3 | 機構設計(MEGIS) | Gate制人工+CadQuery | Zoo.dev/AdamCAD 能做簡單托架，量產公差還不行 | 🟡 觀察，不取代 Gate 制 |
| 4 | 聲學模擬(AERIS) | 未知現有工具 | 無揚聲器專用AI工具；Pyroomacoustics可選 | ⚪ 現況已足夠 |
| 5 | Evidence標準(AIECP) | 自家5級證據(STATIC~OWNER-EXTERNAL) | SLSA供應鏈證明框架(Level 0-3) | 🟢 評估對齊SLSA Level 2/3，缺簽章這一步 |
| 6 | 監管資料格式(JN1-UOA) | 未設計 | OpenTelemetry GenAI語意慣例，業界標準 | 🟢 直接採用，不要自創格式 |
| 7 | 硬體身分(R-02) | "型號RTX Spark"身分未確認 | **已確認**＝Microsoft Surface RTX Spark Dev Box，2026-06-02 Build發表，**2026-10-07才正式上市**（現在還買不到） | 🔴 **P0卡住可能是因為硬體還沒上市**，不是施工延遲；上市後可直接套用官方128GB/1petaFLOP規格 |
| 8 | 喚醒詞 | Voice Agent現有引擎未知 | onnx-wakeword：<130KB、<10ms | 🟡 若現有太重可評估換 |
| 9 | 跨機推理(SuperBrain) | 兩台Spark各自獨立跑 | EXO/vLLM+Ray可切分大模型到多機 | 🟢 想跑更大模型可評估，但你的2.5GbE網路低於業界建議10GbE門檻 |
| 10 | **獨立驗證(G-04)** | 待Stephen設計 | LLM Judge驗證與執行式驗證器有32.4%分歧率；主流benchmark被證實可造假 | ⚠️ **確定性測試優先，LLM Judge只能輔助**，這是本輪最重要的警示 |
| 11 | 並行Agent沙盒(AIECP) | Codex OFFICIAL/PEGA worktree隔離，卡在Windows/ARM64驗證 | Git worktree隔離已是業界標準；更進階是容器化+防火牆 | 🟢 架構方向對，Windows/ARM64這塊業界資料也稀少，可能要自己試出來 |
| 12 | DFM審查(MEGIS) | Gate審查階段抓問題 | 2026做法：建模階段即時+審查階段完整，兩層疊加 | 🟢 評估加裝in-CAD即時檢查，往前抓問題 |
| 13 | 憑證管理(AIECP) | 未知是否短效/透過broker | 短效、限定範圍、broker簽發、推理引擎不碰原始憑證 | 🟢 評估Codex OFFICIAL/PEGA的憑證是否符合這個模式 |
| 14 | 跨專案知識庫 | 人工clone七repo盤點 | LlamaIndex/Haystack本地RAG，混合搜尋+知識圖譜 | 🟡 盤點頻率高了再考慮，現在不急 |

## 三個最該優先看的（2026-09-26 更新）

1. **#7 硬體身分——已確認，且是本輪最大發現**：SPARK-AGAVE-3/4＝Microsoft Surface RTX Spark Dev Box，**2026-10-07 才正式上市**，現在（09-26）根本還買不到。SuperBrain P0「硬體盤點未完成」很可能不是進度落後，而是**硬體還沒上市**——這改變了整個施工時程的解讀，建議把「等 10/7 上市」明確排進 Roadmap，而不是繼續當成一個懸而未決的落後項。
2. **#10 獨立驗證的警示**——直接關係到你自己要設計的 G-04，業界最新數據說「LLM互相驗證」本身就不可靠，設計時務必以確定性測試為主軸。
3. **#5 + #13**——SLSA 對齊 + 憑證管理，這兩條加起來剛好是 AIECP 從「自家證據系統」升級成「跟業界安全標準對齊」的具體路徑，而且都是評估成本低、可能收益高的項目。上市後 Surface RTX Spark Dev Box 內建的 Secured-core PC／BitLocker／Entra ID 也可以直接拿來對照評估。

## 標籤說明

🟢 建議評估（值得花時間驗證）　🟡 持續觀察（未成熟或現在不急）　⚪ 現況已足夠（不用追）

## 使用方式

這份報告是快照，2026-09-26 一次跑完 14 個主題。想更新單一條目，跟 Claude 說「更新雷達 #7」；想跑全新主題，說「跑雷達：XX」。完整細節、每條的原始引用來源，見 [22_GLOBAL_TECH_RADAR.md](22_GLOBAL_TECH_RADAR.md)。
