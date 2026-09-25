# CLAUDE.md — 這個 repo 的 house rules

給任何在 `0_JN1_AI-Engineering-SuperSystem` 裡工作的人類或 AI（Claude Code、Codex、ChatGPT……）。

## 絕對規則

1. **絕不修改其他 repository。** `0_JN1_AERIS`、`0_JN1_AERIS_Local-computer-implementation`、`0_JN1_AERIS_Supervision`、`Offline-Local-Voice-Agent`、`0_JN1_MEGIS`、`0_JN1_AIECP`、`0_JN1_2AGAVE128-1MAERA64` 對本 repo 而言全部是**唯讀來源**。不 clone 後編輯、不開 PR、不改 branch/tag、不寫 issue。想像它們是唯讀掛載的參考資料。
2. **所有分析、藍圖、建議都只寫進這個 repo。** 想對其他專案提出建議，寫進 `Audit/` 或 `Blueprint/17_RISK_GAP_CONFLICT_REGISTER.md`，用「建議」的語氣，不是「已執行」的語氣。
3. **每個結論都要能指出來源。** 引用其他 repo 的內容時，附上「repo 名稱 + 檔案路徑（+ 大致的行號或段落）」。找不到來源、或多個來源互相矛盾時，一律標記 `UNKNOWN` / `NOT VERIFIED` / `NEEDS REVIEW`，不要用常識或推測補上。
4. **不要把「有藍圖文件」等同「已經做完」。** 這是這幾個來源 repo 自己反覆強調的鐵律（AECP 的 STATIC/TESTED/CI/ENVIRONMENT/OWNER-EXTERNAL 證據分級、AERIS 的「文件存在不構成完成證據」），本 repo 延用同一個標準。
5. **保持各專案自治。** 不要在這裡幫 AERIS/MEGIS/AIECP/Voice Agent/SuperBrain 做架構決策；只描述現況、指出衝突與缺口，決策權留給各專案自己的治理流程與 Stephen 本人。
6. **中英混寫是預期行為。** 標題與檔名維持英文（方便跟其他 repo 對齊），說明文字可以用繁體中文，這是刻意的風格選擇，不需要「修正」成全英文。

## 遇到衝突時怎麼辦

- 兩個來源 repo 的文件互相矛盾 → 寫進 `Audit/CONFLICT_ANALYSIS.md` 或 `Blueprint/17_RISK_GAP_CONFLICT_REGISTER.md`，兩邊都引用，不擅自判定誰對。
- 使用者在原始需求中確認的架構方向，跟實際 repo 內容不一致 → **不要用實際 repo 內容悄悄推翻使用者的方向**。把使用者的方向記錄下來，同時把「目前 repo 尚未反映這個方向」記錄成落差/風險項目。
- 私有 repo（例如 `0_JN1_AERIS_Supervision`）如果無法存取 → 標記 `NOT VERIFIED / ACCESS DENIED`，不要用其他公開資訊推測其內容。

## 檔案結構規則

新增文件時，沿用既有的 `Blueprint/00–18`、`Registry/*.yaml`、`Audit/*.md`、`Architecture/*.md` 命名方式，不要另開一套命名系統。
