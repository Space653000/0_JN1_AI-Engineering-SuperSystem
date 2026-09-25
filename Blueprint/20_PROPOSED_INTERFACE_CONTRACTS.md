# 20 — Proposed Interface Contracts（G-01 / G-02 / G-03，建議草案）

> **定位聲明**：本文件全部內容是**本 SuperSystem repo 對 Stephen 與各來源專案的建議草案（DRAFT / 建議，非已採用）**。三個介面（G-01、G-02、G-03）目前在對應來源 repo 中**完全不存在**（見 [17_RISK_GAP_CONFLICT_REGISTER.md](17_RISK_GAP_CONFLICT_REGISTER.md)、[Registry/INTERFACES.yaml](../Registry/INTERFACES.yaml)）。本文件不代表 Voice Agent、AIECP、AERIS、MEGIS、SuperBrain 任何一方已經同意或已經實作這裡描述的形狀；是否採用、何時採用、採用哪個版本，完全由各專案自己的治理流程與 Stephen 本人決定（house rule #5）。
>
> **方法論**：每個草案都刻意「站在既有的肩膀上」——優先沿用來源 repo 已經驗證過的既有格式/機制作為草案基礎，而不是發明一套全新協定。每一條引用都附「repo 名稱 + 檔案路徑」。找不到依據的地方一律標 `NEEDS DESIGN INPUT FROM OWNER`，不臆測。

---

## G-01：Voice Agent ↔ AIECP

### 現況缺口

Registry/INTERFACES.yaml 的 `voice-agent-to-aiecp` 項目：`contract_status: MISSING`。Audit/CONFLICT_ANALYSIS.md C-02 已指出，Voice Agent 實際整合對象是 AERIS（透過 `ORDER.md`），不是 AIECP；兩個 repo 全文互相 grep 零命中，彼此不知道對方存在。

### 可沿用的既有precedent（雙邊都已存在，只是彼此不知道對方）

1. **Voice Agent 側已有的「檔案交接、不共用程式碼」模式**：Voice Agent 與 AERIS 的整合刻意選擇「兩個專案完全獨立施工、不共用程式碼，只透過 `ORDER.md` 檔案格式互通」，交接資料夾是一個「雙方都認得的本機路徑」（例：`C:\0_JN1_AERIS_HANDOFF\orders\`）——來源：`Offline-Local-Voice-Agent/docs/05_AERIS_Integration_Split.md` 第4、24、46-54行。這個「檔案掉落 + 監看資料夾」模式已經被驗證是可行的整合方式（AERIS 側寫 Order Validator 讀取）。
2. **Voice Agent 側已有的風險分級**：`src/policy/risk_levels.py`（L0 唯讀／L1 一般可逆／L2 敏感需二次確認／L3 危險需逐次明確確認，且不可記住「這次都同意」）——來源：`Offline-Local-Voice-Agent/src/policy/risk_levels.py` 第5-8、23-26行；`src/policy/policy_engine.py` 第16、25、38-45行。
3. **Voice Agent 側已有的意圖粗分類**：Front Desk 模式的判斷方式是「先用現有 LLM 做一個粗分類（一般桌面操作 vs 工程問題）」，再決定要不要切換成引導流程——來源：同上 `docs/05_AERIS_Integration_Split.md` 第25行。
4. **AIECP 側已有的 provider-agnostic 交接格式 `aecp.task/v1`**：Command Card（JSON，含 `schema`/`title`/`workspace`/`goal`/`action`/`permissions`/`verification`）、Context Capsule（`aecp.context/v1`）、Result Capsule（`aecp.result/v1`）、Execution Trace（`aecp.trace/v1`），以及剪貼簿框架標記 `AECP_COMMAND_CARD_V1` / `AECP_RESULT_CAPSULE_V1`——來源：`0_JN1_AIECP/Blueprint/11_TASK_PROTOCOL.md` 全文（schema 範例見第14-40、63-78、80-101、105-117、121-129行）。
5. **AIECP 側目前的能力邊界**：v0.1.0 只支援 `inspect-workspace`／`git-status` 兩種**唯讀**動作，明文「There is intentionally no arbitrary `shell` action in v1 preview」；`write-file`/`apply-patch`/`git-commit` 等異動能力屬於「Future capability evolution」，需要明確的 Policy Engine 規則與核准閘門才能加入——來源：同上第16-21、42-61行。

### 建議的契約形狀（DRAFT）

**觸發條件**：Voice Agent 的 Front Desk 粗分類器（見上 precedent 3）新增第三種分類結果：「一般工程/Git/倉庫查詢類」（區別於現有的「一般桌面操作」與「AERIS 聲學工程問題」兩類）。當分類結果為此類、且對話狀態機（S0–S8，來源同上文件第22行）走到收斂完成（S8 對應狀態）時，觸發本介面。

**交接機制（沿用 precedent 1 的檔案掉落模式，但 payload 改用 precedent 4 的 schema）**：

- 交接資料夾（建議命名，比照 ORDER.md 案例）：`C:\0_JN1_AIECP_HANDOFF\voice-tasks\inbox\<task-uuid>.json`
- 檔案內容 = 一份符合 `aecp.task/v1` 的 Command Card，額外加一個非破壞性的 metadata 欄位 `origin`：

```json
{
  "schema": "aecp.task/v1",
  "origin": "voice-agent",
  "title": "<Voice Agent 對話收斂出的任務標題>",
  "workspace": "<AIECP 端已知的 workspace 名稱，由 Voice Agent 端維護一份對照表>",
  "goal": "<收斂後的自然語言目標，單句>",
  "action": {
    "type": "inspect-workspace"
  },
  "permissions": ["workspace:read"],
  "verification": {
    "type": "operation-success",
    "expected": true
  },
  "voiceMeta": {
    "riskLevel": "L0",
    "confirmedByUser": true,
    "confirmationTranscript": "<使用者口頭確認的逐字稿，供事後稽核>",
    "sessionId": "<Voice Agent 對話 session id>"
  }
}
```

- **AIECP 端**（建議，非本 repo 施工範圍）：新增一個 handoff watcher，比照 ChatGPT Web 的 Command Card import 流程驗證 schema、拒絕未知版本，把 `voiceMeta` 當作不可信 metadata（不得用它繞過既有 permission 檢查）。
- **回傳**：AIECP 完成後在 `C:\0_JN1_AIECP_HANDOFF\voice-tasks\outbox\<task-uuid>.json` 寫入一份 `aecp.result/v1` Result Capsule；Voice Agent 監看 outbox，讀到後用既有 TTS 把 `summary` 欄位唸給使用者，比照 Voice Agent 現有「AIECP 回傳 WAITING_APPROVAL 時要把 approval 內容原文唸給使用者」這類語音播報慣例（此慣例目前見於 SuperBrain 的 `sb` CLI 設計，`0_JN1_2AGAVE128-1MAERA64/.ai/BLUEPRINT.md` 第217-225行，可作為「語音層如何播報審批狀態」的參考模式，非 AIECP 自己現有機制）。

**風險分級對應（建議映射，未經任一方確認）**：

| Voice Agent 風險等級 | 建議對應 AIECP 動作類型 | 現況是否可行 |
|---|---|---|
| L0 唯讀查詢 | `inspect-workspace` / `git-status` | ✅ AIECP v0.1.0 已支援唯讀動作 |
| L1 一般可逆 | 尚無對應 action schema | ❌ 需 AIECP 新增結構化 write action（`write-file` 等），且需 YELLOW 等級 Policy 規則 |
| L2/L3 敏感／危險 | 尚無對應 action schema，且需人工核准 | ❌ 需 AIECP 的 RED 等級 approval gate 落地後才可能，見 G-01 的錯誤處理章節 |

**錯誤處理**：
- Command Card schema 版本不符 → AIECP 端按既有規則直接拒絕，不建立 Task（沿用 `0_JN1_AIECP/Blueprint/11_TASK_PROTOCOL.md` 第43行「unknown schema version is rejected」）。
- Voice Agent 分類錯誤（誤判為 AIECP 任務，實為 AERIS 聲學問題）→ 建議 AIECP 端對 `workspace` 欄位做白名單檢查，非白名單 workspace 一律拒絕並回寫 outbox 一則 `status: REJECTED` 的 Result Capsule，附 `nextDecision` 建議使用者改走 AERIS 流程。
- outbox 逾時無回應 → Voice Agent 端建議加一個等待逾時（例如 30 秒），逾時後用既有 L2 語音提示告知使用者「AIECP 沒有回應，請確認服務是否啟動」，不可靜默假裝成功。

**證據/紀錄期待**：Voice Agent 端把 `confirmationTranscript` 與 outbox 收到的 `evidenceRef` 一併寫入自己既有的 Memory/Logging 資料表（`.ai/STATUS.md` 第28行提到「Memory/Logging資料表都已實作並真實驗證」），形成雙邊都能各自稽核的紀錄，而不是只信任對方回傳的文字。

---

## G-02：AIECP ↔ AERIS／AIECP ↔ MEGIS

### 現況缺口

Registry/INTERFACES.yaml 的 `aiecp-to-aeris`、`aiecp-to-megis` 兩項皆 `contract_status: MISSING`。AIECP 的 `aecp.task/v1` 目前只有兩種通用唯讀動作（`inspect-workspace`/`git-status`），沒有聲學或機構工程專屬欄位或路由；MEGIS 明文禁止與其他專案共用環境。

### 可沿用的既有 precedent

1. **AIECP 側**：`aecp.task/v1` 系列 schema（同 G-01 引用），以及 Blueprint/03 §8 的角色路由模型 `roles: {supervisor, planner, builder, reviewer, verifier, local_compute}`，「Any role may be remapped without changing Task state」——來源：`0_JN1_AIECP/Blueprint/03_PROVIDER_ROUTER.md` 第146-160行。AIECP 自己的五級證據制度 STATIC/TESTED/CI/ENVIRONMENT/OWNER-EXTERNAL——來源：`0_JN1_AIECP/.ai/ACCEPTANCE.md` 第13-19行。
2. **AERIS 側**：Front Desk 交接後，AERIS 端要做的事包括「Order Validator（schema/completeness 檢查）」「100 個 Capability Contract YAML」「Results.xlsx + Report.pptx 固定交付格式」「G0–G10 驗收 Gate 機制，不可只憑文件宣稱完成度」——來源：`Offline-Local-Voice-Agent/docs/05_AERIS_Integration_Split.md` 第38-42行（這是 Voice Agent repo 對 AERIS 該做的事的規劃性引用，AERIS 自己 repo 內尚未逐一驗證這些機制是否已落地，屬 `NOT VERIFIED`）；AERIS 自己的 Gate 體系見 `0_JN1_AERIS/docs/governance/ASTRA_EXECUTION_GATE_V3.md`（GATE-01～08）。
3. **MEGIS 側**：Gate-driven 工作項目 + 驗證腳本模式：每個工作項目（例如 `G4-MOD-001`）都對應一組 schema（`schemas/v3/*.schema.json`）、golden corpus（`contracts/g*/golden/*.json`）、測試與 `verify_g*_*.py` 腳本、以及寫入 `artifacts/<gate>-<item>/verification.json` 的機器可讀證據——來源：`0_JN1_MEGIS/execution/PROJECT_STATE.md` 第9-33行（G4-MOD-001/G4-GRF-001 已閉合段落）。MEGIS 也有自己的任務佇列檔案 `execution/WORK_QUEUE.yaml`（來源：`0_JN1_MEGIS/execution/WORK_QUEUE.yaml` 檔案存在，內容本輪未逐行深讀，標記 `NOT FULLY VERIFIED`）。MEGIS 的硬邊界：「`C:\0_JN1_AERIS` 與 `C:\0_JN1_Offline-Local-Voice-Agent` 不得變更、共用或依賴」——來源：`0_JN1_MEGIS/execution/PROJECT_STATE.md` 第80行「Boundaries」章節。

### 建議的契約形狀（DRAFT）

**共同原則（建議）**：因為 MEGIS 明文禁止與其他 repo 共用環境或依賴，且 AERIS/Voice Agent 的整合模式本身就選擇「不共用程式碼、只共用檔案格式」，本草案對 AERIS 與 MEGIS 都採用**同一種一致的原則：AIECP 只透過檔案落地的 Command Card / Result Capsule 與對方交握，絕不直接呼叫對方的內部函式或共用 runtime**。

**觸發條件**：AIECP 的 Planner 角色（Blueprint/03 §8 的 `planner`）判斷一個 Task 屬於「聲學工程領域」或「機構工程領域」時，不再走自己的通用 Worker，而是把 Task 序列化成一份「領域派工 Command Card」，寫入對應 handoff 資料夾。

**AIECP → AERIS（建議 schema，擴充自 `aecp.task/v1`）**：

```json
{
  "schema": "aecp.task/v1",
  "action": {
    "type": "domain-dispatch",
    "domain": "aeris-acoustic",
    "capabilityRef": "<對照 AERIS 100 Capability Contract 中的編號，例如 003/018/021...>"
  },
  "goal": "<派工目標>",
  "permissions": ["domain:read"],
  "verification": {
    "type": "gate-acceptance",
    "expectedGate": "G0-G10 之一"
  }
}
```

- **落地路徑（沿用 ORDER.md 交接資料夾的既有慣例）**：`C:\0_JN1_AERIS_HANDOFF\orders\<task-uuid>.md`（建議直接沿用 AERIS 既有的 ORDER.md 格式作為 payload body，`aecp.task/v1` 的 JSON 只作為信封 metadata 附掛在同目錄的 `<task-uuid>.json`，避免要求 AERIS 重新設計自己的 Order Validator）。
- **回傳**：AERIS 端 Order Validator 驗證通過、跑完既有 Gate 流程後，把 `Results.xlsx`/`Report.pptx`（見上 precedent 2）的路徑寫進 AIECP 的 `aecp.result/v1`：

```json
{
  "schema": "aecp.result/v1",
  "status": "PASS",
  "evidenceRef": "local://aeris-handoff/results/<task-uuid>/Results.xlsx",
  "verification": {
    "status": "PASS",
    "method": "aeris-gate-acceptance",
    "expected": "gate-closed",
    "actual": "G<n>-ACC-001 closed"
  }
}
```

- **證據等級對應（建議，需 AIECP 與 AERIS 雙方確認）**：AERIS 的 Gate 驗收（人工簽核）對應 AIECP 的 `OWNER-EXTERNAL` 等級（因為需要領域專家/Stephen 本人核准，非 CI 可自動判定）；AERIS 的 Order Validator schema 檢查對應 AIECP 的 `STATIC` 等級。這只是概念對照，兩邊詞彙不強制統一（呼應 Audit/DUPLICATION_ANALYSIS.md D-03「合理重複，詞彙不統一」的既有結論）。

**AIECP → MEGIS（建議 schema）**：

```json
{
  "schema": "aecp.task/v1",
  "action": {
    "type": "domain-dispatch",
    "domain": "megis-mechanical",
    "gateRef": "<MEGIS 現有 Gate 代號，例如 G4-MOD-002>"
  },
  "goal": "<派工目標>",
  "permissions": ["domain:read"]
}
```

- **落地路徑**：因 MEGIS 明文禁止共用環境，建議比照 MEGIS 自己既有的 `execution/WORK_QUEUE.yaml` 佇列模式，但**不是**讓 AIECP 直接寫入該檔案（那會違反 MEGIS 的自治邊界），而是新增一個 MEGIS 自己擁有的、獨立於主 execution 目錄的「外部請求收件匣」（例如 `C:\0_JN1_MEGIS\external_requests\aiecp\<task-uuid>.json`），由 MEGIS 自己的流程決定要不要、何時把它轉成內部 WORK_QUEUE 項目——這個「是否接受外部請求」的決策權完全保留給 MEGIS 專案自己（house rule #5，本 repo 不代 MEGIS 做這個決定）。
- **回傳**：MEGIS 對應工作項目關閉時產生的 `artifacts/<gate>-<item>/verification.json`（precedent 3）直接作為 `evidenceRef` 回填進 AIECP 的 Result Capsule，不需要 MEGIS 重新產生一份給 AIECP 專用的證據格式。

**錯誤處理（兩者共通）**：
- 對方 handoff 資料夾不存在或無回應 → AIECP Task 保持 `PENDING`，比照自己現有的「有界排程器」邏輯設定重試/逾時（`0_JN1_AIECP/.ai/STATUS.md` 提及的排程機制，細節本輪未逐行深讀，標記 `NEEDS DESIGN INPUT FROM OWNER`）。
- 領域端 Gate 驗收失敗 → Result Capsule `status: FAIL`，`nextDecision` 欄位建議填入「需要人工判斷是否重派工或改變任務範圍」，不可自動重試寫入行為（避免在未經審查的情況下對聲學/機構工程資料重複操作）。

---

## G-03：AIECP ↔ SuperBrain

見 [19_MASTER_PROGRESS_TRACKER.md](19_MASTER_PROGRESS_TRACKER.md) 與 [18_DECISION_LOG.md](18_DECISION_LOG.md)：2026-09-25 Stephen 已裁決 Queue/Router/Worker/Evidence/Approval 這組控制平面原語歸 AIECP 所有，SuperBrain 提升為跨機資源調度/統籌規劃層，**不得**另建同性質的 Queue/Router/Worker。本草案在此裁決範圍內設計。

### 可沿用的既有 precedent

1. **AIECP 側**：Provider/Worker 分離模型——「Provider = intelligence/API/model source and its capability, health, credential and usage contract」「Worker = a concrete isolated execution process」，五狀態健康模型 `NOT_CONFIGURED/READY/DEGRADED/UNAVAILABLE/AUTH_REQUIRED`，`ProviderAdapter` 介面（`id`/`kind`/`capabilities()`/`health()`/`invoke?()`/`cancel?()`）——來源：`0_JN1_AIECP/Blueprint/03_PROVIDER_ROUTER.md` 第35-56、76-85、120-129行。既有 `local` provider 類別，用途是「bounded, high-volume preprocessing」——來源同上第28-33行。
2. **SuperBrain 側**：`sb` CLI 是「語音與 Agent 對 SuperBrain 的唯一入口」，指令包含 `sb submit`/`sb status`/`sb workers`/`sb approve`/`sb cancel`，其中 `sb workers` 回傳「Laptop / Spark1 / Spark2 健康狀態」，`sb approve` 只接受 exact-action digest——來源：`0_JN1_2AGAVE128-1MAERA64/.ai/BLUEPRINT.md` 第204-224行。SuperBrain 的靜態分流表 `config/routing.yaml`（`threshold: 0.85`，依任務類別分流到 spark1/spark2/工具/雲端）——來源同上第235-258行。

### 建議的契約形狀（DRAFT）

**核心設計原則（呼應 Stephen 的裁決）**：SuperBrain **不**作為 AIECP 的對等控制平面，而是作為 AIECP Provider Router 底下的**一種新的 `local` 類 Provider**（precedent 1 的既有分類），名稱建議為 `superbrain-fabric`。AIECP 仍然是唯一的 Queue/Task/Approval 權威；SuperBrain 只回答「這個計算任務要不要接、現在誰有空、跑完了沒有」，不自己維護一套平行的任務狀態機。

**Provider Adapter 對照（建議映射）**：

```yaml
id: superbrain-fabric
kind: local
capabilities:
  - batch-inference
  - long-context-review
  - embedding
health: |
  透過呼叫 SuperBrain 既有的 `sb workers`（precedent 2）取得 Laptop/Spark1/Spark2 健康狀態，
  映射到 AIECP 的五狀態模型：
    - sb workers 回報 Spark ONLINE 且無 lease 佔滿 → READY
    - Spark 忙碌但可排隊 → DEGRADED
    - sb workers 回報 OFFLINE（心跳逾時，SuperBrain 自訂 30 秒判定） → UNAVAILABLE
    - sb 尚未設定/未啟動 → NOT_CONFIGURED
    - SuperBrain 端憑證/連線問題 → AUTH_REQUIRED
invoke: |
  AIECP 呼叫等同於執行 `sb submit "<結構化任務描述>" --local-only`（precedent 2 第207行 CLI 語法），
  取代讓使用者自己在語音層呼叫 sb；AIECP 的 Task 狀態機仍然是唯一真相來源，
  `sb submit` 回傳的 task_id 只是 AIECP Task 底下的一個外部 correlation id。
cancel: |
  對應 `sb cancel <task_id>`（precedent 2 第212行）。
```

**觸發條件**：AIECP Router 依 Blueprint/03 §8 的角色路由表，判斷某個 `worker` 或 `local_compute` 角色適合路由到 `superbrain-fabric`（例如高 token 批次分析、長上下文審查——這類任務類別與 SuperBrain 既有的 `config/routing.yaml` 分類，例如 `test_data_analysis`/`repo_analysis`，性質相近，見 precedent 2 第246-248行），且該任務不涉及本地 Git 寫入或 GitHub 交付（這部分留在 AIECP 自己的 Worker，不下放給 SuperBrain，維持 Stephen 裁決的分工邊界）。

**錯誤處理**：
- `health()` 回報 `UNAVAILABLE`／`DEGRADED` → AIECP 依既有規則「Failure of an optional provider must not prevent Safe Bridge use」（precedent 1 第129行同一份文件第128行附近的 6. Provider health 章節精神），改用其他 Provider（例如既有的 codex-cli Worker），不得讓 SuperBrain 缺席阻塞 AIECP 主流程。
- `sb submit` 回傳 `WAITING_APPROVAL`（SuperBrain 既有的核准語意，precedent 2 第224行）→ AIECP 不得替使用者自動核准；建議把這個等待狀態原樣映射進 AIECP 自己的 RED 等級 Approval 佇列，讓 Stephen 只需要在**一個**核准介面（AIECP Harness）做決定，而不是要分別去 AIECP Dashboard 和 SuperBrain Dashboard 各按一次（此建議亦回應 [17](17_RISK_GAP_CONFLICT_REGISTER.md) 的 G-07「跨專案統一核准佇列不存在」）。

**證據/紀錄期待**：SuperBrain 端既有的 SQLite state + JSONL audit（`0_JN1_2AGAVE128-1MAERA64/.ai/BLUEPRINT.md` 第195行）作為 `ENVIRONMENT` 等級證據來源，由 AIECP 的 Result Capsule 引用其 `evidenceRef`（例如 SuperBrain 的 audit JSONL 該筆記錄的雜湊/路徑），而不是要求 SuperBrain 重新輸出一份 AIECP 專用格式。

**與現況的落差（誠實揭露）**：SuperBrain 目前仍在 P0（硬體盤點未完成，見 [19](19_MASTER_PROGRESS_TRACKER.md)），`sb workers`/`sb submit` 等 CLI 本身尚未有真實跑通證據；本草案描述的是「P0-P2 完成之後」才具備條件實作的介面，**現階段連前置條件都不成立**，屬於中長期建議。

---

## 後續動作（建議，非承諾）

- 三份草案的落地順序，建議見 [16_ROADMAP_AND_ACCEPTANCE.md](16_ROADMAP_AND_ACCEPTANCE.md) 的建議優先順序（先 G-02 的最小場景，再 G-01，G-03 排最後，因為前置條件 SuperBrain P0-P2 尚未完成）。
- 任何一方若要採用本草案，建議先在自己的 repo 內以 `NOT VERIFIED` 狀態的設計文件形式落地，再回頭更新本 repo 的 [Registry/INTERFACES.yaml](../Registry/INTERFACES.yaml) 把 `contract_status` 從 `MISSING` 改為 `PARTIAL`，最後才是 `EXISTS`——本 repo 不會自己把狀態改成 `EXISTS`，因為那需要來源 repo 自己的證據（house rule #4）。
