# System Map

> 這張圖畫的是**使用者確認的目標架構（TO-BE）**，不是目前任何 repo 已實作的現狀。目前現狀請見 [Audit/REPOSITORY_INVENTORY.md](../Audit/REPOSITORY_INVENTORY.md) 與 [Audit/CONFLICT_ANALYSIS.md](../Audit/CONFLICT_ANALYSIS.md) C-01。

## 目標架構（TO-BE）

```text
                              ┌───────────────────────────┐
                              │          Human             │
                              │        (Stephen)           │
                              └──────────────┬──────────────┘
                                             │ 語音 / 文字
                                             ▼
                         ┌───────────────────────────────────────┐
                         │            ULTRA-MAERA-2                │
                         │   主控電腦 / HMI / Internet Gateway      │
                         │  ┌───────────┐  ┌─────────────────────┐│
                         │  │Voice Agent│─▶│        AIECP          ││
                         │  │(STT/Intent│  │ Mission/Task/Queue    ││
                         │  │  /HMI)    │  │ Planner→Builder→      ││
                         │  └───────────┘  │ Reviewer→Verifier     ││
                         │                 │ Git/CI/Evidence/      ││
                         │                 │ Approval/Provider     ││
                         │                 │ Router                ││
                         │                 └──────┬───────┬────────┘│
                         │        ┌────────────────┘       │        │
                         │        ▼                        ▼        │
                         │  ┌───────────┐          ┌───────────┐   │
                         │  │   AERIS   │          │   MEGIS   │   │
                         │  │(acoustic  │          │(mechanical│   │
                         │  │ domain)   │          │ domain)   │   │
                         │  └───────────┘          └───────────┘   │
                         └──────────────┬──────────────┬────────────┘
                                        │              │
                         ┌──────────────▼──┐        ┌──▼───────────────┐
                         │  SPARK-AGAVE-3   │        │  SPARK-AGAVE-4    │
                         │  FAST worker     │        │  DEEP / verify    │
                         │  local LLM       │        │  reviewer, audit  │
                         │  batch/high-token│        │  red-team, QA gate│
                         └──────────────────┘        └────────────────────┘

                    SuperBrain = ULTRA-MAERA-2 + SPARK-AGAVE-3 + SPARK-AGAVE-4（合稱運算織理）
```

## 現狀架構（AS-IS，本輪盤點證實）

```text
   Human                Human                    Human
     │                    │                         │
     ▼                    ▼                         ▼
┌─────────────┐   ┌───────────────┐         ┌───────────────┐
│ Voice Agent │   │ ChatGPT Web   │         │ ChatGPT Web/   │
│ (RTX Spark, │   │  ──▶ AIECP    │         │ Claude/Gemini  │
│ 完全離線)    │   │ (通用 Git 控制 │         │  ──▶ SuperBrain│
└──────┬──────┘   │  平面)        │         │ (規劃中，P0未起)│
       │ ORDER.md │└───────────────┘         └───────────────┘
       ▼
┌─────────────┐        ┌──────────────────────┐
│ AERIS Core   │◀──────│ AERIS Local Impl      │
│ (Blueprint)  │       │ (aeris_runtime)       │
└──────┬───────┘       └───────────────────────┘
       │ 手動觸發
       ▼
┌─────────────────┐        獨立、互不知道彼此存在：
│ AERIS Supervision│        MEGIS（C:\0_JN1_MEGIS，明確聲明不與 AERIS/Voice Agent 共用環境）
│ (發布快照)        │        AIECP（通用控制平面，未綁定任何工程領域）
└──────────────────┘        SuperBrain（通用多機調度，未提及任何領域專案）
```

**結論**：現狀是五～六座孤島，各自有自己完整的治理/驗收/證據體系，彼此之間只有一條已知的橋（Voice Agent ↔ AERIS 的 `ORDER.md`）。目標架構要求的其餘所有連線目前都是 `MISSING`（見 [Registry/INTERFACES.yaml](../Registry/INTERFACES.yaml)）。

## 合併視圖（Mermaid）：human → voice → AIECP → domain → execution → verify → evidence → approval → release

> 圖例：**實線 = AS-IS（本輪盤點證實可運作）**；**虛線 = TO-BE（目標架構，目前 MISSING/規劃中，未實作）**。虛線上的 `G-0x` 標記對應 [Blueprint/17](../Blueprint/17_RISK_GAP_CONFLICT_REGISTER.md) 的缺口編號與 [Blueprint/20](../Blueprint/20_PROPOSED_INTERFACE_CONTRACTS.md) 的建議草案，草案本身尚未被任何來源 repo 採用。

```mermaid
flowchart TD
    Human([Human 語音／文字])

    Human -->|語音，已驗證| VoiceAgent[Voice Agent<br/>STT/Intent/HMI<br/>已驗證：中文辨識97.9%]
    Human -.->|TO-BE：語音下指令直達控制平面| AIECP

    VoiceAgent -->|"ORDER.md 檔案，已驗證<br/>docs/05_AERIS_Integration_Split.md"| AERIS[AERIS<br/>聲學工程權威]
    VoiceAgent -.->|"G-01 TO-BE：aecp.task/v1<br/>Command Card 檔案掉落<br/>Blueprint/20 §G-01"| AIECP[AIECP<br/>Mission/Task/Queue<br/>Provider Router]

    AIECP -.->|"G-02 TO-BE：domain-dispatch<br/>Blueprint/20 §G-02"| AERIS
    AIECP -.->|"G-02 TO-BE：domain-dispatch<br/>Blueprint/20 §G-02"| MEGIS[MEGIS<br/>機構工程權威<br/>G0-G3 closed, G4 進行中]
    AIECP -.->|"G-03 TO-BE：superbrain-fabric<br/>as local Provider<br/>Blueprint/20 §G-03"| SuperBrain[SuperBrain<br/>跨機統籌規劃層<br/>P0 未完成]

    AIECP -->|"AS-IS：ChatGPT Web →<br/>aecp.task/v1 Command Card"| AIECPHarness[AIECP Harness<br/>Planner→Builder→<br/>Reviewer→Verifier]

    AERIS -->|手動觸發，已驗證| AERISSup[AERIS Supervision<br/>發布快照 S0005]

    AIECPHarness -->|"AS-IS：exact-HEAD CI"| Execution[Execution<br/>本機工具／Codex OFFICIAL+PEGA]
    SuperBrain -.->|TO-BE：P1-P2 完成後| SparkFast[SPARK-AGAVE-3<br/>FAST worker]

    Execution -->|"AS-IS：npm test 290/290"| Verify[Verify<br/>Reviewer/確定性測試]
    SparkFast -.->|TO-BE：G-04 待 Stephen 自行設計| SparkDeep[SPARK-AGAVE-4<br/>DEEP／獨立驗證]

    Verify -->|已驗證| Evidence[Evidence<br/>AIECP 五級：STATIC/TESTED/<br/>CI/ENVIRONMENT/OWNER-EXTERNAL]
    Evidence -->|"AS-IS：RED 等級需<br/>exact-action digest"| Approval[Human Approval<br/>Stephen]
    Approval -->|"AS-IS：PR #7 待合併"| Release[GitHub / Release]

    classDef asis fill:#1f6f43,stroke:#0e3d24,color:#fff;
    classDef tobe fill:#5a4a1f,stroke:#3a2f10,color:#fff,stroke-dasharray: 4 3;
    class VoiceAgent,AERIS,AERISSup,AIECPHarness,Execution,Verify,Evidence,Approval,Release asis;
    class AIECP,MEGIS,SuperBrain,SparkFast,SparkDeep tobe;
```

**如何讀這張圖**：AIECP 目前確實存在且運作（灰底綠色節點的 Harness 分支），但它服務的是「ChatGPT Web → 通用 Git 任務」這條線，**不是**圖中虛線描繪的「Voice Agent → AIECP → 領域路由 → SuperBrain」目標流程；後者整條路徑目前都是建議草案（[Blueprint/20](../Blueprint/20_PROPOSED_INTERFACE_CONTRACTS.md)），尚未有任何一段被來源 repo 實作。
