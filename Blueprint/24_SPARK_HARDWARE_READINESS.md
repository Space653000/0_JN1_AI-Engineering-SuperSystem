# 24 — SPARK-AGAVE-3/4 到貨前準備清單

> 這不是提醒排程，是**一份現在就能看完、你買到機器那天直接照做的清單**。所有安裝步驟其實 SuperBrain repo 自己早就寫好了（見下方路徑），本文件只是把「買之前要先準備什麼」「路徑在哪」整理成一頁，不重寫 SuperBrain 的 SOP 內容。

## 硬體與上市狀態（見 [22_GLOBAL_TECH_RADAR.md](22_GLOBAL_TECH_RADAR.md) 雷達#7）

- 機型：**Microsoft Surface RTX Spark Dev Box**，NVIDIA RTX Spark（N1X）晶片，128GB 統一記憶體，1 petaFLOP，100W TDP 桌機造型。
- 正式上市：**2026-10-07**，首波僅美國、僅 Microsoft.com 獨家銷售。**若你不在美國，購買/運送/報關的前置時間要另外抓，不是上市當天就能拿到手**——這件事本 repo 沒有你的實際地理位置資訊，無法幫你估算，請自行確認。
- 需要買 **兩台**（SPARK-AGAVE-3、SPARK-AGAVE-4 各一台，規格相同）。

## 安裝 SOP 已經寫好了，路徑在這裡（來源：`0_JN1_2AGAVE128-1MAERA64`，唯讀）

| 機器 | SOP 路徑 | 腳本路徑 |
|---|---|---|
| ULTRA-MAERA-2（你現在用的筆電，要先做這台） | `ULTRA-MAERA-2/README.md` | `ULTRA-MAERA-2/scripts/laptop-isolated-nic-setup.ps1`、`laptop-generate-ssh-key.ps1` |
| SPARK-AGAVE-3（先做這台，穩定幾天再做下一台） | `SPARK-AGAVE-3/README.md` | `SPARK-AGAVE-3/scripts/spark-bootstrap.ps1` |
| SPARK-AGAVE-4（等 SPARK-AGAVE-3 穩定後才做） | `SPARK-AGAVE-4/README.md` | `SPARK-AGAVE-4/scripts/spark-bootstrap.ps1`（跟 SPARK-AGAVE-3 同一支腳本，換參數） |

GitHub 直連：https://github.com/Space653000/0_JN1_2AGAVE128-1MAERA64

## 買之前／到貨前先準備好這幾樣東西

1. **2.5GbE 隔離交換器** — 不接家用 Router，先買好、擺好位置。
2. **USB-C 2.5GbE 網卡**（給 ULTRA-MAERA-2 用，接隔離交換器）。
3. **一台螢幕+鍵盤**（給沒有內建螢幕的 Dev Box 開機用，兩台 Spark 輪流用同一套即可）。
4. **USB 隨身碟**（Spark 裝好後會斷網，腳本要用隨身碟帶過去，不能臨時下載）。
5. 先確認 ULTRA-MAERA-2 上**還沒有**跑過 `laptop-generate-ssh-key.ps1`（金鑰只產生一次）——如果你想現在（機器到貨前）就先做完 ULTRA-MAERA-2 這一端的準備，是可以提前做的，跟 Spark 到不到貨無關。

## 到貨後的順序（照 SuperBrain 自己的 SOP，不是本 repo 發明的）

1. ULTRA-MAERA-2：裝隔離網卡 → 產生 SSH 金鑰 → 把公鑰（`superbrain_ed25519.pub`）拷到隨身碟。
2. SPARK-AGAVE-3：開機設定 → 暫時連網跑 Windows Update/NVIDIA驅動/`wsl --install` → 改接隔離交換器 → 用隨身碟帶腳本進去跑 `spark-bootstrap.ps1 -Name spark-agave-3 -StaticIP 10.77.0.11 ...`。
3. 回 ULTRA-MAERA-2 驗證 `ssh <帳號>@10.77.0.11 hostname` 成功。
4. 回 SPARK-AGAVE-3 跑 `-KeyOnly`（停用密碼登入，這是 RED 動作，一定要等第3步驗證成功才做）。
5. **穩定幾天後**，SPARK-AGAVE-4 重複第2-4步（IP 換成 `10.77.0.12`，電腦名稱 `spark-agave-4`）。**不要兩台同時暫時連網**（SuperBrain 自己的維護窗口規則）。

## 驗收清單（來源同上，抵達後自己勾）

- [ ] 兩台 Spark 各自 SSH 連續 100 次成功
- [ ] 兩台 Spark 上 `Test-NetConnection 8.8.8.8` 與 `Resolve-DnsName microsoft.com` 都**必須失敗**（斷網是預期行為）
- [ ] 防火牆只放行 ULTRA-MAERA-2（10.77.0.1）的 22、30000-30010 埠
- [ ] 兩台密碼登入都已停用
- [ ] ULTRA-MAERA-2 端 `ping 10.77.0.11` / `10.77.0.12` 各 1000 次 0% loss，且跑過 iperf3 基準

## 跟本次雷達（#2、#9）的銜接

機器到手、P0 盤點完成後，直接回來跟我說一聲，我可以把雷達 #2（本地LLM選型：MoE模型優先）跟 #9（跨機推理：EXO/vLLM+Ray）的建議套用到你實際確認的規格上，不用再等。

## 邊界提醒

以上路徑與腳本內容全部來自 `0_JN1_2AGAVE128-1MAERA64` 這個既有 repo（唯讀），本文件沒有新增或修改任何一行安裝腳本，只是把散在 README/腳本/雷達裡的資訊收斂成一份「買之前先看這個」的清單。
