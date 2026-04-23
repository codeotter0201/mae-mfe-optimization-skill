# 決策 Playbooks

本檔把影片萃取內容轉成可執行的回答流程。使用者問「要怎麼設計/判斷/優化」時，優先讀本檔，再視需要回到 `chapter-details.md` 或 `episode-notes.md`。

## 先判斷是否值得 MAE/MFE 優化

1. 取得 base model，不加 SL/TP、TS/BE、加減碼等資金管理。
2. 用 `practical-triage.md` 的 PF → WR → RF 分流。
3. 若 PF < 0.5 且 EP < 0，優先修策略訊號或檢查交易成本。
4. 若 PF 0.5-5 且交易數足夠，進入 MAE/MFE 分析。
5. 若 PF > 5，先檢查交易數與 Sharpe 是否暗示樣本不足。

輸出時要明確分開：策略訊號問題、進場問題、出場問題、資金管理問題。

## 建立 MAE/MFE 分析底稿

必要欄位：

- `mae`
- `mfe` 或 `mfe_before_mae`
- `g_mfe`
- `mhl`
- `profit_loss`
- `entry_time`
- `direction`
- 時間索引欄位，如 `mae_idx`, `mfe_idx`, `g_mfe_idx`

流程：

1. 用 Daily ATR 或等價波動單位標準化 MAE/MFE。
2. 分勝手/輸手、多單/空單、波動 regime。
3. 畫 MAE vs P/L、MFE vs P/L、MAE CDF、MFE CDF、MAE vs MFE。
4. 先解釋分布，再提出參數候選。

## 設定 Stop Loss

1. 看 MAE 對 P/L，確認勝手與輸手是否能在 MAE 軸上分開。
2. 畫勝手 MAE CDF 與輸手 MAE CDF。
3. 從勝手 MAE Q1/Q2/Q3 與輸手集中區找候選 SL。
4. 用 placement test 比較少數候選值。
5. 檢查 WR、EP、交易數、OOS 穩定性。

避免：

- 直接 grid search 找最高 PF。
- 把 SL 放在交易群集中心。
- 用 MDD 最小化當作主要目標。

## 設定 Take Profit

1. 看 MFE/G_MFE 對 P/L，確認是否存在未落袋獲利。
2. 畫勝手與輸手 G_MFE CDF。
3. 若 G_MFE 高但 P/L 低，檢查 TP、TS、BE 或 Lock Profit。
4. 以分位數產生 TP 候選，而不是密集掃描。
5. 檢查 TP 收緊後 WR、EP、尾部獲利是否被破壞。

MFE 定義：

- 一般 SL/TP 與 placement：預設用 MFE before MAE。
- 動態停損、BE、Lock Profit：改看 Global MFE。

## 做 MAE vs MFE Placement Test

1. 兩軸使用相同比例尺。
2. 先辨識群集型態：向上、向下、向左、向右、多群。
3. 在散布圖上放候選 SL/TP 水平線。
4. 看右上角 Q3 以上位置：
   - MFE 明顯大於 MAE：偏向收緊 TP 或動態出場。
   - MAE 明顯大於 MFE：偏向收緊 SL、延遲進場或方向過濾。
5. 若出現 L 型反比分布，進入 TS/BE/Lock Profit 評估。

## 設計延遲進場

先分清兩種問題：

- Favorable 延遲：用更好的價格進場，壓縮 MAE。
- Adverse 延遲：要求價格先證明方向，過濾失敗訊號。

Favorable：

1. 參考 Win Trade MAE CDF。
2. 初始候選：MAE_Q1 到 `(Q1+Q2)/2`。
3. 上限通常不超過 MAE_Q3。
4. 檢查成交率、MAE 降幅、MFE 損失、EP。

Adverse：

1. 參考 Loss Trade G_MFE Q3 或 Win/Lose MFE 分界。
2. 用觸價條件過濾反彈不足的輸手。
3. 檢查是否提高 WR，但不要讓 EP 大幅下降。

延遲進場失敗訊號：

- MFE 縮水幅度大於 MAE 改善。
- 成交率過低。
- 低 G_MFE 比例上升。
- 參數只在 IS 有效。

## 判斷 Trailing Stop、Breakeven、Lock Profit

1. 改用 Global MFE 與 MHL，不要沿用 MFE before MAE。
2. 先看 MAE vs Global MFE 是否呈 L 型反比分布。
3. 用 Lose Trade MFE 的 Q2 或 `(Q1+Q2)/2` 作為 BE 觸發候選。
4. 用 MHL CDF 或 MHL/G_MFE 決定 TS 追蹤幅度。
5. 若 MHL/G_MFE 小，TS 較適合。
6. 若 G_MFE 高但回吐大，Lock Profit 較適合。
7. 比較固定 TP、BE、TS、Lock Profit 對 WR、EP、PF、RF 的影響。

## 設計加碼與減碼

前提：

- 不用加碼拯救基本體質差的策略。
- 每一碼都要獨立計算風險。
- 加碼應在 SL/TP、延遲進場、動態停損的基本架構穩定後才做。

E1 不利方向加碼：

1. 適用於勝率高但輸一次輸很多的策略。
2. 參考 Win Trade MAE Q1-Q2 作為加碼候選區。
3. Q2-Q3 或 exit signal 可作為減回/停止條件。
4. 嚴格限制最大倉位與總風險。

E2 有利方向加碼：

1. 適用於 MFE 延伸穩定的策略。
2. 參考 Win Trade MFE Q2-Q3 或 Win/Lose MFE CDF 交叉點。
3. 加碼後重新定義該碼 SL，不要讓整體風險失控。

E3 減碼：

1. 在加碼位置與 TP 的 1/2 到 1/3 處先減碼。
2. 或按持有時間、浮盈分位數階梯式平倉。
3. 減碼是損益修飾，不應掩蓋策略訊號問題。

## 避免過擬合

必要檢查：

- 用 MAE/MFE 分布產生候選，再回測驗證，不反過來。
- 每個參數只測少量結構性候選。
- 檢查 IS/OOS、rolling、交易數、方向分組、波動分組。
- 若參數線穿過群集中心、交易數急速下降、或 OOS 崩潰，視為高風險。

回答使用者時，明確說明哪些是「觀察」、哪些是「候選參數」、哪些仍需「回測驗證」。
