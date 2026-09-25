# 04 — Project Responsibility Matrix

「擁有」代表該專案是這項職責的權威來源；「無」代表本輪盤點未發現任何實作；`UNKNOWN` 代表可能存在但未核實。

| 職責 | AERIS Core | AERIS Impl | AERIS Supervision | Voice Agent | MEGIS | AIECP | SuperBrain |
|---|---|---|---|---|---|---|---|
| 聲學工程判斷 | ✅ 擁有 | ✅ 擁有（執行） | 無 | 無 | 無 | 無 | 無 |
| 機構工程判斷 | 無 | 無 | 無 | 無 | ✅ 擁有 | 無 | 無 |
| 語音輸入/HMI | 無 | 無 | 無 | ✅ 擁有（已驗證） | 無 | 無（Agent Switcher 非語音） | 規劃中（Phase A-C，未實作） |
| Windows 桌面自動化 | 無 | 無 | 無 | ✅ 擁有 | 無 | Windows UI Automation（唯讀，scoped） | 無 |
| Git/GitHub 交付 | 自己的 tag 保護規則 | UNKNOWN | 唯讀發布腳本，禁止修改 Blueprint/Impl | UNKNOWN | 自己的 baseline CI | ✅ 最成熟：governed delivery、exact-HEAD CI、簽章 webhook | 規劃中 |
| CI | 自己的（governance 檢查為主） | UNKNOWN | 無 | UNKNOWN | ✅ `run-baseline-ci.ps1` | ✅ 完整 GitHub Actions 矩陣 | 無 |
| 任務佇列/排程 | 無 | 無 | 無 | 內部 PlanRunner（單機多步驟） | Gate 工作圖佇列（非執行期） | ✅ 持久 Mission/Task/Scheduler | 規劃中（SQLite） |
| Provider Router | 無 | 無 | 無 | 無 | 無 | ✅ 角色↔供應商解耦 | 規劃中（config/routing.yaml） |
| 證據/Evidence | `traceability.json`/`review.json` | `audit.py`/`claim_guard.py` | SHA-256 bundle 驗證 | `progress/p*/REPORT.md` + JSON | artifact classification + fingerprint | ✅ 五級證據分級 | 規劃中（SQLite+JSONL） |
| 人類核准 Gate | constitution GATE-01~08 | UNKNOWN | 人工觸發 Publisher | L0-L3 + 語音文字雙確認 | Gate acceptance 簽核 | GREEN/YELLOW/RED + RED 人工核准 | 規劃中（exact-action digest） |
| 發布/快照 | 自己的 tag 機制 | 無 | ✅ 擁有（唯一職責） | 無 | 無 | 自己的 Release/SHA256SUMS | 無 |
| 多機硬體資源分配 | 無 | 無 | 無 | 無 | 無 | 無 | ✅ 擁有（規劃中） |
| 供應商帳號/憑證管理 | UNKNOWN | UNKNOWN | 無 | 無 | 無 | ✅ OS-backed safeStorage | 規劃：PowerShell SecretStore |

## 結論

每個專案在自己的核心職責上都是清楚的權威（AERIS=聲學、MEGIS=機構、Voice Agent=語音+桌面、AIECP=通用 Git 控制平面、SuperBrain=多機資源、AERIS Supervision=AERIS 發布）。**問題不在於職責不清，而在於這些職責之間完全沒有互相呼叫的介面**——每個專案都是自己領域裡的孤島式權威。這正是本 SuperSystem repo 存在的核心理由：不是要重新分配職責，而是要把現有職責之間的橋樑（目前是 `MISSING` 的介面）畫出來，見 [Registry/INTERFACES.yaml](../Registry/INTERFACES.yaml)。
