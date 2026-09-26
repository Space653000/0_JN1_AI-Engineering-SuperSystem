# 28 — 給各專案 Claude Code 的導入評估提示詞

> 2026-09-27。Stephen 要求把 [Blueprint/27](27_FRONTIER_RADAR_MASTER_SYNTHESIS.md) 的🔴精選項目（扣除 Stephen 決定暫緩的 AIECP Trusted Signing）拆成各自獨立的提示詞，分別給指向對應本機資料夾＋對應 GitHub repo 的 Claude Code session 去做「能不能導入、怎麼導入」的評估。
>
> **重要**：這些提示詞是要貼到**另一個、有權限修改對應來源 repo 的 Claude Code session**（不是這個 SuperSystem repo），本 SuperSystem repo 自己不會、也不能執行這些改動。每份提示詞已內建「先讀那個 repo 自己的 CLAUDE.md/AGENTS.md，遵守它自己的規則」這個提醒，避免跳過該專案自己的治理流程。
>
> 排除項目：**AIECP Trusted Signing 申請**——Stephen 已決定暫緩（個人開發考量），未列入以下提示詞，保留在 [Blueprint/27](27_FRONTIER_RADAR_MASTER_SYNTHESIS.md) 精華清單第2條備查。

---

## 1｜AERIS 能力地圖自查

**目標資料夾**：`C:\0_JN1_AERIS`（本機）
**目標 GitHub repo**：`Space653000/0_JN1_AERIS`（必要時也讀 `Space653000/0_JN1_AERIS_Local-computer-implementation`）

```
請先讀這個 repo 自己的 CLAUDE.md / AGENTS.md / constitution.md，遵守它既有的治理規則（尤其 GATE-01~08）。

任務：對 AERIS 現有的「100 席位聲學工程能力清單」做一次自查，找出跟業界完整能力地圖相比的缺口。

背景：AERIS 的願景是「一位人類主管＋100個聲學專業能力席位＝一人抵百人AI聲學公司」，聲學範疇明確涵蓋揚聲器與麥克風兩條線。目前沒有任何全球統一的「聲學工程師完整能力地圖」官方清單，但可以用以下四份業界清單的交集拼一份對照草稿：
1. INCE（Institute of Noise Control Engineering）噪音控制工程師認證考照範圍
2. ASA（Acoustical Society of America）14個技術委員會涵蓋的領域
3. AES（Audio Engineering Society）技術委員會範疇
4. IEC 61094 麥克風校準/陣列標準涵蓋的技術項目

請你：
1. 找到並讀取這個 repo 裡定義 100 席位的實際檔案（`role_specs.py`、`role_acceptance.py`，或 `docs/AERIS_BLUEPRINT_ZH_TW.md` 裡對席位的描述）。
2. 檢索上述四份業界清單的「大分類」層級內容（噪音控制、心理聲學、揚聲器換能器設計、麥克風換能器設計、MEMS麥克風、陣列波束成形、房間聲學、電聲量測、法規合規…）。
3. 逐項比對，列出「AERIS 100席位有對應／沒有對應／不確定」。
4. **特別檢查**：麥克風校準是否有獨立席位（不是被籠統歸進「聲學量測」）；陣列波束成形是否有獨立席位。
5. 產出一份缺口清單，附上你的具體建議（哪些缺口值得新增席位、哪些是刻意排除、理由是什麼）。

不要修改任何 100 席位的既有定義檔案，除非我明確要求——這一輪只做評估與建議，先把缺口清單給我看。
```

---

## 2｜MEGIS 能力地圖自查

**目標資料夾**：`C:\0_JN1_MEGIS`（本機）
**目標 GitHub repo**：`Space653000/0_JN1_MEGIS`

```
請先讀這個 repo 自己的 CLAUDE.md / AGENTS.md / MEGIS_Blueprint 裡的治理規則，遵守既有的 Gate 制流程。

任務：對 MEGIS 現有的 G0-G9 Gate 架構做一次自查，對照全球機械工程界最具公信力的兩份官方骨架，找出涵蓋範圍的缺口。

對照來源：
1. ABET（Accreditation Board for Engineering and Technology）Engineering Accreditation Commission 的《Criteria for Accrediting Engineering Programs》裡 Mechanical Engineering Program Criteria 章節（ABET官網 Accreditation → Accreditation Criteria 可下載當年度PDF）——這份準則明確要求「熱力系統」（thermal systems）與「機械系統」（mechanical systems）兩軌都要涵蓋。
2. ASME《Vision 2030》報告（搜尋 "ASME Vision 2030" 即可找到PDF，內容是訪談1,470位業界人士的調查結果，指出畢業生普遍缺乏「實務經驗、溝通能力、系統性思維」）。

請你：
1. 讀取 `execution/PROJECT_STATE.md` 與 `MEGIS_Blueprint/…v3.0-claude-code.md`，確認目前 G0-G9 各 Gate 實際涵蓋的技術範圍（已知涵蓋治具幾何G2、聲學/機器人薄切片G7/G8）。
2. 對照 ABET 準則的機械系統/熱力系統兩軌，**明確回答：MEGIS 目前有沒有任何 Gate 涵蓋熱力系統（熱傳、流體、能量轉換）**，還是全部都只在機械系統這軌。
3. 對照 ASME Vision 2030 的調查結論，評估 MEGIS 的 Gate 審查機制有沒有涵蓋「跨模組系統性思維」這種整合考量，還是只逐一驗證單一零件。
4. 產出一份缺口清單，附上具體建議（如果要補熱力系統這軌，該從哪個 Gate 開始比較合理）。

不要修改既有 Gate 定義檔案，除非我明確要求——這一輪只做評估與建議。
```

---

## 3｜AIECP 憑證管理對照

**目標資料夾**：`C:\0_JN1_AIECP`（本機）
**目標 GitHub repo**：`Space653000/0_JN1_AIECP`

```
請先讀這個 repo 自己的 .ai/BLUEPRINT.md / .ai/STATUS.md / CLAUDE.md，遵守既有的九條產品不可退讓原則（尤其「人類權威」與「證據優於自我宣稱」）。

任務：對 Codex OFFICIAL / Codex PEGA 兩個 worker 現有的憑證/token 取用方式做一次對照評估。

對照原則（已由外部研究查證，2026年業界最佳實踐）：
- 憑證應該短效（不是長期有效）
- 限定單一任務範圍（不是萬用憑證）
- 由獨立的憑證 broker 簽發（不是寫死在設定檔）
- AI 推理引擎本身不直接持有原始憑證，只拿到 broker 核發的臨時憑證

請你：
1. 找出 Codex OFFICIAL 與 Codex PEGA 現在怎麼取得 GitHub token、API key（通常在 `.env`、環境變數設定腳本、CI 設定檔、或 CODEX_HOME 底下）。
2. 逐條對照上述四項原則，明確列出「符合／不符合」。
3. 如果評估後認為值得改善，請研究並提出一個低成本方案（例如 Infisical——一個輕量級的憑證管理工具，門檻比 SPIFFE/SPIRE 這種企業級方案低很多），說明導入的具體步驟與工作量估計。
4. 不要求你現在就導入，先給我一份「現況 vs 原則」對照表 + 改善方案的可行性評估。

不要修改任何憑證設定，除非我明確要求——這一輪只做評估與建議。
```

---

## 4｜AIECP：GitHub Artifact Attestations 導入評估

**目標資料夾**：`C:\0_JN1_AIECP`（本機）
**目標 GitHub repo**：`Space653000/0_JN1_AIECP`

```
請先讀這個 repo 自己的 .ai/BLUEPRINT.md，了解現有的五級證據分級（STATIC/TESTED/CI/ENVIRONMENT/OWNER-EXTERNAL）與現有的 SHA256SUMS + RELEASE_PROVENANCE.json 發布證明機制。

任務：評估導入 GitHub Artifact Attestations（微軟/GitHub官方2026年提供的建置證明功能，只要在 GitHub Actions workflow 加幾行 YAML，就能讓建置產物帶有官方可驗證的建置歷程證明，不用自己管理簽章金鑰）。

請你：
1. 確認 AIECP 現有的 GitHub Actions workflow 檔案位置與內容。
2. 研究 GitHub Artifact Attestations 的官方文件（`actions/attest-build-provenance`），評估要加進現有 workflow 需要改動哪些地方。
3. 評估這是否能跟現有的 SHA256SUMS + RELEASE_PROVENANCE.json 機制並存、互補，還是會衝突。
4. 若要進一步對齊 SLSA Level 3，研究 `slsa-framework/slsa-github-generator` 這個現成工具鏈是否適用，評估工作量。
5. 產出一份「現在能做的最小改動」建議（先上 Attestations），跟「若要衝 Level 3」的後續路徑。

如果評估後你認為這個改動風險低、效益明確，可以先做一個實驗性的 PR 讓我看，但不要直接合併到主分支。
```

---

## 5｜AIECP：MCP 2026-07-28 協定改版相容性檢查

**目標資料夾**：`C:\0_JN1_AIECP`（本機）
**目標 GitHub repo**：`Space653000/0_JN1_AIECP`

```
請先讀這個 repo 裡任何跟 MCP（Model Context Protocol）相關的整合程式碼與設定檔。

任務：MCP 協定在 2026-07-28 有一次重大改版（協定核心從有狀態變成無狀態、移除了初始化交握流程），官方稱是最大幅度的一次修訂。請檢查 AIECP 現有的任何 MCP 整合是否建立在舊版（有狀態、有初始化交握）的假設上。

請你：
1. 找出 AIECP 現在有沒有使用 MCP、用在哪裡（Provider Router？某個工具整合？）。
2. 對照官方 2026-07-28 改版的變更說明，逐項確認現有實作有沒有依賴已經被移除的機制。
3. 如果有相容性風險，具體列出哪些程式碼需要更新，並評估工作量。
4. 如果目前根本沒有用到 MCP，或用的部分完全不受影響，直接回報「無風險」即可，不用做多餘的改動。

這是一次相容性檢查為主的任務，只有在確認真的有相容性問題時才需要動手改程式碼，並且改動前要先讓我確認。
```

---

## 6｜MEGIS：CadQuery AI 生態系（MCP Copilot）導入評估

**目標資料夾**：`C:\0_JN1_MEGIS`（本機）
**目標 GitHub repo**：`Space653000/0_JN1_MEGIS`

```
請先讀這個 repo 自己的治理規則，了解 G4（模組與限制條件組合）目前的人工幾何草案流程。

任務：MEGIS 已經在用 CadQuery 當幾何核心。CadQuery 生態系本身已經長出會暴露 MCP server 的 AI copilot 工具，跟本專案技術棧天然相容（不像 Zoo.dev/AdamCAD 那種獨立商業產品）。請評估導入這類 CadQuery 原生 AI 工具，能不能加速 G4 階段的幾何草案產出。

請你：
1. 檢索目前 CadQuery 生態系裡暴露 MCP server 的 AI copilot 工具有哪些、各自的能力與限制。
2. 評估這類工具跟 MEGIS 現有的 LLM 角色限制是否相容——記得 MEGIS 的核心原則是「LLM只協助整理設計意圖、提出問題、解釋結果，不取代幾何核心、物理求解器或工程簽核」，導入的 AI copilot 不能違反這條原則。
3. 具體評估：這類工具能不能只用在「加速產生草案幾何」這個階段，仍然把最終驗證留給既有的 Gate 審查流程。
4. 產出一份可行性評估 + 如果可行，一個小範圍試用的具體步驟。

不要繞過既有 Gate 審查流程，這是評估任務，不是要求你現在就正式導入到生產流程。
```

---

## 7｜SuperBrain：Bastion Host 資安慣例補強

**目標資料夾**：`C:\SuperBrain`（本機，ULTRA-MAERA-2 上）
**目標 GitHub repo**：`Space653000/0_JN1_2AGAVE128-1MAERA64`

```
請先讀這個 repo 的 .ai/BLUEPRINT.md，了解現有「ULTRA-MAERA-2 唯一連網、兩台 Spark 隔離不連網」的網路架構設計。

背景：這個架構其實正是資安圈用了三十幾年的「Bastion Host（堡壘主機）」標準模式，不是自創的權宜設計，但目前沒有刻意套用 Bastion Host 的兩條配套慣例。

任務：評估並補強兩件事：
1. **精簡 ULTRA-MAERA-2 對外暴露的服務**——Bastion Host 的核心原則是這台機器本身應該盡量少開放服務、減少攻擊面。請盤點 ULTRA-MAERA-2 目前對外（家用網路/網際網路）暴露了哪些服務/連接埠，評估哪些是必要的、哪些可以關閉或限制存取來源。
2. **集中記錄 Laptop→Spark 的每一筆指令**——Bastion Host 標準做法是所有經過堡壘主機轉發的操作都要留下集中稽核紀錄。請評估現有的 SSH 連線／指令派送機制有沒有做到這件事，如果沒有，提出一個低成本的紀錄方案（不需要新裝額外的商用工具）。

請先給我盤點結果與具體建議，不要直接修改防火牆規則或網路設定，等我確認後再動手。
```

---

## 8｜AIECP／本 SuperSystem：Prompt Caching 命中率檢查

**目標資料夾**：`C:\0_JN1_AIECP`（本機，若你的 Claude Code 使用場景主要在 AIECP 這邊）
**目標 GitHub repo**：`Space653000/0_JN1_AIECP`

```
背景：Claude 的 prompt caching 在 2026-09-01 降價 75%（cache read 降到每百萬token $0.25），任何「同一份長內容被重複當輸入」的場景現在用快取的效益比降價前更高。

任務：檢查 AIECP 專案裡 Claude Code / Codex Reviewer 的使用模式，看有沒有「同一份長 system prompt / CLAUDE.md / BLUEPRINT.md 反覆被當作輸入」但沒有命中快取的情況。

請你：
1. 找出 AIECP 的 Claude Reviewer / Codex worker 每次呼叫時，system prompt 或 context 裡有多少是「每次都一樣、可以被快取」的內容（例如完整的 CLAUDE.md、BLUEPRINT.md）。
2. 確認目前的呼叫方式有沒有正確設定 prompt caching（例如是否把可快取內容放在 prompt 的固定前綴位置）。
3. 如果沒有命中快取，估算改成正確設定後大概能省下多少百分比的輸入成本。
4. 提出具體的程式碼/設定調整建議。

這是設定層級的低風險調整，如果評估後確認可以省錢又不影響功能，可以直接調整，調整後回報省了多少。
```

---

## 使用方式提醒

- 每份提示詞開新的 Claude Code session，並且指向對應的本機資料夾/GitHub repo（不是這個 SuperSystem repo）。
- 每份提示詞都內建「先讀那個 repo 自己的治理規則」——這是為了確保改動符合各專案自己的既有規矩，不是本 SuperSystem repo 越權指揮。
- 完成任一項後，回來這個 SuperSystem session 跟 Claude 說一聲（例如「AERIS能力地圖自查做完了」），我會幫你把結果更新進 [Blueprint/26 §9](26_VISION_DRIVEN_FRONTIER_RADAR.md)（或對應章節）跟 [Blueprint/19 進度總表](19_MASTER_PROGRESS_TRACKER.md)。
