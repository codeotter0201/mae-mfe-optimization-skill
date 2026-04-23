# MAE/MFE Optimization Skill

English: A reusable trading-analysis skill for MAE/MFE-based strategy optimization.

中文：這是一套可重複使用的交易分析 skill，聚焦於以 MAE/MFE 方法優化策略。

## What This Repository Does / 這個 repo 在做什麼

English: This repository packages an agent-readable playbook for analyzing and improving trading strategies with:

中文：這個 repo 提供一套可供 agent 讀取與使用的方法論，用來分析與優化交易策略，重點包括：

- MAE / MFE / Global MFE
- delayed entry / 延遲進場
- stop-loss / take-profit placement / 停損停利配置
- trailing stop / breakeven logic / 移動停損與保本邏輯
- scaling in / scaling out / 加碼與減碼
- overfitting prevention heuristics / 避免過擬合的實務 heuristics

## Who This Is For / 適用對象

English: This repository is for people who:

中文：這個 repo 適合以下使用者：

- analyze backtest results with MAE/MFE metrics / 用 MAE/MFE 指標分析回測結果
- design entry/exit rules from excursion distributions / 從 excursion 分佈設計進出場規則
- want a reusable methodology rather than a broker- or platform-specific workflow / 想要可重用的方法論，而不是綁定特定平台流程

## What’s Inside / 內容結構

- `SKILL.md`: core skill definition and decision framework / 核心 skill 定義與決策框架
- `references/practical-triage.md`: PF -> WR -> RF triage / PF -> WR -> RF 的實務分流
- `references/chapter-details.md`: chapter-by-chapter methodology details / 章節式方法細節
- `references/README.md`: public references, authorship notes, and boundary definition / 公開參考資料、作者綜述與公開邊界說明

## Usage / 使用方式

English: Install or copy this skill into your local skills directory, then invoke it when working on:

中文：將這個 skill 安裝或複製到本地 skills 目錄後，可在以下情境呼叫使用：

- MAE/MFE optimization / MAE/MFE 優化
- delayed entry design / 延遲進場設計
- stop-loss / take-profit placement / 停損停利配置
- trailing-stop / breakeven analysis / 移動停損與保本分析

Example prompts / 範例提示：

- `Analyze this strategy using MAE/MFE optimization.`
- `Help me design delayed entry offsets from MAE distributions.`
- `Review this SL/TP placement using MAE vs Global MFE.`

## Scope / 範圍

English: This repository focuses on methodology and decision frameworks.

中文：這個 repo 專注在方法論與決策框架。

It does not cover / 不涵蓋：

- backtest execution / 回測執行
- broker integration / broker 整合
- production trade routing / 生產環境下單路由
- proprietary internal APIs / 專有內部 API

## Methodology Notes / 方法備註

- This repository uses **Global MFE** as the default MFE definition unless stated otherwise. / 除非另有說明，這個 repo 預設使用 **Global MFE** 定義。
- Several thresholds and formulas are practical heuristics for research workflows, not universal truths. / 部分門檻與公式屬於研究流程中的實務 heuristics，並非普世定律。
- The core skill materials are currently written primarily in Traditional Chinese. / 核心 skill 內容目前仍以繁體中文為主。

## Public References / 公開參考資料

- [MAE/MFE official site](https://www.maemfe.org/)
- [MAE/MFE YouTube channel](https://www.youtube.com/channel/UCHQ2g2rm9ml4dzdUfPdXuwQ)

See [references](./references/) for source categories, interpretation notes, and public-repo boundaries.

## Disclaimer / 免責聲明

This repository is for research and educational purposes only. It is not financial advice.

本 repository 僅供研究與教育用途，不構成投資建議。

## License

MIT
