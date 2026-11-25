# Abstract — Hybrid Technical & Price Action Agent (Hybrid TPA-Agent)

> **狀態聲明**：本摘要所載之效能數字（如回撤目標 `≤ 12%`、預期獲利因子 `1.94`、一致率 `> 95%`）均為**研究假說與預期目標（Expected Outcomes）**，尚未宣稱為已完成之實測成果。

---

## 1. A–F 結構化摘要剖析 (Structured Abstract Breakdown)

| 段落 | 核心問題 | Hybrid TPA-Agent 內容與實證依據 |
| :--- | :--- | :--- |
| **A. Attention Getter** | 為何此問題現在很重要？ | 短線加密貨幣與衍生品交易面對高度微觀雜訊、情緒偏誤與劇烈波動衝擊（如 Reuters 2025 年 10 月報導加密貨幣市場兩日強制平倉逾 190 億美元）。SEBI（2026）對 FY26 786 萬名個人衍生品交易者的統計顯示，高達 **87.7%** 出現淨虧損，且持續交易五年皆獲利者僅 **0.5%**，證明交易決策輔助系統的核心痛點在於**風險約束與決策可靠性**，而非單純產生更多交易訊號。 |
| **B. But (Challenge)** | 現有方法有何未解缺口？ | 傳統技術指標嚴重滯後且缺乏不連續空間座標；純機器學習將 K 線視為均勻切片，在低流動性假突破中易誤判；而純 LLM 金融代理人雖能進行語意推理，卻面臨嚴重的生成隨機性（Koviazin et al., 2026 指出在 `temperature=1` 下三個月累積報酬標準差達 `9.0–9.4` 個百分點）、數值計算幻覺，以及歷史資訊洩漏導致回測虛高（FinForesight, 2026 顯示嚴格時點控制可使分數差距縮減 `76%`）。 |
| **C. Cure (Proposed Solution)** | 提出什麼解決方案？ | 本研究提出 **Hybrid TPA-Agent**，一個「**數值運算、機率估計、語意推理與程式化風控徹底解耦**」的對話式/系統化短線交易輔助代理人，將多模組整合設計為可量化、可否證的功能分工假說。 |
| **D. Development (Method)** | 系統如何整合運作？ | 採四層八模組架構：(1) 確定性雙指針演算法結合 Volume Profile HVN 標記供需區與 S/R Flip；(2) 校準後之 XGBoost 分類器估計條件勝率 $W$ 並進行初篩；(3) LLM 僅接收唯讀 JSON 與價格行為知識庫（Pinbar、Liquidity Sweep、ChoCh）執行低隨機性（`temperature=0.0`）CoT 驗證；(4) 風控引擎以 ATR 停損、幾何 $RR \ge 2.0$、含成本期望值 $E$、強制 `WAIT/NO_TRADE` 機制與三級降級協定輸出受人工覆核約束之決策報告。 |
| **E. Experiments (Evaluation)** | 如何嚴謹驗證系統？ | 以 `BTC/USDT` 與 `ETH/USDT`（`5m`/`15m`）為標的，對比 Baseline 1 (`EMA+RSI`)、Baseline 2 (`Pure XGBoost`)、Baseline 3 (`Pure LLM`) 與 Ablation (`S&D + XGBoost`)。驗證分為四階段：時序切分測試、嚴格 Point-in-Time（PIT）控制、極端行情壓力測試（2025-10 清算事件）與雜湊鎖定前瞻模擬；並納入手續費（Taker `0.04%`）、滑點、多輪一致率及 `WAIT` 反事實結果評估。 |
| **F. Findings (Expected Outcomes)** | 預期貢獻與學術價值？ | 預期建立可複現的開源 Spec-Driven 研究原型，將最大回撤（MDD）抑制目標設為 **12% 內**並維持 **> 95%** 決策一致率；更重要的是，透過消融與反事實分析釐清 LLM 在金融決策中的適用邊界與工程代價。 |

---

## 2. 繁體中文精煉學術摘要（約 300 字）

短線交易與日內衍生品投資長期受限於高頻市場雜訊、情緒偏誤與極端流動性風險。印度證券交易委員會（SEBI, 2026）報告指出 FY26 高達 87.7% 個人衍生品交易者出現虧損，顯示交易輔助系統須以風險控制與決策可靠性為核心。然而，傳統技術指標缺乏空間結構表達，純機器學習模型在假突破與概念飄移下容易失效，而純大型語言模型（LLM）交易代理人則面臨生成隨機性高、數值幻覺與歷史資訊洩漏等問題。為此，本研究提出 Hybrid TPA-Agent（Hybrid Technical & Price Action Agent），建立「數值幾何計算、機率估計、語意推理與程式化風控徹底解耦」之四層八模組短線交易輔助系統。系統首先以確定性演算法結合 Volume Profile 高成交量節點標記未回測供需區與支撐壓力互換狀態；再由機率校準之 XGBoost 模型評估條件勝率並進行初篩；隨後將結構化 JSON 與價格行為知識庫交由 LLM 執行低溫度思維鏈（CoT）情境驗證；最終由確定性風控引擎依據動態 ATR 停損、幾何風險報酬比（$RR \ge 2.0$）與含成本正數學期望值把關，並納入 `WAIT/NO_TRADE` 不交易機制、人工覆核及三級異常降級。實驗規劃採用 BTC/USDT 與 ETH/USDT 之 5m/15m 資料，在統一交易成本（0.04% Taker 手續費與滑點）下，透過時序切分、嚴格時點控制（Point-in-Time）、極端行情壓力測試及前瞻模擬，與傳統指標、純 ML、純 LLM 及消融模型進行比較。本研究旨在建立可驗證之規格驅動（OpenSpec）研究原型，預期量化各模組對淨績效、決策一致性與回撤控制（目標 12% 內）之實質貢獻，並界定 LLM 於金融決策之適用邊界。

---

## 3. IEEE Systems Journal Style Abstract (Single-Paragraph English)

**Title—** *Hybrid TPA-Agent: A Decoupled Data-Grounded and Risk-Gated AI Agent for Reliable Short-Term Cryptocurrency Trading Support*

**Abstract—** Short-term cryptocurrency and derivatives trading exposes market participants to severe microstructure noise, extreme liquidation cascades, and cognitive biases. Empirical regulatory evidence from the Securities and Exchange Board of India (SEBI, 2026) indicates that 87.7% of individual equity derivatives traders incurred net losses in FY26, underscoring that decision-support systems must prioritize risk containment and decision reliability over sheer signal volume. However, conventional technical indicators suffer from lag and lack continuous spatial representation, standalone machine learning classifiers fail to capture contextual price structure under shifting liquidity regimes, and pure large language model (LLM) trading agents exhibit substantial stochastic variance across decoding runs, numerical hallucination, and vulnerability to historical data leakage. To address these limitations, this paper proposes Hybrid TPA-Agent, a decoupled four-tier, eight-module trading decision-support architecture that strictly separates deterministic spatial computation, calibrated probabilistic estimation, semantic contextual reasoning, and programmatic risk enforcement. Specifically, a deterministic engine identifies unmitigated supply and demand zones alongside Volume Profile high-volume nodes and support/resistance flips, after which a probability-calibrated XGBoost classifier estimates conditional rebound or breakout win probability to filter low-confidence setups. Passing candidates are evaluated by an LLM planner constrained to read-only structured JSON inputs and a codified Price Action knowledge base (Pinbar absorption, liquidity sweeps, and change of character) under zero-temperature Chain-of-Thought decoding. Finally, a deterministic risk engine computes dynamic ATR-buffered stop-loss and opposing-zone take-profit levels, enforces geometric risk-to-reward ($RR \ge 2.0$) and cost-adjusted positive expectancy thresholds, logs `WAIT/NO_TRADE` abstentions for counterfactual auditing, and applies a three-level latency/outage degradation protocol under human-in-the-loop oversight. The planned four-stage evaluation framework—spanning chronological splits, strict point-in-time isolation, extreme volatility stress testing (including the October 2025 $19 billion liquidation event), and hash-committed forward simulation on 5-minute and 15-minute `BTC/USDT` and `ETH/USDT` datasets—compares the proposed agent against rule-based, standalone ML, standalone LLM, and ablated baselines under explicit 0.04% taker fees and slippage. By converting architectural hypotheses into testable OpenSpec requirements, Hybrid TPA-Agent establishes a reproducible foundation for quantifying whether functional decoupling improves decision consistency, drawdown containment (targeted within 12%), and verifiable AI safety boundaries in short-term financial markets.

**Index Terms—** Conversational and domain-specific AI agents, algorithmic trading, supply and demand zones, XGBoost probability calibration, large language models, point-in-time evaluation, risk management, spec-driven development.
