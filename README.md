# mae-mfe-optimization-skill

把公開分享的 MAE/MFE 交易分析內容，整理成可供 Codex、Claude Code 等 AI agent 直接載入使用的 skill。

這個 repo 是文件與方法論導向，不是回測引擎、最佳化 API 或下單框架。它的目標是把 MAE/MFE 策略優化的概念、術語和決策流程整理成 agent 可重複使用的知識包。

English version: [README.en.md](./README.en.md)

## 這個 skill 包含什麼

主題聚焦在以 MAE/MFE 為核心的策略優化，包括：

- MAE、MFE before MAE、Global MFE、MHL、Edge Ratio 的定義
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

## 30 秒上手

如果你是第一次讀這份 repo，按這個順序開檔最短路徑：

1. **策略剛回測完，不確定值不值得優化** → `references/practical-triage.md`（PF → WR → RF 三階段分流）
2. **已有具體優化問題（SL/TP、延遲進場、加減碼、動態停損等）** → `references/decision-playbooks.md`（照場景找操作流程）
3. **需要對齊術語或追溯影片來源** → `references/core-concepts.md`、`references/source-map.md`、`references/episode-notes.md`

`SKILL.md` 本身是入口索引與決策樹；細節都在 `references/`。

## Repo 結構

```text
.
├── README.md
├── README.en.md
├── SKILL.md
├── references
│   ├── core-concepts.md
│   ├── practical-triage.md
│   ├── chapter-details.md
│   ├── decision-playbooks.md
│   ├── source-map.md
│   └── episode-notes.md
└── subtitles
    └── maximum-excursion-analysis
        └── INDEX.md
```

### `SKILL.md`

主 skill 定義，包含：

- 觸發條件與使用範圍
- 核心術語
- 五步驟優化主軸
- 章節與決策對照
- 反過擬合原則

### `references/practical-triage.md`

以前置分流 `Profit Factor -> Win Ratio -> Recovery Factor` 判斷策略是否值得進入更深的 MAE/MFE 分析。

### `references/core-concepts.md`

整理最核心的概念層內容，包括：

- MFE before MAE 與 Global MFE 的使用邏輯
- 核心指標速查
- 反過擬合原則

### `references/chapter-details.md`

補充主要主題的展開說明，包括：

- ATR 波動標準化
- 時間面 MAE/MFE 與 Edge Ratio
- Placement Test
- Delayed Entry
- 環境與 regime filter
- Trailing Stop / Breakeven
- 加減碼與帳戶層級風控

### `references/decision-playbooks.md`

把章節內容轉成可直接照著跑的操作流程。遇到「要怎麼設計/判斷/優化 …」類問題時先開這個檔，內含 SL、TP、Placement Test、Delayed Entry、Trailing Stop / Breakeven、加減碼等場景的回答框架。

### `references/source-map.md`

集數 ↔ 章節 ↔ reference 的對照表，含播放清單與每集 YouTube 連結。需要追溯概念出處或確認某段說法來自哪一集時使用。

### `references/episode-notes.md`

從 22 集字幕萃取出的逐集中層筆記，每集列核心問題、使用資料、實務判斷與風險提醒。比 `chapter-details.md` 更貼近原始講解順序，適合逐集比對。

### `subtitles/maximum-excursion-analysis/INDEX.md`

字幕索引（字幕原檔 `.srt` 依 `.gitignore` 不納入版本管理）。僅在本機需要重新萃取內容時使用。

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

重點是優先觀察 MAE、MFE before MAE、Global MFE、MHL、Edge Ratio 這類交易內部結構，而不是只看 MDD、Sharpe 這些表層總績效指標。

## 安裝

此 skill 的 `SKILL.md` 使用標準 frontmatter 格式，**Codex 與 Claude Code 皆可直接載入**。選你用的平台：

| 平台 | skills 目錄 |
| --- | --- |
| Codex | `~/.codex/skills/` |
| Claude Code | `~/.claude/skills/` |

### 方式 1：直接 clone

```bash
# Codex 使用者
git clone https://github.com/codeotter0201/mae-mfe-optimization-skill.git \
  ~/.codex/skills/mae-mfe-optimization

# Claude Code 使用者
git clone https://github.com/codeotter0201/mae-mfe-optimization-skill.git \
  ~/.claude/skills/mae-mfe-optimization
```

完成後重新開啟該平台的 session，讓 skill 被重新載入。

### 方式 2：本地開發時用 symlink 掛載

如果你想一邊修改 repo、一邊讓 agent 讀到：

```bash
# Codex 使用者
ln -s /path/to/mae-mfe-optimization-skill \
  ~/.codex/skills/mae-mfe-optimization

# Claude Code 使用者
ln -s /path/to/mae-mfe-optimization-skill \
  ~/.claude/skills/mae-mfe-optimization
```

這種方式比較適合正在迭代 `SKILL.md` 與 reference 文件時使用。

### 安裝後預期結構

```text
~/.codex/skills/                       ~/.claude/skills/
└── mae-mfe-optimization               └── mae-mfe-optimization
    ├── SKILL.md                           ├── SKILL.md
    └── references/                        └── references/
```

## 呼叫方式

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

完整起手式（策略剛回測完、還不知道瓶頸在哪）：

```text
Use mae-mfe-optimization. 先用 practical-triage.md 判斷這個策略值不值得深入優化，
然後根據瓶頸落在 PF / WR / RF 哪一階段，從 decision-playbooks.md 提出下一個具體動作。
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

原始資料強調的核心包括 validation、交易內部結構觀察，以及避免過擬合。本 repo 做的是將這些內容重新組織為 Codex / Claude Code 可直接載入的 skill 格式。

## 非官方聲明

這是非官方 repo，與原作者、原網站、原頻道沒有附屬、背書或維護關係。

若你要再散布或衍生使用，建議保留原始來源連結，並清楚標示哪些部分屬於你的整理或改寫。

## 免責

本 repo 僅供研究、文件整理與 agent 輔助分析使用，不構成投資建議、交易建議或績效保證。
