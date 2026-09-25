# 21 — Voice Agent × SuperBrain Phase C Integration Proposal（建議，回應 D-02）

> **定位聲明**：本文件是本 SuperSystem repo 對 Stephen、Voice Agent 專案與 SuperBrain 專案的**建議**，不是任何一方已經同意的計畫，也不是本 repo 對兩個 repo 做的架構決策（house rule #5）。是否採用、採用到什麼程度，完全由兩個專案自己的治理流程決定。本文件回應 [Blueprint/17](17_RISK_GAP_CONFLICT_REGISTER.md) 的 D-02（Voice Agent 已驗證的離線語音能力與 SuperBrain 規劃中的 Phase C 語音能力重疊，兩者互不知道對方存在），也是 [Audit/DUPLICATION_ANALYSIS.md](../Audit/DUPLICATION_ANALYSIS.md) 標記的「最直接可行的避免重造輪子的機會」的具體展開。
>
> 讀取依據：本輪重新 clone Voice Agent（HEAD `4628e30`，2026-09-24）與 SuperBrain（HEAD `c4e0f75`，2026-09-25）的最新內容。

---

## 1. 背景對照：兩邊各自的現況

### SuperBrain Phase C（規劃中，尚未動工）

來源：`0_JN1_2AGAVE128-1MAERA64/.ai/BLUEPRINT.md` 第273-298行。

Phase C 是三階段語音方案中的「離線」路徑：`XVF3800 → KWS → VAD → 本地 ASR → Intent（Spark1）→ sb → 本地 TTS`，目標是「斷網也能用，且隱私資料不上雲」，成本 US$0（同文件第281行）。其元件表（第283-292行）列出：

| 層 | Baseline | 狀態 |
|---|---|---|
| 收音 | reSpeaker XVF3800（4 麥克風、硬體 AEC/波束成形/DoA） | 尚未採購/測試（P0 硬體盤點都還沒完成） |
| 喚醒詞 | sherpa-onnx KWS（自訂中文詞「小腦」） | 規劃中，未實作 |
| VAD | Silero VAD | 規劃中 |
| ASR | Breeze-ASR（聯發科，中英混說） | 規劃中 |
| TTS | sherpa-onnx 相容中文 TTS | 規劃中 |

語音測試集規劃：200句（50中/40英/60中英混/25工程名詞/25危險指令），KPI 指令意圖≥98%、RED類100%、誤喚醒<0.5次/小時（同文件第296-298行）。**這些 KPI 目前全部是目標值，沒有任何一項有真實測試數據**——本輪盤點在 SuperBrain repo 全文搜尋，找不到任何一次真實錄音/測試的執行紀錄。

### Voice Agent 已驗證的離線語音管線（已在跑，非規劃）

來源：`Offline-Local-Voice-Agent/.ai/STATUS.md` 第14-28行（2026-09-24 查證，附「盤點方法：這次盤點直接查證，不是憑印象」的方法論聲明）。

| 項目 | 狀態 | 實測數據 |
|---|---|---|
| P0 硬體驗證 | ✅ 100% | ASR（whisper.cpp）+ LLM（llama.cpp）在 RTX Spark GPU 上跑通，`hardware_compat_report.json` 已產出 |
| P1 語音輸入 | ✅ 100% | 中文辨識率 **97.9%**（目標>95%），喚醒詞真實部署 88.5% |
| P2 意圖辨識 | ✅ 100% | 工具選擇準確率 97.1%（103句真實測試） |
| P4 安全機制 | 🟢 約95% | L0-L3 風險分級判斷、語音+文字雙確認、Emergency Stop、Memory/Logging 都已實作並真實驗證 |

模組實體位置（本輪確認的檔案路徑）：
- 喚醒詞模型：`src/wakeword/hai_xiao_zhuli_wakeword.onnx`
- 風險分級：`src/policy/risk_levels.py`（L0_READONLY/L1_ROUTINE/L2_SENSITIVE/L3_DANGEROUS，第23-26行）
- 政策引擎：`src/policy/policy_engine.py`（L2 需二次確認、L3 需 typed confirmation，第38-45行）
- 喚醒詞開關：`src/wake_setting.py`（預設關閉，用真實音檔驗證過，`.ai/STATUS.md` 第28行）

**已知限制**（誠實揭露，來源同上第28、69行）：喚醒詞對近似詞的辨別力偏弱，尚未改善；喚醒詞8小時無人值守誤觸發率的原始 KPI 沒有直接測過，改用架構設計（預設關閉）取代。

---

## 2. 模組對照表（建議映射，非任一方確認）

| SuperBrain Phase C 元件 | Voice Agent 對應模組/成果 | 建議動作 |
|---|---|---|
| 喚醒詞（sherpa-onnx KWS，中文詞「小腦」） | `src/wakeword/hai_xiao_zhuli_wakeword.onnx`（已訓練、已真實部署 88.5% 準確率） | **建議重用/微調**：Voice Agent 的喚醒詞模型是自訓練的中文詞模型，若 SuperBrain 需要不同喚醒詞（「小腦」vs Voice Agent 現用詞），建議沿用同一套訓練管線（`progress/p1_wakeword_train/`，本輪確認資料夾存在，訓練腳本細節本輪未逐行深讀，標記 `NOT FULLY VERIFIED`）重新訓練新詞，而不是從零研究喚醒詞技術路線 |
| VAD（Silero VAD） | Voice Agent P1 語音輸入管線內建 VAD（`progress/p1_vad_wakeword/`，本輪確認資料夾存在） | **建議直接重用**：兩邊都規劃/已採用 Silero VAD 家族技術，Voice Agent 已有真實運作經驗，SuperBrain 可直接參考其設定值，不必重新調參 |
| 本地 ASR（Breeze-ASR 規劃 vs whisper.cpp 已驗證） | Voice Agent 用 whisper.cpp（medium 模型，GPU），中文辨識率 97.9%（已達成且超過 SuperBrain 自訂的意圖 KPI 98% 門檻的量級） | **需要決策，非直接照搬**：SuperBrain Phase C 選型是 Breeze-ASR（因為考慮「台灣華語與中英混說」），與 Voice Agent 已驗證的 whisper.cpp 是不同技術選型。建議 SuperBrain 先用 Voice Agent 現成的 whisper.cpp pipeline 作為 baseline 起跑（節省從零建置與調校的時間），再視自己的中英混說測試集表現決定要不要換成 Breeze-ASR，而不是一開始就重新從 Breeze-ASR 建置 |
| Intent（Spark1，規劃中） | P2 意圖辨識，llama-server + JSON schema tool-calling，97.1% 準確率（103句真實測試） | **建議重用架構模式**：「llama.cpp/llama-server + JSON schema 強制工具呼叫」這個模式已經在 Voice Agent 驗證過，SuperBrain 若要在 Spark1 上做 Intent 分類，可以直接沿用同一套 tool-calling schema 設計方式，只是換成 SuperBrain 自己的任務類別（`sb submit` 對應的任務類型），不需要重新設計 intent schema 的格式 |
| 收音硬體（reSpeaker XVF3800，4麥克風、硬體 AEC/DoA） | `NOT APPLICABLE` — Voice Agent 目前無對應模組 | 這是 SuperBrain 獨有需求（多機 DoA「對著 Spark2 說話就預設派給 Spark2」，`.ai/BLUEPRINT.md` 第292行），Voice Agent 沒有類似硬體整合經驗，**這部分無法重用，需 SuperBrain 自行研發** |
| TTS（sherpa-onnx 中文 TTS，規劃中） | `UNKNOWN` — 本輪未在 Voice Agent repo 找到已驗證的 TTS 模組（Voice Agent 目前偏重語音輸入與文字/桌面操作輸出，TTS 播報部分本輪未見獨立驗證證據） | 標記 `NOT VERIFIED`：兩邊在 TTS 這一項都缺乏已驗證成果，建議列為共同待辦，而非其中一方可以直接提供給另一方 |
| 安全分級（RED 類語音指令需 100% 準確率的規劃） | L0-L3 分級 + L2 二次確認 + L3 typed confirmation，已真實驗證（`src/policy/policy_engine.py`） | **建議直接重用整套設計**：SuperBrain Phase C 規劃的「危險指令的語音規則：遇到 RED 動作…核准必須在螢幕或手機上點選 exact-action，並且要用完整句子複誦確認」（`.ai/BLUEPRINT.md` 第294行）與 Voice Agent 的 L2/L3「二次確認/typed confirmation、每次都要問、不可記住這次同意」在精神與機制上高度一致，建議 SuperBrain 的 RED 類語音規則直接以 Voice Agent 的 `risk_levels.py`/`policy_engine.py` 設計為藍本，不必重新發明一套分級邏輯 |

---

## 3. 什麼需要改、什麼可以照樣重用

### 可以（幾乎）照樣重用

1. **VAD 設定與門檻**：Silero VAD 家族選型已一致，Voice Agent 的實測參數可直接作為 SuperBrain 的起始值。
2. **L0-L3 風險分級與確認流程的設計模式**（非程式碼本身，因為兩專案是不同語言/架構的獨立系統，但邏輯與狀態機可以整套搬過去參考）。
3. **whisper.cpp + JSON schema tool-calling 的 Intent 辨識架構模式**：作為 SuperBrain Spark1 Intent 層的起跑點，即使最終選型換成別的 ASR，tool-calling schema 的設計方式仍可沿用。

### 需要改動才能用

1. **ASR 引擎選型**：whisper.cpp（Voice Agent 現用）在中英混說場景的表現，Voice Agent 自己的測試集（97.9% 中文辨識率）本輪未見「中英混說」子項的分項數據；SuperBrain 需要的正是「台灣華語與中英混說」（Breeze-ASR 的選型理由），這部分需要 SuperBrain 自己補測，不能直接假設 Voice Agent 的 97.9% 涵蓋這個場景。
2. **喚醒詞詞彙**：Voice Agent 現用詞與 SuperBrain 規劃的「小腦」不同，需重新訓練（沿用管線，換訓練資料）。
3. **多機/DoA 整合**：Voice Agent 是單機（一台 RTX Spark）設計，SuperBrain Phase C 需要「講話對著哪台機器」的方向判斷（XVF3800 DoA），這是 Voice Agent 完全沒有的維度，需要 SuperBrain 自己設計「DoA → 路由到哪個 Spark」的邏輯，不屬於可重用範圍。
4. **輸出端**：Voice Agent 的執行結果是 Windows 桌面操作（`src/executor/`），SuperBrain 需要的是呼叫 `sb submit` 並用 TTS 播報 `sb status`/approval 內容——這是兩邊輸出語意完全不同的地方，需要 SuperBrain 自行接上。

### 完全需要自建

1. **XVF3800 硬體整合**（收音、AEC、DoA）——Voice Agent 無對應經驗。
2. **TTS**——兩邊都缺乏已驗證成果。

---

## 4. 整合風險

| 風險 | 說明 | 緩解方向（建議） |
|---|---|---|
| **機器身分不確定（R-02）** | Voice Agent 現在跑的「RTX Spark」是否為 SPARK-AGAVE-3/4 之一未經確認（見 [17](17_RISK_GAP_CONFLICT_REGISTER.md) R-02、[03](03_MACHINE_ARCHITECTURE.md)）。若剛好是同一台實體機器，代表 GPU/VRAM 資源會被 Voice Agent 的語音管線與 SuperBrain 的 FAST/DEEP 批次工作同時搶用 | 優先完成機器身分確認（[16](16_ROADMAP_AND_ACCEPTANCE.md) 建議順序第1項），再談整合細節 |
| **兩個專案完全獨立施工的既有原則（C-02 精神）** | Voice Agent 與 AERIS 的整合刻意選擇「不共用程式碼」；若 Voice Agent 與 SuperBrain 要重用同一套模組，需要決定是「共用程式碼庫」還是「各自複製一份再各自維護」——兩種選擇的長期維護成本不同 | 建議優先採「各自維護一份，定期對照升級」而非共用 repo，避免破壞兩專案現有的獨立施工節奏（呼應 Voice Agent 自己選擇的整合哲學） |
| **技術選型不一致（ASR/喚醒詞）** | whisper.cpp vs Breeze-ASR、既有喚醒詞 vs「小腦」，直接搬過去可能不符合 SuperBrain 自己設定的 KPI（中英混說、特定喚醒詞） | 用 SuperBrain 自己的 200 句語音測試集（`.ai/BLUEPRINT.md` 第296行）先跑一輪 baseline（用 Voice Agent 現成引擎），再決定是否要換 |
| **安全語意落差** | SuperBrain 的 RED 類語音規則目前只是文件敘述，沒有像 Voice Agent 一樣落地成程式碼與測試；若照搬 Voice Agent 的分級邏輯但沒有對應測試覆蓋，可能造成「文件說有安全機制，實際沒驗證」的落差（違反 house rule #4 的精神） | 若採用 Voice Agent 的分級模式，SuperBrain 也必須自己補上對應的測試（比照 Voice Agent 的驗證方法：真實音檔 + typed confirmation 測試），不能只複製文件敘述 |
| **P0 前置條件未滿足** | SuperBrain 目前連硬體盤點都沒做完，任何整合工作都無法在真實 Spark 環境驗證 | 本提案的可執行順序（見下）已把整合工作排在 P0-P1 之後 |

---

## 5. 建議的執行順序（僅供參考，非承諾時程）

1. **（前置）確認機器身分**：Voice Agent 現用的 RTX Spark 是否為 SPARK-AGAVE-3/4 之一（見 [16](16_ROADMAP_AND_ACCEPTANCE.md) 建議順序第1項）。這會決定整合是「同機資源共享」還是「跨機呼叫」問題。
2. **SuperBrain 完成 P0 硬體盤點**：這是 Phase C 動工的既有前提，與是否重用 Voice Agent 無關，本來就该先做。
3. **技術選型驗證（小規模）**：SuperBrain 用自己的 200 句測試集，先跑一輪「直接借用 Voice Agent 的 whisper.cpp + VAD + policy_engine 分級邏輯」的 baseline，量出目前 KPI 缺口有多大，再決定要不要換成 Breeze-ASR。
4. **喚醒詞重訓**：若 baseline 可接受，沿用 Voice Agent 既有訓練管線，用 SuperBrain 自訂喚醒詞「小腦」的語料重新訓練一個新模型（不動 Voice Agent 現有模型）。
5. **安全分級落地與測試**：把 Voice Agent 的 L0-L3/policy_engine 設計模式搬到 SuperBrain 端實作，並比照 Voice Agent 的方法補上真實音檔測試（尤其 RED 類 100% 準確率的驗收要求）。
6. **XVF3800 硬體整合與 DoA 路由邏輯**：這部分自建，與 Voice Agent 無關，可與上述步驟平行進行。
7. **TTS 選型與驗證**：兩邊都缺，建議兩專案協調由其中一方先做出來，另一方直接重用其驗證結果，而非各自重做。

---

## 6. 誠實揭露：本提案無法確認的部分

- 本輪未能在 Voice Agent repo 找到「中英混說」場景的分項測試數據，97.9% 中文辨識率的測試集組成細節本輪未逐行深讀，標記 `NOT FULLY VERIFIED`。
- Voice Agent 的 `progress/p1_wakeword_train/`、`progress/p1_vad_wakeython/` 資料夾內的訓練腳本與資料集細節，本輪只確認資料夾存在與檔名，未逐檔深讀，標記 `NOT FULLY VERIFIED`。
- 本提案完全基於兩邊 repo 目前的公開文件與程式碼結構，未涉及任何實機測試，所有「建議重用」的判斷都待兩專案實際嘗試後才能證實可行性。

## 相關文件

- [17_RISK_GAP_CONFLICT_REGISTER.md](17_RISK_GAP_CONFLICT_REGISTER.md) D-02、R-02
- [Audit/DUPLICATION_ANALYSIS.md](../Audit/DUPLICATION_ANALYSIS.md)（語音能力重疊原始分析）
- [03_MACHINE_ARCHITECTURE.md](03_MACHINE_ARCHITECTURE.md)（機器身分不確定性）
- [09_SUPERBRAIN_COMPUTE_FABRIC.md](09_SUPERBRAIN_COMPUTE_FABRIC.md)
