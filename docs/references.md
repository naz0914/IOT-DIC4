# References — Hybrid TPA-Agent

> 本參考文獻清單嚴格區分同儕審查學術論文、預印本、官方監管統計報告、權威新聞事件與領域經典專書，所有標題、作者、年份與 DOI 均經查核，未虛構任何文獻。

1. **Securities and Exchange Board of India (SEBI), Department of Economic and Policy Analysis (DEPA).** (2026, August). *Trading Behaviour of Individual Traders in the Equity Derivatives Segment (FY25–FY26)*. Official Regulatory Study, SEBI.
   - **引用數據與邊界**：涵蓋 FY26 共 786 萬名（78.6 lakh）個人衍生品交易者，其中 **87.7%** 出現淨虧損。該數據源於印度股票衍生品市場，用於說明零售高頻短週期交易之普遍虧損風險，不直接等同加密貨幣市場之確切比例。

2. **Koviazin, A., Mudarisov, T., Polyachenko, E., & State, R.** (2026). Reproducibility in the TradingAgents Framework. *Proceedings of the 2026 International Conference on Artificial Intelligence and Fintech (ICAIF '26)*, ACM, pp. 365–370. DOI: [10.1145/3800973.3801029](https://doi.org/10.1145/3800973.3801029).
   - **引用數據與邊界**：揭示 LLM 多代理人框架在 `temperature = 1` 下三個月累積報酬標準差高達 9.0–9.4 個百分點；推動本系統採用 `temperature = 0.0`、數值解耦與多輪一致率評估。

3. **FinForesight Authors.** (2026). FinForesight: Assessing Information Leakage and LLM Financial Prediction Capabilities. *arXiv preprint*.
   - **引用數據與邊界**：評估 22 種金融模型於美股、A 股與港股 27 組時間比較，發現 25 組存在歷史洩漏偏差；加入 Point-in-Time (PIT) 與洩漏過濾後分數差距縮減約 76%。推動本系統設計嚴格 PIT 快照隔離。

4. **Chen, T., & Guestrin, C.** (2016). XGBoost: A scalable tree boosting system. *Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining (KDD '16)*, pp. 785–794.
   - **引用意義**：本系統 Module 2.2 條件勝率分類器之基礎演算法。

5. **arXiv:2407.11786.** (2024). Cryptocurrency Price Forecasting Using XGBoost Regressor and Technical Indicators. *arXiv preprint*. DOI: [10.48550/arXiv.2407.11786](https://doi.org/10.48550/arXiv.2407.11786).
   - **引用意義**：驗證 XGBoost 結合技術指標於加密貨幣價格波動預測之可行性，並凸顯純技術指標缺乏空間結構與風控閘道之局限。

6. **Almeida, J., & Gonçalves, T. C.** (2023/2024). Cryptocurrency market microstructure: a systematic literature review. *Annals of Operations Research*. DOI: [10.1007/s10479-023-05627-5](https://doi.org/10.1007/s10479-023-05627-5).
   - **引用意義**：統整 138 篇加密貨幣微觀結構研究，支撐本系統對買賣價差、盤口深度、Taker 手續費（`0.04%`）與滑點摩擦的模型設計，以及捨棄 `1m` 高雜訊週期的決定。

7. **Brooks, A.** (2012). *Reading Price Charts Bar by Bar: The Technical Analysis of Price Action for the Serious Trader*. John Wiley & Sons.
   - **引用意義**：為 Module 2.1 與 Module 3.1 之價格行為學（Pinbar 被動吸收、S/R Flip、Liquidity Sweep、ChoCh）提供領域理論基礎。

8. **Reuters.** (2025, October). *Crypto markets face severe liquidation event amid volatile trading conditions* (兩日內逾 190 億美元槓桿強制平倉與 BTC 逾 14% 跌幅報導).
   - **引用意義**：作為極端波動與流動性枯竭壓力測試（Stage 3）之具名歷史事件基準。
