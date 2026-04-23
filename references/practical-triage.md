# 實務分流決策樹（Practical Triage）

策略回測完成後，先用 **Profit Factor → Win Ratio → Recovery Factor** 三階段判斷策略是否值得進入 MAE/MFE 深度優化。

> 不同策略建構方式（回測時間尺度、回測長度、交易標的數量等）的優化軌跡都不同。以下僅為拋磚引玉的通用框架，實際需根據需求調整。

---

## Phase 1：Profit Factor 分流

```
PF 值
├── < 0.5  → 策略可能有根本問題
├── 0.5~2  → 有基礎，進入 Phase 2 觀察 WR
├── 2~5    → 體質良好，驗證穩定性後進入 Phase 2
└── > 5    → 疑似交易數不足，需檢查
```

### PF < 0.5：策略可能輸交易成本

**Alpha Loss 線診斷**：畫一條斜率 = 負的單位交易成本的線。若策略 payoff 遠低於 Alpha Loss 線 → 考慮**策略倒置**（買變賣、賣變買）。若接近 Alpha Loss → 仍然只是輸在交易成本。

**ATR/ADX 噪音檢查**：觀察策略進出場與波動指標（ATR/ADX）的關係。常見病徵：高波動時進場、低波動時出場 → 被市場噪音影響。

**優先行動**：調整參數、修改策略訊號，而非進入資金管理優化。

> PF < 0.5 且 Expected Payoff < 0 = 策略需要修正的明確訊號。
>
> PF < 0.5 但 EP > 0？可能的情形：贏的幅度非常大但次數極少。若能把輸的小幅度交易過濾掉，有機會得到勝率接近 50% 且只贏大錢的策略。

### PF 0.5~2：有基礎

直接進入 **Phase 2** 觀察勝率。

### PF 2~5：體質良好

**延長回測期間驗證**：固定參數，回測多個區間，觀察每個區間的 PF 是否都在 2~5 左右。若穩定 → 考慮用資金管理穩定到 PF ≈ 3。

進入 **Phase 2**。

### PF > 5：疑似交易數不足

**Sharpe > 0.3 警告**：PF > 5 搭配 Sharpe > 0.3，高度懷疑交易數量不足以支撐統計意義。

**行動**：更積極地提高勝率（多輸一些小 Trade 沒關係），增加交易數量。

---

## Phase 2：Win Ratio 觀察

### PF 0.5~2，WR 40%~60%

**目標**：優先提升勝率到 60%。

優化過程中會遇到 **WR 50% 瓶頸** — 這是常見現象。此階段的重點是確認策略基本體質：
- 策略在未使用資金管理前，勝率是基本體質指標
- 若能從 40% 提到 60%，代表策略有優化空間

達到 60% 後 → 進入 **Phase 3**。

### PF 0.5~5，WR < 40%

**期望驅動型策略**：PF 有一定水準但勝率極低，損益由 Expected Payoff 驅動。

**EP 監控法**：

```
Expected Payoff = WR × Avg.Gain + (1 - WR) × Avg.Loss
```

嘗試提高勝率，但每次調整都監控 EP：
- EP 掉太多 → 碰到勝率瓶頸，停止提升 WR
- 觀察 **Total Net Profit / Expected Payoff** 比值作為健康度診斷

提升到可行的 WR 後 → 進入 **Phase 3**。

### PF 2~5，WR ≥ 60%

**理想情形**。直接進入 **Phase 3**。

**重要約束**：此階段不能使用任何加減碼，只允許 Stop Loss 和 Take Profit。加減碼保留給 Phase 3 用來優化 Recovery Factor。

---

## Phase 3：Recovery Factor 優化

### RF < 8：使用 MAE/MFE 分析三步驟

依序執行，每一步穩定後才進入下一步：

**A. 延遲進場（Delayed Entry）**

透過 MAE/MFE 分析調整 SL/TP，並使用延遲進場壓縮 MAE：
- Favorable：進場價 X → 改為 X - offset 限價單（壓縮 MAE）
- Adverse：進場價 X → 改為 X + offset 觸價單（過濾輸手 MFE 不超過 offset 的交易）
- 可將延遲進場參數與策略參數一起小範圍掃描優化

**B. 加減碼（Scaling）**

透過 MAE/MFE 分析決定加減碼時機：
- 不利方向攤平加碼（Anti-Martingale）
- 有利方向提前減碼（Partial Close）

**C. Trailing Stop / Breakeven**

- 透過 MHL 找到最佳移動停損方式
- 透過 Lose Trade MFE 位置設 Breakeven Stop

透過 A → B → C 應能篩選出 RF ≥ 8 的策略。**注意：MDD 的改善隱含在 RF 優化中，不需要獨立處理 MDD。**

### RF ≥ 8：分流點

```
RF ≥ 8
├── 散戶（Retail）→ 進階驗證後可上場
└── 機構（Institutional）→ 需額外處理 Recovery Period
```

---

## 進階驗證（RF ≥ 8 後）

### 散戶路徑

1. **滾動回測（Rolling Backtest）**：驗證參數在不同時間窗口的穩定性
2. **敏感度分析**：參數微調後觀察績效變化幅度，變化過大 = 過擬合風險
3. **波動壓力測試**：模擬歷史價格波動放大 1.5x、2x，觀察策略反應
4. **Sure-and-Fire 避險**：搭配選擇權 + 極小 TP 避險單，控制極端風險

### 機構路徑

機構每月需攤報表，對 P&L 穩定性要求更高：
- **Recovery Period 優化**：多久能回到前一個淨值高點
- **解法**：單一策略很難優化 RP，需透過**組合多個策略**達成
- 對應 scoring-model.md 的 Layer 4（Portfolio Construction）

---

## 快速參考表

| 狀態 | 行動 |
|------|------|
| PF < 0.5, EP < 0 | 修改策略訊號或參數 |
| PF < 0.5, EP > 0 | 過濾小虧損交易，可能有救 |
| PF < 0.5, 低於 Alpha Loss | 考慮策略倒置 |
| PF 0.5~2 | 進入 WR 觀察 |
| PF 2~5 | 延長回測驗證穩定性 |
| PF > 5, Sharpe > 0.3 | 交易數可能不足，提高 WR |
| WR < 40% | EP 監控法，找勝率瓶頸 |
| WR 40~60% | 優先提升到 60% |
| WR ≥ 60%, PF 2~5 | 理想，進入 RF 優化 |
| RF < 8 | Delayed Entry → Scaling → Trailing Stop / Breakeven |
| RF ≥ 8（散戶）| 滾動回測 + 敏感度分析 + 壓力測試 |
| RF ≥ 8（機構）| 組合策略優化 Recovery Period |
