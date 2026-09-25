# 14 — Failure Recovery and Resilience

## 使用者問題16：三台機器中若有一台故障會怎樣？

**目前沒有任何來源 repo 定義跨機故障轉移策略。** 現有的容錯設計都是單機/單專案範圍：

- SuperBrain：Spark heartbeat 10 秒一次，30 秒沒回應判定 OFFLINE，收回 lease 並重新派工（規劃中，未實作驗證，T16）。UPS 斷電時，Spark1 經隔離 LAN SSH 叫 Spark2 關機，再關閉自己（T29，規劃中）。
- AIECP：crash/restart 復原不得擴權；孤兒執行階段重新排隊；待處理 CI 監控恢復（已實作，`.ai/STATUS.md`）。

**缺口**：如果 ULTRA-MAERA-2（唯一連網節點）故障，目前沒有任何機制讓 SPARK-AGAVE-3/4 獨立對外運作（它們設計上本來就無出網路徑）；如果 AIECP 所在的機器故障，也沒有備援 AIECP 實例的設計。這代表目前的目標架構事實上有單點故障風險（ULTRA-MAERA-2 是唯一 Gateway），使用者若在意這點，需要額外設計備援方案。

## 使用者問題17：如果網路斷線會怎樣？

- Voice Agent：完全離線設計，不受影響（已驗證）。
- AIECP：Web Safe Bridge 依賴官方 ChatGPT Web，斷線會退化為無法接收新任務，但本機已派發任務的執行/驗證流程不依賴網路。
- SuperBrain：T17 驗收測試要求「本地的 health、status 與基本任務仍能運作」，包含離線語音，但**尚未實作驗證**。
- 兩台 Spark 節點本來就設計成不連網，不受網路斷線影響。

## 使用者問題18：雲端 Token 用完會怎樣？

- SuperBrain：Router 依 golden set 分流結果自動改走本地；Antigravity 免費額度耗盡時 Router 自動改派 fallback（T30 驗收測試，未實作驗證）。
- AIECP：Provider 健康狀態機標記為 `AUTH_REQUIRED`/`UNAVAILABLE`，但「選用供應商失敗不得拖垮 Safe Bridge」（R3 需求），即 ChatGPT Web 主線不受影響。

## 已知限制的誠實揭露（來自各專案自己的文件）

- Voice Agent：Wake→ASR 延遲 5.16 秒，離 1.5 秒目標仍有距離，屬於本機 ASR 模型的物理限制，非 bug。
- AIECP：10 項 ENVIRONMENT gate 明確列出「不能靠增加程式碼或測試關閉，只能靠真實環境執行」。
- SuperBrain：P0 尚未完成，兩台 Spark 尚無任何真實故障恢復測試數據。

## 建議（僅供參考）

若要落實使用者的目標架構，恢復能力設計至少需要回答：
1. ULTRA-MAERA-2 故障時，是否需要一個備援 Gateway（例如另一台可以暫時接手的機器）？目前無設計。
2. AIECP 的 Harness 狀態如果需要跨機備份/恢復，是否應該借用 SuperBrain 的三機拓撲做異地備份？目前無設計。
3. 這些都是需要 Stephen 決定優先順序後，交由對應專案自己設計實作的新工作項目。
