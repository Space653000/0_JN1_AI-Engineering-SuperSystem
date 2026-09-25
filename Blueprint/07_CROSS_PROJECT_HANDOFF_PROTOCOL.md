# 07 — Cross-Project Handoff Protocol

## 現況：唯一存在的跨專案 handoff

`ORDER.md` 檔案格式（Voice Agent ↔ AERIS）。這是本輪盤點唯一能指出具體機制名稱的跨專案 handoff；其詳細 schema 內容本輪未讀取（`docs/05_AERIS_Integration_Split.md` 只說明整合原則，未附完整格式定義），標記 `NOT VERIFIED — schema details`。

## AIECP 自己的 Handoff 協議（單一 repo 內，可作為跨專案設計的參考範本）

AIECP 的 `Blueprint/21_AGENT_ROLES_AND_HANDOFF_PROTOCOL.md` 定義了一套「Worker 身分與角色分離」的 handoff 模式：

```yaml
role: Builder
worker: codex-official
provider: openai-official
```

「Role handoff schemas must not encode PEGA/OpenAI-specific state. Worker handoff includes only canonical worker/provider/model/task/worktree/evidence identifiers.」

這個設計原則——**角色穩定、供應商/Worker 身分可替換、handoff 內容不得洩漏供應商特定細節**——值得作為未來 AIECP↔AERIS、AIECP↔MEGIS、AIECP↔SuperBrain 之間 handoff 協議的參考範本。

## 建議的跨專案 Handoff 骨架（本 repo 提案，未被任何來源 repo 採納）

```yaml
# 提案格式，非任何 repo 的既有實作
handoff:
  from_role: Harness            # AIECP
  to_domain: aeris | megis      # 目標領域權威
  task_id: <uuid>
  intent: <自然語言或結構化目標>
  evidence_refs: []             # 執行前的既有證據參照
  privacy_class: LOCAL_ONLY | SHAREABLE
  risk_level: GREEN | YELLOW | RED
  requires_verification_by: SPARK-AGAVE-4 | independent-reviewer | deterministic-verifier
```

此提案刻意仿照 AIECP 既有 Worker Runtime Record 的欄位精神（見 AIECP `Blueprint/00_MASTER_BLUEPRINT.md` §11A），但目前**沒有任何 repo 承諾採用**，僅供 Stephen 未來決策參考。

## Voice Agent → AIECP 的 handoff（目前完全不存在）

若要打通這條路徑，至少需要：
1. Voice Agent 端：把語音意圖轉成結構化 Command Card（類似 AIECP 現有的 `aecp.task/v1`）。
2. AIECP 端：接受非 ChatGPT-Web 來源的 Command Card 輸入（目前 AIECP 的 Safe Bridge 設計預設輸入來源是使用者從 ChatGPT Web 複製貼上，語音場景需要一個新的輸入 adapter）。

兩者都需要各自專案的維護者評估與實作，本 repo 只記錄需求，不代為設計實作細節。
