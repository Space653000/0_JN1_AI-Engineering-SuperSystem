# 05 — Source of Truth and Authority Matrix

| 專案 | Source of Truth（唯一真相文件） | 進度真相 | 驗收真相 | 治理權威 |
|---|---|---|---|---|
| AERIS Core | `docs/AERIS_BLUEPRINT_ZH_TW.md`（README 稱「產品入口」） | `HANDOFF.md`（要求每次即時查證，不信任快照） | `BLUEPRINT_BASELINE.md`（三輪發布檢查） | `constitution.md`（GATE-01～08） |
| AERIS Local Impl | `AGENTS.md` + `blueprint_compatibility.py`（以 Core 為 WHAT） | UNKNOWN（未見獨立 STATUS.md，本輪未讀到） | UNKNOWN | `aeris.local.policy.yaml`（存在，內容未核實） |
| AERIS Supervision | `SUPERVISION_CONTRACT.md`（12 條規則） | `LATEST.json`（指標，非證據） | 「Publication ≠ engineering PASS」（規則6） | 人類 Chief Engineer 保留最終 GO/NO-GO（規則8） |
| Voice Agent | `.ai/BLUEPRINT.md`（索引，指向 `docs/03`+`04`+`05` 為規格） | `.ai/STATUS.md`（明文「直接查證，不是憑印象」） | 各 Phase KPI（`.ai/BLUEPRINT.md` §3、§4） | `.ai/CLAUDE_REVIEWER.md` |
| MEGIS | `MEGIS_Blueprint/..._v3.0-claude-code.md`（唯一主要施工依據） | `execution/PROJECT_STATE.md` + README 進度表 | Gate acceptance（`*-ACC-*` 工作圖，需可重跑證據） | `CLAUDE.md` + Gate 審查流程 |
| AIECP | `.ai/BLUEPRINT.md`（應該做成什麼樣） | `.ai/STATUS.md`（現在做到哪，矛盾時優先更新） | `.ai/ACCEPTANCE.md`（怎樣算做完，五級證據鐵律） | `Blueprint/REQUIREMENTS.md` R1-R8 |
| SuperBrain | `.ai/BLUEPRINT.md`（明文「本專案唯一的藍圖依據」） | `.ai/STATUS.md` | `.ai/ACCEPTANCE.md`（T01-T30，PASS 需證據） | `.ai/CLAUDE_REVIEWER.md` |

## 跨專案衝突時的優先順序（本 repo 建議，非強制）

1. **各專案自己的 Source of Truth 對自己領域內的問題永遠優先**——本 SuperSystem repo 不會、也沒有權限裁決「AERIS 的聲學判斷」或「MEGIS 的幾何契約」孰是孰非。
2. **跨專案介面的定義權**：如果未來 AIECP 與 AERIS 要定義呼叫介面，這個介面契約本身應該有一個明確歸屬（建議歸屬 AIECP，因其扮演 Control Plane 角色），但介面「觸發的工程判斷結果」仍然歸 AERIS 權威。
3. **機器命名與角色**：目前唯一權威來源是 `0_JN1_2AGAVE128-1MAERA64` 的 `.ai/STATUS.md`；任何文件（包含本 repo）引用 ULTRA-MAERA-2/SPARK-AGAVE-3/4 時都應該以此為準。
4. **本 SuperSystem repo 自己不是任何專案的 Source of Truth**——它是索引與整合說明，衝突時永遠指回原始 repo 的權威文件，見 [CLAUDE.md](../CLAUDE.md)。
