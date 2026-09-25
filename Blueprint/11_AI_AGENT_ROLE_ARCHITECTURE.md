# 11 — AI Agent Role Architecture

## 抽象角色（穩定，不綁定供應商）

沿用 AIECP `Blueprint/21_AGENT_ROLES_AND_HANDOFF_PROTOCOL.md` 的九角色模型（本次盤點中唯一明確做「角色 vs 供應商」解耦設計的來源），完整登錄見 [Registry/AGENTS.yaml](../Registry/AGENTS.yaml)：

Supervisor、Planner、Researcher、Builder、Reviewer、Verifier、Maintainer、Local Compute、Harness。

## Provider Mapping（目前現實，會變動）

| 角色 | 目前 Provider |
|---|---|
| Supervisor | Human (Stephen) |
| Planner | Claude Code |
| Researcher | Gemini (Antigravity CLI, best-effort)、Claude Code |
| Builder | Codex CLI（OFFICIAL / PEGA） |
| Reviewer | Claude Code |
| Verifier | 確定性測試（npm test/pytest/verifier scripts）；未來 SPARK-AGAVE-4（未實作） |
| Maintainer | Codex CLI |
| Local Compute | Ollama、LM Studio、llama.cpp（規劃中）、whisper.cpp（Voice Agent，已運作） |
| Harness | AIECP Harness（已實作）；SuperBrain `sb` CLI（規劃中） |

**原則**：架構不寫死任何供應商名稱；未來 Gemini API、其他模型供應商都可以替換進 Provider Mapping 而不需要改變角色定義本身。

## 如何避免 Builder 審查自己（使用者問題24）

這是 AIECP 已經在自己 repo 內解決的問題，原則可以直接套用到整個 SuperSystem：

1. **角色分離是結構性的，不是自覺性的**：Task 狀態機本身不允許同一個 Worker 身分同時持有 Builder 與 Reviewer 的權限。
2. **Worker 身分與 Provider 身分分開追蹤**：即使 Codex OFFICIAL 與 Codex PEGA 用的是同一家供應商的模型，它們的 Worker 身分（含隔離的 `CODEX_HOME`）仍然是分開的，避免「同一個執行環境既做事又審查自己」。
3. **完成判定權不屬於任何 AI 角色**：「Neither OFFICIAL nor PEGA may self-declare engineering completion」——完成必須經過 Verifier（確定性測試）+ Reviewer（獨立審查）兩關。

## SuperBrain 的 FAST/DEEP 分工如何落實這個原則（使用者問題23，目前缺口）

理論上 SPARK-AGAVE-4（DEEP）應該扮演「獨立驗證 SPARK-AGAVE-3（FAST）」的角色，但目前 SuperBrain 藍圖沒有具體定義：

- SPARK-AGAVE-3 的輸出要以什麼格式送到 SPARK-AGAVE-4？
- SPARK-AGAVE-4 的驗證是「重跑一次同樣的任務用更大模型比對結果」，還是「檢查 FAST 節點輸出是否符合某種確定性規則」？
- 如果 SPARK-AGAVE-4 判定 FAIL，流程如何回到 SPARK-AGAVE-3 或升級給人類？

**本 repo 提出的最小可行協議草案**（未被任何來源 repo 採納，僅供參考）：

```yaml
verification_request:
  from: SPARK-AGAVE-3
  to: SPARK-AGAVE-4
  task_id: <uuid>
  fast_output: <路徑或內容>
  fast_confidence: <FAST 節點自評信心值，僅供參考，不可作為 PASS 依據>
  verification_method: rerun_with_larger_model | deterministic_rule_check | cross_reference
verification_result:
  from: SPARK-AGAVE-4
  verdict: PASS | FAIL | NEEDS_HUMAN
  evidence_refs: []
```

這只是一個起點草案，實際協議需要 SuperBrain 專案自己根據真實工作負載設計與驗證。
