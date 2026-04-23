# References Notes / 參考資料說明

English: This folder mixes public source concepts, author synthesis, and implementation notes.

中文：這個資料夾同時包含公開概念來源、作者整理，以及方法實作層級的說明。

## 1. Source Concepts / 概念來源

These are the public trading-analysis concepts this repository builds on:

本 repo 所建立於其上的公開交易分析概念包括：

- MAE / MFE analysis
- stop-loss / take-profit placement
- delayed entry
- trailing stop / breakeven logic
- scaling and position management

Public references / 公開參考：

- [https://www.maemfe.org/](https://www.maemfe.org/)
- [https://www.youtube.com/channel/UCHQ2g2rm9ml4dzdUfPdXuwQ](https://www.youtube.com/channel/UCHQ2g2rm9ml4dzdUfPdXuwQ)

## 2. Author Synthesis / 作者整理

A substantial portion of this repository is **author synthesis**:

這個 repo 有相當大一部分屬於 **作者整理與延伸**：

- decision trees / 決策樹
- optimization order / 優化順序
- practical thresholds / 實務門檻
- comparative heuristics / 比較與判斷 heuristics

These are not presented as universal laws. They are working heuristics intended for research and strategy iteration.

這些內容不是普世定律，而是用於研究與策略迭代的工作型 heuristics。

## 3. Implementation Notes / 實作備註

Some documents include implementation-oriented guidance, for example:

有些文件會包含偏實作取向的備註，例如：

- why Global MFE is used as the default definition / 為何預設使用 Global MFE
- why median is preferred for trade-level excursion metrics / 為何交易內 excursion 指標偏好使用中位數
- why certain thresholds are treated as practical rather than absolute / 為何某些門檻被視為實務值而非絕對值

## Public Repo Boundary / 公開 repo 邊界

This public version intentionally avoids:

這個公開版本刻意避免包含：

- proprietary code references / 專有程式碼引用
- internal package names / 內部 package 名稱
- organization-specific workflow dependencies / 組織特定 workflow 依賴
- broker, routing, or production deployment details / broker、路由或生產部署細節

Where a document describes a formula or workflow, read it as a portable design pattern rather than a binding software API.

若文件描述公式或流程，應將其視為可移植的設計模式，而不是綁定特定系統的 API 合約。
