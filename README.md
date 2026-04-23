# mae-mfe-optimization-skill

把公開分享的 MAE/MFE 交易分析內容，整理成可供 Codex / AI agent 使用的 skill。

這個 repo 是文件與方法論導向，不是回測引擎、最佳化 API 或下單框架。它的目標是把 MAE/MFE 策略優化的概念、術語和決策流程整理成 agent 可重複使用的知識包。

English version: [README.en.md](./README.en.md)

## 這個 skill 包含什麼

主題聚焦在以 MAE/MFE 為核心的策略優化，包括：

- MAE、MFE、Global MFE、MHL、Edge Ratio 的定義
- Delayed Entry 設計
- Stop Loss / Take Profit 配置
- Trailing Stop / Breakeven 的判斷
- Scaling in / Scaling out 的適用條件
- 深度優化前的 Practical Triage
- 以避免過擬合為前提的優化順序

適合拿來讓 agent：

- 解釋 MAE/MFE 優化框架
- 提出較合理的優化順序
- 討論 SL/TP 或延遲進場選擇
- 區分結構性改善與回測表面績效
- 用研究流程而不是零碎技巧理解這個主題

## Repo 結構

```text
.
├── README.md
├── README.en.md
├── SKILL.md
└── references
    ├── chapter-details.md
    └── practical-triage.md
```

### `SKILL.md`

主 skill 定義，包含：

- 觸發條件與使用範圍
- 核心術語
- 五步驟優化主軸
- 章節與決策對照
- 反過擬合原則

### `references/practical-triage.md`

以前置分流 `PF -> WR -> RF` 判斷策略是否值得進入更深的 MAE/MFE 分析。

### `references/chapter-details.md`

補充主要主題的展開說明，包括：

- ATR 波動標準化
- 時間面 MAE/MFE 與 Edge Ratio
- Placement Test
- Delayed Entry
- 環境與 regime filter
- Trailing Stop / Breakeven
- 加減碼與帳戶層級風控

## 方法論摘要

這份 skill 的主線不是直接暴力調參，而是先問三件事：

1. 這個策略到底值不值得優化？
2. 瓶頸出在進場、SL/TP、波動環境，還是持倉管理？
3. 改善是交易內部結構的改善，還是只是回測結果好看？

目前 skill 內整理的優化順序是：

1. 延遲進場
2. 停損止盈調整
3. 環境過濾
4. 動態停損
5. 加減碼
6. 帳戶層級避險

重點是優先觀察 MAE、MFE、Global MFE、MHL、Edge Ratio 這類交易內部結構，而不是只看 MDD、Sharpe 這些表層總績效指標。

## 安裝成 Codex skill

### 方式 1：直接 clone 到 Codex skills 目錄

若你的 Codex skills 目錄是 `~/.codex/skills`，可直接安裝：

```bash
git clone https://github.com/codeotter0201/mae-mfe-optimization-skill.git \
  ~/.codex/skills/mae-mfe-optimization
```

完成後重新開啟 Codex session，讓 skill 被重新載入。

### 方式 2：本地開發時用 symlink 掛載

如果你想一邊修改 repo、一邊讓 Codex 讀到：

```bash
ln -s /path/to/mae-mfe-optimization-skill \
  ~/.codex/skills/mae-mfe-optimization
```

這種方式比較適合正在迭代 `SKILL.md` 與 reference 文件時使用。

### 安裝後預期結構

```text
~/.codex/skills/
└── mae-mfe-optimization
    ├── SKILL.md
    └── references/
```

## 在 Codex 裡使用

安裝後，可以直接用 skill 名稱或直接提及主題。示例：

```text
Use mae-mfe-optimization to explain the correct optimization order for this strategy.
```

```text
Use mae-mfe-optimization to decide whether this system should test delayed entry before tightening SL.
```

```text
Apply mae-mfe-optimization and summarize whether this strategy is in a PF, WR, or RF bottleneck.
```

```text
Using mae-mfe-optimization, explain whether trailing stop or fixed TP is more appropriate here.
```

## 適用範圍與非目標

這個 repo 適合：

- 方法論封裝
- 研究導引
- 術語對齊
- 讓 agent 可讀的文件整理

它不包含：

- 交易資料 ETL
- 回測執行程式
- 自動化參數搜尋
- 券商或實盤執行整合

如果你要接到完整生產工作流，通常還需要資料層、排名層、回測層和執行層。

## 來源與致謝

本 repo 是基於公開資料進行的二次整理，主要來源為：

- MAE/MFE 網站：<https://www.maemfe.org/>
- YouTube 頻道：<https://www.youtube.com/channel/UCHQ2g2rm9ml4dzdUfPdXuwQ>

原始資料強調的核心包括 validation、交易內部結構觀察，以及避免過擬合。本 repo 做的是將這些內容重新組織為 Codex skill 可用的格式。

## 非官方聲明

這是非官方 repo，與原作者、原網站、原頻道沒有附屬、背書或維護關係。

若你要再散布或衍生使用，建議保留原始來源連結，並清楚標示哪些部分屬於你的整理或改寫。

## 免責

本 repo 僅供研究、文件整理與 agent 輔助分析使用，不構成投資建議、交易建議或績效保證。
