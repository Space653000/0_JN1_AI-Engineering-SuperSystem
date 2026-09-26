# 18 — Decision Log

記錄本 SuperSystem repo 建置過程中做的重要判斷。這是本 repo 自己的決策紀錄，不代表任何來源 repo 的決策。

| 日期 | 決策 | 理由 |
|---|---|---|
| 2026-09-25 | 採用只讀方式盤點全部七個來源 repo（其中六個公開透過匿名 clone、一個私有 repo 透過 add_repo 附加 read 權限） | 遵守任務指示的「絕對規則」：只讀不寫其他 repo |
| 2026-09-25 | 排除 `0_JN1_Robotcar` | 使用者明確指示排除 |
| 2026-09-25 | 對 AIECP 命名採用「本 SuperSystem repo 統一使用 AIECP 稱呼，但引用其原文時保留 AECP」的折衷方案 | 使用者堅持系統名稱為 AIECP，但誠實引用來源文件的實際用字（見 Audit/CONFLICT_ANALYSIS.md C-03） |
| 2026-09-25 | 把使用者「confirmed architecture」明確標示為 TO-BE（目標），並在每個相關章節同時列出 AS-IS（現況） | 避免讀者誤以為這是已經運作的系統；同時不擅自用現況推翻使用者的方向 |
| 2026-09-25 | AERIS Supervision 的擴展建議維持保守（建議選項 A：AERIS-only），不主動提議擴大範圍 | 沒有任何來源 repo 表達過擴展需求，避免本 repo 過度延伸建議 |
| 2026-09-25 | 對 AIECP `Blueprint/01`-`20`、`24` 等未逐份精讀的文件，在 Audit/GAP_ANALYSIS.md 中誠實揭露此限制 | 遵守「有證據才下結論」原則，避免基於未讀內容做出過度推論 |
| 2026-09-25 | 使用 grep 全文搜尋交叉驗證「repo 之間是否互相提及」的關鍵發現（C-01、C-02），而非僅憑抽樣閱讀判斷 | 這是本次盤點最重要的結論，需要更高的確定性 |
| 2026-09-25 | **Stephen 裁定 C-04 / D-01（AIECP vs SuperBrain 重疊）分工邊界**：Queue / Router / Worker / Evidence / Approval 這組「控制平面原語」歸 AIECP 所有；SuperBrain **不得**另建一套同性質的 Queue/Router/Worker/Evidence/Approval，SuperBrain 的定位往上提升為**跨機資源調度與統籌規劃層**（決定「哪個任務該去哪台機器」的上層決策，而非重造 AIECP 已有的排程/佇列機制）。此為 Stephen 本人對本 SuperSystem 分工方向的決策，尚未回頭修改任一來源 repo，屬於本 repo 記錄的「建議分工方向」，實際落地仍需 AIECP / SuperBrain 各自專案採納 | 避免 D-01 所述「兩套控制邏輯各自演化、未來分裂成不相容真相」的風險重複；使用者明確裁示不想重工 |
| 2026-09-25 | G-04（SPARK-AGAVE-4 獨立驗證 SPARK-AGAVE-3 的協議）Stephen 認領，列為「使用者後續自行設計」，本輪不代為設計 | 避免本 SuperSystem repo 越權替來源專案做架構決策（house rule #5） |
| 2026-09-25 | 依 Stephen 要求，檢索 2026 年公開業界資料，佐證上述 C-04/D-01 分工裁決與 G-04 的可能參考模式，寫入 [Audit/EXTERNAL_RESEARCH.md](../Audit/EXTERNAL_RESEARCH.md)（Agent Orchestration vs Control Plane 分層、Verifier Pattern / Maker-Checker 職責分離、System-of-Systems 自治框架） | 業界慣例可作為決策佐證與後續設計輸入，但不取代 Stephen 或各專案自己的判斷 |
| 2026-09-25（第二輪） | 依 Stephen 要求重新盤點：新增 `Blueprint/19_MASTER_PROGRESS_TRACKER.md` 作為跨專案總覽儀表板。編號延續既有 00–18 的順序（19 為下一個號碼），不另開命名系統，符合 house rule #6「沿用既有命名方式」的精神——這是一份既有 00–18 號文件都沒有涵蓋的新性質產物（可供 Stephen 自行勾選更新的活體追蹤表），因此判斷為新增檔案而非塞進既有文件 | 使用者明確要求「一個總表」，且要求以後可以自己回來更新進度；既有 Blueprint 文件都是一次性分析產出，沒有一份是設計給使用者手動編輯的活體追蹤介面 |
| 2026-09-25（第二輪） | 重新 clone/pull 全部 7 個來源 repo 的當前 HEAD（而非沿用第一輪 clone 的快照），逐一比對是否有實質進度變化 | 使用者要求「re-confirm 各自進度」，且明確提到「以後我只要完成各自功能就會回來跟這個藍圖回報」，代表這份 tracker 之後會被反覆信任為現況依據，必須先確認自己不是憑第一輪的舊資料 |
| 2026-09-25（第三輪） | 新增 `Blueprint/20_PROPOSED_INTERFACE_CONTRACTS.md`（G-01/G-02/G-03 三個缺失介面的具體草案）與 `Blueprint/21_VOICE_SUPERBRAIN_INTEGRATION_PROPOSAL.md`（D-02 的具體整合建議），編號延續 19 之後的順序。同步擴充 `Registry/INTERFACES.yaml`，在既有 `MISSING` 條目下新增 `proposed_contract` 欄位（含 precedent 引用），並在 `Architecture/SYSTEM_MAP.md`、`MACHINE_MAP.md`、`DATA_FLOW.md` 補上 Mermaid 圖 | 使用者明確要求把 G-01/G-02/G-03 從「已知缺口」推進到「具體草案」，並把 D-02 從「重複風險」推進到「可執行的整合建議」；全文一律使用建議/草案語氣，不宣稱已被來源 repo 採用（house rule #2） |

| 2026-09-26 | 依 Stephen 要求，把七專案施工順序建議從形容詞改為量化評分（見 [16](16_ROADMAP_AND_ACCEPTANCE.md)），公式與權重明確標註為本 repo 自訂判斷，非科學公式 | 避免用「動能強」「優先度低」這類無法比較的形容詞 |
| 2026-09-26 | 記錄 Stephen 對 AERIS／AERIS Local Implementation／AERIS Supervision 的角色定位（藍圖／主施工／副監工），並記錄這是「Codex 的債」、Stephen 要自己先理清才會處理（含 AERIS Supervision 更名/擴大範圍一事）。本 repo 在此之前不代為調整這三者的分工或評分 | 尊重 house rule #5（保持各專案自治），避免在 Stephen 自己理清前搶先下判斷 |

| 2026-09-26 | **Stephen 澄清本 repo 建立初衷**：主要工作是持續往雲端/全球檢索最新 AI 進展，對標本地七專案，把落差寫成建議（見新增 [22_GLOBAL_TECH_RADAR.md](22_GLOBAL_TECH_RADAR.md)），System-of-Systems 盤點藍圖是這個初衷的副產品，不是主要目的。已同步更新 README.md「建立初衷」段落 | 修正先前只做靜態盤點的定位偏差，回到使用者原始動機 |
| 2026-09-26 | **Stephen 確認目標流程**：Voice Control 應先接 AIECP，AIECP 再分派給 AERIS＋MEGIS（不是 Voice 直接接 AERIS）。這與既有 [Blueprint/20](20_PROPOSED_INTERFACE_CONTRACTS.md) G-01/G-02 提案方向一致，本次是使用者對此方向的正式確認，非新提案 | 讓語音入口具備完整架構能力，而不是繞過控制平面直接綁定單一領域 |
| 2026-09-26 | **Stephen 確認 AERIS Supervision 目標範圍**：擴大監管三台本地機器（ULTRA-MAERA-2/SPARK-AGAVE-3/4）＋ Voice Control ＋ AIECP ＋ AERIS ＋ MEGIS，即監管全系統，不再只服務 AERIS 一個專案。**這是目標方向的確認，不是本 repo 代為執行的實作**——實際改名/擴大範圍的工程需要另開一個對 `0_JN1_AERIS_Supervision` 有寫入權限的 session 執行，本 repo 只記錄方向 | 呼應 house rule：本 repo 只讀不寫其他 repo，架構決策記錄與實際落地分屬不同 session |

| 2026-09-26 | **本 repo（雲端獨立）提案並經 Stephen 選定正式名稱**：整套「三台本地機器＋Voice Control＋AIECP＋AERIS＋MEGIS」混合整體命名為 **JN1 Unified Operations Domain（JN1-UOD）**；AERIS Supervision 擴大監管全部後的新名稱為 **JN1 Unified Oversight Authority（JN1-UOA）**。命名風格比照 AIECP（AI Engineering Control Plane）的「功能性縮寫」慣例，非神話/拉丁詞。**這只是命名提案的選定，不是任何 repo 的改名執行**——`0_JN1_AERIS_Supervision` 本身尚未改名，需 Stephen 另開有寫入權限的 session 執行 | Stephen 明確要求由 cloud 獨立建議名稱，且要求純功能性、避免創意命名的歧義/版權疑慮 |

| 2026-09-26 | **重大雷達發現**：Stephen 提供 Microsoft Build 2026 中文報導線索，本 repo 查證確認 SPARK-AGAVE-3/4 對應的硬體是 **Microsoft Surface RTX Spark Dev Box**，正式上市日 **2026-10-07**，本文撰寫時尚未上市。已更新 [Blueprint/22 雷達#7](22_GLOBAL_TECH_RADAR.md)、[Blueprint/23 彙整報告](23_TECH_RADAR_SUMMARY_REPORT.md)、[Blueprint/03](03_MACHINE_ARCHITECTURE.md)。**推論**（未經 Stephen 確認）：SuperBrain P0 硬體盤點卡住，可能原因是硬體還沒上市，不是施工延遲 | 這是本輪雷達最重要的單一發現，直接影響 Roadmap 對 SuperBrain P0 進度的解讀方式 |

| 2026-09-27 | Stephen 口頭補充三個專案的定位（記錄於 [Blueprint/26](26_VISION_DRIVEN_FRONTIER_RADAR.md)、[Blueprint/23](23_TECH_RADAR_SUMMARY_REPORT.md)）：AERIS＝聲學工程師全方位能力盤點、MEGIS＝機構工程師全方位能力盤點、AIECP＝跨領域協調MEGIS與AERIS（協調/溝通/接收指令/分配交付/銜接本地雲端）。三者現在做不到的能力項目，未來交給對應子藍圖與工程專案逐步達成。同時再次確認 AERIS Supervision 未來改名為監管全系統施工/監工/運作狀況（呼應既有 JN1-UOA 決策）| 這些是 Stephen 對各專案定位的補充說明，比來源 repo 現有文件更具體，本 repo 誠實標註來源為口頭確認、非 repo 文件引用，不代替各專案自行修改藍圖 |

| 2026-09-27 | **Stephen 接受本輪四條深挖建議**：①AERIS——自行拼一份 INCE/ASA/AES/IEC 61094 交集草案對照100席位，特別檢查麥克風校準/波束成形是否被獨立列出；②AIECP——直接借用 NASA OpenMDAO 的「共用設計變數/誰先決定/如何收斂衝突」詞彙設計 MEGIS↔AERIS 協調層（G-02）；③Voice Agent——雙向澄清對話方向接受，但 3-4B模型是否可離線即時跑在本地機器（推理速度而非記憶體，ULTRA-MAERA-2 64GB 記憶體本身綽綽有餘）留待實際設計時才研究判斷；④MEGIS——直接採用 ABET機構工程認證準則+ASME Vision 2030 對照 G0-G9，尤其熱力系統這條軌 | 記錄為 Stephen 已接受的方向決策，實際落地仍屬各專案自己的施工，本 repo 不代為執行 |

| 2026-09-26（第四輪） | 完成三個新深挖主題（見 [Blueprint/26](26_VISION_DRIVEN_FRONTIER_RADAR.md) §13-15）：JN1-UOA×Assurance Case（GSN/CAE/DO-178C等）方法論、SuperBrain小規模艦隊管理與Bastion Host隔離節點模式、AERIS Local Implementation可重現工程模擬（RO-Crate/W3C PROV/位元級可重現）；同步更新 [Blueprint/23](23_TECH_RADAR_SUMMARY_REPORT.md) 四欄比較 | 延續 2026-09-27 已接受的四條深挖建議之外，Stephen 要求的第二批深挖主題，聚焦在「文件/監控存在」與「實際結果」之間如何結構化連結的方法論 |

| 2026-09-27 | Stephen 接受 §13-15（JN1-UOA×GSN保證論證、SuperBrain堡壘主機模式確認、AERIS可重現模擬容器化）三條深挖建議 | 記錄為已接受方向，實際落地仍是各專案自己的施工 |

| 2026-09-26（第五輪） | 完成兩個新深挖主題（見 [Blueprint/26](26_VISION_DRIVEN_FRONTIER_RADAR.md) §16-17）：G-01/G-02/G-03 具體協定參照（A2A逐欄位對照`aecp.task/v1`、AGENTS.md角色釐清、STEP AP242/OSLC誠實無解、EDA Handoff Perspective論文的Stage/Flow/Organization-Bound分類）、Public Portal安全公開投影前沿（Anthropic Transparency Hub、SRE公開狀態頁「內外分開維護」共識、PII脫敏工具的語意判斷落差）；同步更新 [Blueprint/23](23_TECH_RADAR_SUMMARY_REPORT.md) 四欄比較 | Thread H/I 兩個新分支，延續 Stephen「90%外部前沿、10%本地對照」的既定方向 |

| 2026-09-27 | Stephen 接受 §16-17（G-01/02/03 A2A逐欄位對照、Public Portal安全公開投影前沿含「即時投影不可行、需人工審查」的誠實結論）| 記錄為已接受方向 |

| 2026-09-26（第六輪） | 完成兩個新深挖主題（見 [Blueprint/26](26_VISION_DRIVEN_FRONTIER_RADAR.md) §18-19）：G-04 深挖（借用 SAFECOMP 2026 GSN 案例，畫出「SPARK-AGAVE-4 驗證 SPARK-AGAVE-3」報告可借用的 GSN 形狀草圖，明確標註非協定設計、不涉及核准邏輯/流程/觸發條件，仍由 Stephen 自行設計）、R-01 深挖（單一連網閘道 ULTRA-MAERA-2 故障情境的小規模對策——USB行動網路備援WAN、獨立於Laptop本身的告警裝置，並區分「Spark無法連雲端AI屬既有穩態設計」vs「Stephen無法下指令屬真正新增風險」兩種不同性質的影響）；同步更新 [Blueprint/23](23_TECH_RADAR_SUMMARY_REPORT.md) 四欄比較 | Thread J/K 兩個新分支，J 延續 §13 GSN 方法論但明確劃清「參考範例」與「代為設計」的界線；K 延續 §14 Bastion Host 確認，往下挖 Stephen 明確要求的「誠實評估風險大小」問題 |

| 2026-09-27 | Stephen 接受 §18-19（G-04的GSN參考草圖，明確非協定設計；R-01風險拆解後確認不需蓋第二台Gateway）| 記錄為已接受方向 |

## 尚待 Stephen 決策的事項（本 repo 整理，不代為決定）

1. 是否要推進 AIECP ↔ AERIS/MEGIS ↔ SuperBrain 的整合，以及優先順序（見 [16](16_ROADMAP_AND_ACCEPTANCE.md)）。
2. AIECP 的命名是否要統一為 "AIECP"（目前 repo 內文件全用 "AECP"）。
3. ~~AERIS Supervision 是否要擴展為跨專案發布監督~~ → **方向已於 2026-09-26 確認**（擴大監管三機＋Voice Control＋AIECP＋AERIS＋MEGIS，見上表），但要等 Stephen 理清 AERIS／Local Impl／Supervision 三者的「Codex 債」後才動工，且實際執行需另開對 `0_JN1_AERIS_Supervision` 有寫入權限的 session（見 [10](10_SUPERVISION_AND_EVIDENCE.md)）。
4. Voice Agent 現用的 RTX Spark 機器身分確認（是否為 SPARK-AGAVE-3/4 之一）。
5. ~~SPARK-AGAVE-4 驗證 SPARK-AGAVE-3 的具體協議設計~~ → 已由 Stephen 認領（見上表 2026-09-25），本 repo 不再列為待決事項，改追蹤於 Roadmap（見 [16](16_ROADMAP_AND_ACCEPTANCE.md)）。
