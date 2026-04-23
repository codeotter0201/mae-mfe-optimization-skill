# 核心概念與原則

## MFE 使用定義

本手冊的預設主體分析使用 **MFE before MAE**，而不是 **Global MFE**。

- **MFE before MAE**：MAE 之前的最大有利幅度 — 反映「最大逆境前的甜頭」
- **Global MFE**：整個持倉週期的最大有利幅度
- 一般的 MAE/MFE 分析、SL/TP 配置、Placement Test，預設以 **MFE before MAE** 為主
- 若進入 **Trailing Stop**、**Breakeven Stop**、**Lock Profit** 階段，才改看 **Global MFE**
- 若 `MFE before MAE` 與 `Global MFE` 重疊性越低，代表價格在 MAE 後仍有明顯延伸，越需要進一步評估動態停損（Ch9）

## 核心指標速查

| 指標 | 定義 | 用途 |
|------|------|------|
| MAE | 持倉期間最大虧損幅度 | SL 設定依據 |
| MFE (before MAE) | MAE 之前的最大獲利幅度 | 波段 vs 趨勢判斷 |
| G_MFE | 整個持倉週期最大獲利幅度 | Trailing Stop / Breakeven / Lock Profit 決策、獲利延伸評估 |
| MAE_lv1 | 首次到達 MAE 的幅度 | 區別首次回撤與後續更深回撤 |
| MHL | G_MFE 到隨後低點的最大回撤 | 追蹤停損幅度參考 |
| Edge Ratio | MFE(t) / MAE(t) | 進場後 t 個 Bar 的優勢比 |

## 反過擬合原則

1. **不要以 MDD 最小化作為優化的「因」** — MDD 變小只是 SL/TP 調整的「果」
2. **使用「交易內」指標（MAE/MFE）而非僅「交易間」指標（MDD、Sharpe）**
3. **加碼不是用來救策略** — 基本體質不好時，加碼只是分攤過擬合負擔
4. **波動是策略的命脈** — 所有優化最終都在回答：策略能否在不同波動環境下穩定表現？
