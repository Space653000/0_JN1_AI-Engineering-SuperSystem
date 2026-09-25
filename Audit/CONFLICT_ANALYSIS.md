# Conflict Analysis

每一條衝突都附上互相矛盾（或互相不知道對方存在）的具體來源。這份文件不裁決誰對誰錯，只記錄落差，留給 Stephen 本人或各專案自己的治理流程決定。

---

## C-01：使用者「confirmed architecture」與五個來源 repo 的實際內容之間存在系統性落差

**衝突內容**：本次任務的背景敘述（見任務指示的「Confirmed top-level architecture」）明確描述：
- ULTRA-MAERA-2 主控電腦同時承載 Voice Agent、AIECP、AERIS、MEGIS。
- SPARK-AGAVE-3 是 AIECP/SuperBrain 派工的 FAST worker。
- SPARK-AGAVE-4 是獨立驗證/審查的 DEEP node。
- SuperBrain = 三機整體。

**實際來源證據**：
- 對全部七個來源 repo 做過 `grep -ril "ULTRA-MAERA\|SPARK-AGAVE\|2AGAVE\|SuperBrain\|AIECP"`，結果：
  - `0_JN1_AERIS`、`0_JN1_AERIS_Local-computer-implementation`、`0_JN1_AERIS_Supervision`、`0_JN1_MEGIS`：**零命中**。
  - `Offline-Local-Voice-Agent`：**零命中**（只提到獨立跑在「RTX Spark」這個硬體型號，未具名為 SPARK-AGAVE-3/4）。
  - `0_JN1_AIECP`：**零命中**（AIECP 也未提到 AERIS/MEGIS/Voice Agent 或任何具名機器）。
  - `0_JN1_2AGAVE128-1MAERA64`：是唯一定義 ULTRA-MAERA-2/SPARK-AGAVE-3/SPARK-AGAVE-4 這三個名稱的 repo（`.ai/STATUS.md` 2026-09-25 決策紀錄），但其藍圖內文從未提到 AIECP、AERIS、MEGIS、Voice Agent 這幾個 repo 名稱。

**判定**：使用者描述的「confirmed architecture」是**尚待實現的目標藍圖**，目前沒有任何一個來源 repo 反映或實作這個整合。這不是「哪個 repo 過期了」的問題，而是這五～六個專案本來就是各自獨立演化，從未被要求互相整合。

**建議**：本 SuperSystem repo 的 Blueprint 應該把這個目標架構明確標示為「TARGET / TO-BE」，並在每個相關章節註明「目前 AS-IS 狀態為：尚未整合」，不要讓讀者誤以為這是已經運作的系統。

---

## C-02：Voice Agent 的實際運作模式 vs. 使用者描述的「Voice Agent → AIECP」資料流

**衝突內容**：使用者確認的核心流程是「Human → Voice Agent → AIECP → Mission/Task/Queue → Domain(AERIS/MEGIS) → …」。

**實際來源證據**：
- `Offline-Local-Voice-Agent/.ai/BLUEPRINT.md` 核心原則1：「完全離線——正式運作階段禁止任何雲端 API」；該專案的語音指令直接驅動本機 Windows 工具呼叫（Structured Tool Call → Policy Engine → Executor），**沒有把請求送到 AIECP 的路徑**。
- `Offline-Local-Voice-Agent/docs/05_AERIS_Integration_Split.md`：「兩專案完全獨立施工、不共用程式碼，只透過 `ORDER.md` 檔案格式互通」——唯一已知的跨專案整合對象是 AERIS，不是 AIECP。
- `0_JN1_AIECP` 全文對 Voice Agent 零提及；AIECP 的輸入來源被定義為「官方 ChatGPT Web 產生的 Command Card」，不是語音代理人。

**判定**：實際情況是 Voice Agent → AERIS（透過 `ORDER.md`），而不是 Voice Agent → AIECP。使用者設想的「Voice Agent 是 AIECP 的語音入口」目前完全沒有對應實作。

**建議**：Blueprint 07（Handoff Protocol）與 Blueprint 02（System-of-Systems Architecture）中明確畫出「AS-IS：Voice Agent → ORDER.md → AERIS」與「TO-BE：Voice Agent → AIECP」兩條線，不要合併成一條。

---

## C-03：AECP vs AIECP 命名不一致

**衝突內容**：GitHub repo 名稱是 `0_JN1_AIECP`，但 repo 內部所有文件（`README.md`、`.ai/*.md`、`Blueprint/00_MASTER_BLUEPRINT.md` 標題）都自稱 **"AECP"（AI Engineering Control Plane）**，完全沒有使用 "AIECP" 這個縮寫。

**來源**：`0_JN1_AIECP/README.md` 第1行「# AI Engineering Control Plane」；`0_JN1_AIECP/Blueprint/00_MASTER_BLUEPRINT.md` 第3行「**AI Engineering Control Plane (AECP)** is a Windows-first local engineering control plane」。

**判定**：這是純粹的命名不一致，不是功能衝突——但如果未來要在其他文件（包含使用者本人的敘述、以及這個 SuperSystem repo）統一引用這個專案，需要決定要用哪個縮寫。本次任務的敘述中使用者堅持「name is AIECP not AECP」，因此本 SuperSystem repo 統一採用 **AIECP** 稱呼這個系統，但引用其 repo 內文件時保留原文的 "AECP" 字樣以示忠實引用。

**建議**：這個決定權在 Stephen；如果決定統一命名，需要在 `0_JN1_AIECP` repo 自己的下一輪治理批次處理（本 SuperSystem repo 不會主動去改）。

---

## C-04：AIECP 與 SuperBrain 的 Control Plane 角色重疊

**衝突內容**：使用者的敘述中，AIECP 是「control plane（mission/task/workspace/queue/policy/git/ci/approval）」，SuperBrain 是「compute fabric（machines/workers/local models/execution/resource routing）」，兩者應該有明確分工，不能互相吸收。

**實際來源證據**：
- `0_JN1_AIECP/Blueprint/00_MASTER_BLUEPRINT.md`：AIECP 本身就包含完整的 Task/Queue/Scheduler/Worker/Provider Router/Evidence/Approval——這些恰好也是 compute fabric 的核心構件，不只是「control plane」的窄定義。
- `0_JN1_2AGAVE128-1MAERA64/.ai/BLUEPRINT.md`：SuperBrain 自己也有完整的 Router（`config/routing.yaml`）、Approval（三色風險分級 + exact-action digest）、Evidence（SQLite + JSONL audit）——這些同樣是 control plane 的構件，不只是「純執行 fabric」。

**判定**：兩個系統在目前的藍圖文字裡，事實上都各自宣稱了「control plane + compute fabric」的完整責任範圍，而不是使用者設想的乾淨二分。這是使用者的架構原則（AIECP=控制平面、SuperBrain=運算織理）目前尚未被任一 repo 自己的文件反映的落差，不是兩個 repo 之間的直接矛盾（因為它們根本不知道對方存在）。

**建議**：見 [Blueprint/08](../Blueprint/08_AIECP_ORCHESTRATION_ARCHITECTURE.md) 與 [Blueprint/09](../Blueprint/09_SUPERBRAIN_COMPUTE_FABRIC.md)——本 SuperSystem repo 提出的邊界建議是：AIECP 保留 Mission/Task/Workspace/Policy/Git/CI/Approval 的權威；SuperBrain 的 Spark 節點降級為 AIECP Provider Router 底下的「Local Provider / Worker」選項之一，而不是平行的另一套 Task 狀態機。這只是本 SuperSystem repo 的建議，不代表任一來源 repo 已採納。

---

## C-05：AERIS `v0.7.0-blueprint.1` 的「已凍結」與「未完成」是否矛盾

**衝突內容**：AERIS repo 建立了 `v0.7.0-blueprint.1` 這個 tag，用詞上容易被誤讀為「這個版本已經完成／已經定案可用」。

**實際來源證據**：
- `BLUEPRINT_BASELINE.md`：「定稿只固定這一版設計與驗收要求」「A–D 整體 NOT VERIFIED，E NOT_STARTED」——tag 本身明確只代表「設計文字被凍結」，不代表產品完成。
- `HANDOFF.md`：「本批僅治理整合，A–D 尚未完成，E 後續全面本機驗收 NOT_STARTED。CI 文件檢查不能證明產品完成。」

**判定**：這不是兩個文件互相矛盾，而是 tag 名稱本身容易被外部讀者誤解。repo 自己的文件已經非常清楚地自我澄清了這一點。

**建議**：本 SuperSystem repo 在任何提及 `v0.7.0-blueprint.1` 的地方，一律同時註明「這是設計凍結 tag，不是產品完成證明」，避免以訛傳訛。

---

## C-06：私有 repo 存取限制

**衝突內容**：任務指示假設 `0_JN1_AERIS_Supervision` 可能因為是私有 repo 而無法存取。

**實際情況**：本次 session 具備存取權限，成功 clone 並讀取。內容以自動化腳本與一份簡短的 `SUPERVISION_CONTRACT.md` 為主，工程實質內容有限（未见長篇 Blueprint 文件）。

**判定**：不是存取衝突，而是內容本身簡短。已在 [REPOSITORY_INVENTORY.md](REPOSITORY_INVENTORY.md) 中如實記錄能讀到的內容，未逐行審閱 `automation/` 內每支 PowerShell 腳本的實作細節（標記 `NOT VERIFIED`）。
