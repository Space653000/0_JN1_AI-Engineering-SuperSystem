# 12 — Provider and Model Routing

完整清單見 [Registry/PROVIDERS.yaml](../Registry/PROVIDERS.yaml)。

## 目前現實（會變動，不是架構承諾）

- **Claude Code**：Planner/Reviewer adapter（AIECP）、架構審查/第二意見（SuperBrain）、主要施工代理（MEGIS）。
- **Codex CLI（ChatGPT Plus 內含）**：Builder（AIECP 的 codex-official worker；SuperBrain 的 `codex exec`）。
- **Codex CLI（PEGA endpoint）**：AIECP 的第二個隔離 Builder worker，接 `https://aiapi.t-cyber.com/v1`，ENVIRONMENT 證據仍待補。
- **Gemini（Antigravity CLI，免費 Starter 額度）**：SuperBrain 的 Researcher 角色，僅 best-effort，不得列入關鍵路徑（免費層節流嚴重，且個人 Google 登入的 Gemini CLI 已於 2026-06-18 停止）。
- **官方 ChatGPT Web**：AIECP 的主要對話介面（Web Safe Bridge）；SuperBrain Phase A 的語音前端。
- **本地模型**：Voice Agent 的 whisper.cpp/llama.cpp（已驗證運作）；SuperBrain 規劃中的 Spark 節點候選模型（Qwen 系列、GPT-OSS，尚未 benchmark）。

## 本地模型可以取代什麼（使用者問題19）

| 任務類型 | 本地模型現況 | 判斷依據 |
|---|---|---|
| 中文語音辨識（ASR） | ✅ 已證明可行，97.9% 準確率 | Voice Agent `.ai/STATUS.md` P1 |
| 意圖辨識/工具選擇 | ✅ 已證明可行，97.1% 準確率 | Voice Agent `.ai/STATUS.md` P2 |
| 一般問答/摘要/數據分析 | 預期可行（Spark 30-120B 模型），未實測 | SuperBrain `.ai/BLUEPRINT.md` §13 Q3 |
| 大型程式開發 Agent | ⚠️ 可用本地模型接 Codex/Claude Code 端點，但複雜修改成功率明顯低於雲端前沿模型 | 同上 |
| Windows 畫面操作（Computer Use） | ⚠️ 本地 VLM 可行但不穩定，優先改用 API/PowerShell/UIA | 同上 |
| 最新網路資訊 | ❌ 本質上需要連網 | 同上 |
| 視覺備援（UIA 找不到元素時） | ✅ 部分驗證（Qwen3-VL-8B，定位建議可用，完整操作閉環未完成） | Voice Agent `.ai/STATUS.md` P5 |

## SuperBrain 的分流門檻（本地優先規則）

在 golden set（100 題，涵蓋繁中摘要、中英混合指令、repo 分析、程式修補、除錯、工具呼叫、JSON、驗證、prompt injection）上，**本地分數 ÷ 雲端分數 ≥ 0.85** 的任務類別才可以預設走本地；閾值與分類皆待 P6 實測後才會有真實數據，目前仍是規劃階段，`config/routing.yaml` 的分類是初值猜測，非實測結果。

## 統一原則（跨所有專案適用）

不把任何供應商名稱寫進架構層的角色定義（見 [11](11_AI_AGENT_ROLE_ARCHITECTURE.md)）。Provider Mapping 是這份文件與 Registry 裡唯一允許出現具體廠牌名稱的地方，且應該隨現實變化持續更新，而不是被視為架構承諾。
