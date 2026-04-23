---
name: mae-mfe-optimization
description: |
  MAE/MFE 策略優化手冊 — 理論框架、五步驟流程、評分模型架構。
  Use when:
  - 討論 MAE/MFE 優化理論與方法論
  - 設計 Delayed Entry、SL/TP、TS/BE 參數
  - 建立或理解評分模型（Scoring Model）的各層級公式
  - 規劃策略參數掃描與篩選流程
  - 理解加碼/減碼策略的適用條件
  - 避免過擬合的原則與檢驗標準
  Do NOT use for:
  - 評分→排名→選模型的迭代工作流 API（use strategy-optimization-workflow）
  - TradesAnalyzer API 使用（use trades-analyzer-guide）
  - Trades pipeline 資料處理（use trades-pipeline-guide）
  - StrategyConfig 建立（use strategy-config-builder）
  - 回測提交與結果取得（use backtest-workflow）
triggers:
  - MAE/MFE optimization
  - scoring model
  - delayed entry theory
  - placement test
  - money management tree
  - MMT
  - scaling in
  - scaling out
  - pyramiding
  - anti-martingale
  - overfitting prevention
  - SL/TP placement
  - trailing stop theory
  - breakeven theory
  - MHL
  - edge ratio
  - volatility clustering
  - recovery factor target
  - portfolio construction scoring
  - profit factor triage
  - strategy triage
  - alpha loss
  - strategy inversion
  - win rate bottleneck
  - EP monitoring
  - ATR regime
  - dynamic weight
  - regime detection
  - portfolio weight engine
  - strategy orthogonality
  - correlation weight shrink
---

# MAE/MFE 策略優化手冊

## Global MFE 定義

本手冊使用 **Global MFE**（涵蓋整個持倉生命週期的絕對最高/最低點），而非「MAE 之前的 MFE」。

- **MFE before MAE**：MAE 之前的最大有利幅度 — 反映「最大逆境前的甜頭」
- **Global MFE**：整個持倉週期的最大有利幅度
- 兩者重疊性越低 → 價格在 MAE 後仍大幅回升 → 需要追蹤停損（Ch9）
- 兩者重疊性越高 → 獲利多在 MAE 前出現 → 優先考慮 SL/TP 配置（Ch6）

## 五步驟主軸

| 步驟 | 章節 | 核心問題 |
|------|------|---------|
| A. 觀察波動穩定性 | Ch1, Ch8 | 策略與市場波動的關係是否一致？ |
| B. 重新選擇進場時機 | Ch7 | 能否找到更有利的進場位置？ |
| C. 決定停損與停利 | Ch4-6 | SL/TP 應該放在哪裡？ |
| D. 追蹤停損與損益兩平 | Ch9 | 如何保護已有獲利？ |
| E. 加碼與減碼 | Ch10-11 | 何時乘勝追擊？何時降低曝險？ |

## 優化順序（Ch14 標準流程）

**每一步必須在前一步穩定後才進行：**

| 順序 | 步驟 | 章節 | 主要改善指標 |
|------|------|------|------------|
| 1 | 延遲進場 | Ch7 | Win Rate |
| 2 | 停損止盈調整 | Ch4-6 | Expected Payoff |
| 3 | 環境過濾 | Ch8 | Recovery Factor |
| 4 | 動態停損 | Ch9 | Profit Factor |
| 5 | 加減碼 | Ch10-11 | Sharpe Ratio |
| 6 | 帳戶避險 | Ch13 | 整體風控 |

## 快速決策樹

```
需要什麼？
├── 策略是否值得優化 → references/practical-triage.md（Profit Factor → Win Ratio → Recovery Factor 分流）
├── 理解核心指標定義 → 見下方「核心指標速查」
├── 設計 SL 位置 → Ch4: MAE CDF 分位數法
├── 設計 TP 位置 → Ch5: G_MFE CDF + 獲利回吐率
├── SL/TP 綜合配置 → Ch6: MAE vs MFE 散佈圖 + Placement Test
├── 延遲進場參數 → Ch7: Favorable/Adverse 延遲
├── 環境過濾 → Ch8: 時序圖 + 波動叢聚
├── 動態停損 → Ch9: MHL 分析
├── 加碼策略 → Ch10: E1(不利方向) / E2(有利方向)
├── 減碼策略 → Ch11: E3 通則
├── 評分與篩選 → references/scoring-model.md
└── 避免過擬合 → 見下方「反過擬合原則」
```

## 核心指標速查

| 指標 | 定義 | 用途 |
|------|------|------|
| MAE | 持倉期間最大虧損幅度 | SL 設定依據 |
| MFE (before MAE) | MAE 之前的最大獲利幅度 | 波段 vs 趨勢判斷 |
| G_MFE | 整個持倉週期最大獲利幅度 | TP 設定、獲利潛力評估 |
| MAE_lv1 | 首次到達 MAE 的幅度 | 區別首次回撤與後續更深回撤 |
| MHL | G_MFE 到隨後低點的最大回撤 | 追蹤停損幅度參考 |
| Edge Ratio | MFE(t) / MAE(t) | 進場後 t 個 Bar 的優勢比 |

## 關鍵章節摘要

### Ch4: SL 決策

- 繪製 MAE CDF，分別標示勝手/輸手
- 找出 Q1, Q2, Q3 分位數
- SL 放在能過濾多數輸手、保留大多數勝手的位置
- 收緊 SL 會消耗勝率 → 高勝率是收緊 SL 的前提

### Ch5: TP 決策

- 繪製勝手/輸手的 G_MFE CDF
- 觀察獲利回吐率：`1 - PNL_median / G_MFE_median`
- G_MFE 高但 PNL 偏低 → 出場太晚
- 收緊 TP 也會消耗勝率

### Ch6: SL/TP Placement Test

- MAE vs MFE 散佈圖（兩軸比例尺一致）
- 右上角分析：MFE > MAE 2-5x → 收緊 TP；MAE > MFE 2-5x → 收緊 SL
- **過擬合警告**：SL 切太靠近群集中心 → OOS 崩潰

#### 集群四型態

| 型態 | 特徵 | 對策 |
|------|------|------|
| 向上聚集 | MFE 高，MAE 低 | 專注把握獲利 |
| 向下聚集 | MFE 低，MAE 低 | 收緊 TP，提高周轉率 |
| 向左聚集 | MAE 低，MFE 適中 | 理想分佈，微調即可 |
| 向右聚集 | MAE 高，MFE 適中/低 | 需要延遲進場或方向過濾 |

### Ch7: Delayed Entry

- **Favorable（限價單）**：壓縮 MAE，提升盈虧比
  - 參考 Win Trade MAE 分佈，延遲 < MAE_Q1 到 (Q1+Q2)/2
  - 上限：MAE_Q3
- **Adverse（觸價單）**：過濾失敗訊號，提升勝率
  - 參考 Loss Trade G_MFE Q3
- 兩者交互測試：`Sweep L/S, FAV/ADV`

### Ch9: 動態停損

- **前提**：需要 MAE vs MFE 的 L 型反比分佈
- **BE 設定**：Lose Trade MFE 的 (Q1+Q2)/2 或 Q2 作為觸發價
- **TS 設定**：以 MHL 作為追蹤幅度參考
- **判斷**：MHL/G_MFE 比例小 → TS；比例大 → Lock Profit

### Ch10: 加碼

- **E1 不利方向**：勝率高但輸一次輸很多 → Win MAE Q1~Q2 加碼，Q2~Q3 減回
- **E2 有利方向**：MFE 都較高 → Win MFE Q2~Q3 加碼，或 Win/Lose MFE CDF 交叉處

### Ch11: 減碼

- **E3 通則**：在加碼位置與 TP 的 1/2 到 1/3 處減碼
- 可依持有時間或獲利幅度階梯式平倉

## 反過擬合原則

1. **不要以 MDD 最小化作為優化的「因」** — MDD 變小只是 SL/TP 調整的「果」
2. **使用「交易內」指標（MAE/MFE）而非僅「交易間」指標（MDD、Sharpe）**
3. **加碼不是用來救策略** — 基本體質不好時，加碼只是分攤過擬合負擔
4. **波動是策略的命脈** — 所有優化最終都在回答：策略能否在不同波動環境下穩定表現？

## 參考檔案

- `references/practical-triage.md` — Profit Factor → Win Ratio → Recovery Factor 前置分流決策樹
- `references/scoring-model.md` — 評分模型五層架構與公式
- `references/chapter-details.md` — 各章節完整概念與實現細節
