# 影片來源對照

本檔用來把 `subtitles/maximum-excursion-analysis/` 的原始字幕對應到 skill references。需要追溯來源時，先看本檔，再讀字幕索引或指定 `.srt`。

字幕索引：`subtitles/maximum-excursion-analysis/INDEX.md`

## 使用原則

- 回答使用者問題時，優先讀 `SKILL.md` 與既有 references。
- 需要追溯影片脈絡、確認術語來源、或補章節細節時，再讀本檔。
- 不要把 `.srt` 全量載入 context；只讀對應集數或用 `rg` 搜關鍵字。

## 集數到主題

| 集數 | YouTube ID | 主題 | 主要 reference | 產出用途 |
| --- | --- | --- | --- | --- |
| 01 | `6ubbQKXfjGQ` | 認識 MAE/MFE、最大幅度分析基礎 | `core-concepts.md`, `chapter-details.md` Ch1-Ch2 | 定義、欄位、基準線 |
| 02 | `2-ofNf_I580` | MAE 對 Profit/Loss 圖 | `chapter-details.md` Ch4 | 停損分析 |
| 03 | `4WhvyVidfps` | Profit/Loss 分布與失效情形 | `practical-triage.md` | 策略體質分流 |
| 04 | `SfuQrIlchY0` | MAE 累計圖 | `chapter-details.md` Ch4 | SL 分位數法 |
| 05 | `-qJOm9jrrXQ` | MFE 重新定義、MFE before MAE | `core-concepts.md` | MFE 使用邊界 |
| 06 | `TMFoR1xALi8` | MFE 對 Profit/Loss 分布圖 | `chapter-details.md` Ch5 | TP 與獲利回吐 |
| 07 | `IPRLQ4j0LMM` | MAE 對 MFE 分布圖 | `chapter-details.md` Ch6 | Placement Test |
| 08 | `niMUYUCtZC8` | SL/TP 四大區塊 1 | `chapter-details.md` Ch6 | 分布型態判讀 |
| 09 | `A12ztMoMG6E` | SL/TP 四大區塊 2 | `chapter-details.md` Ch6 | 進出場品質評估 |
| 10 | `Rp4jiGNZJ0U` | 延遲進場 1：降低 MAE、提高 MFE | `chapter-details.md` Ch7 | Delayed Entry 起手式 |
| 11 | `k6P1XdtMwVE` | 延遲進場 2：消滅 MAE 小的交易 | `chapter-details.md` Ch7 | Adverse/Favorable 分流 |
| 12 | `aiEgzhvEV2w` | 延遲進場 3：MFE 可能縮水 | `chapter-details.md` Ch7 | 延遲進場副作用 |
| 13 | `_h1DBKAVUu8` | 延遲進場 4：MFE 可能歸零 | `chapter-details.md` Ch7 | 延遲過度風險 |
| 14 | `2bdpXS-7ZOg` | 設定 SL/TP 1：直接優化陷阱 | `core-concepts.md`, `chapter-details.md` Ch6 | 反過擬合原則 |
| 15 | `TmdB1GRYD3o` | 設定 SL/TP 2：Placement Test | `chapter-details.md` Ch6 | 水平測試流程 |
| 16 | `k6avMDilAcE` | 不利方向加減碼與 Win Trade MAE | `chapter-details.md` Ch10-Ch11 | E1 加減碼 |
| 17 | `0MkqWJoLyJE` | Trailing Stop 與 MHL | `chapter-details.md` Ch9 | 動態停損 |
| 18 | `KZLMF2hgfCA` | 有利方向加減碼與 Win Trade MFE | `chapter-details.md` Ch10-Ch11 | E2/E3 加減碼 |
| 19 | `5TMWBd_EYPA` | 損益兩平停損 1：MFE 均勻分布 | `chapter-details.md` Ch9 | Breakeven 觸發 |
| 20 | `Hh6QVbWt36o` | 損益兩平停損 2：MFE 對 MHL | `chapter-details.md` Ch9 | BE/TS/Lock Profit 選擇 |
| 21 | `zwbjMqBnM7M` | 損益兩平停損 3：Lock Profit | `chapter-details.md` Ch9 | 鎖利規則 |
| 22 | `ydOybfYI9WM` | MAE/MFE 時序圖與 Daily ATR | `chapter-details.md` Ch1, Ch8 | 波動穩定性與環境過濾 |

## 字幕查找提示

常用查找：

```bash
rg -n "MFE before MAE|Global MFE|MHL|延遲進場|損益兩平|追蹤停損|水平測試" subtitles/maximum-excursion-analysis
```

若要確認特定集數，先從檔名前綴定位，例如：

```bash
rg -n "關鍵字" "subtitles/maximum-excursion-analysis/17 - 追蹤停損與 MHL.srt"
```
