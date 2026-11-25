# Introduction — Hybrid TPA-Agent (G–L / A–F Refactored)

> 本章依循 **DIC3 / 期末專題提案規範（A–F 六段結構）** 與 **2020 A-Z SOP（G–L）** 進行學術化重構，修正原提案文件章節編號重複問題，並建立「文獻限制 → 設計決策 → 可驗證貢獻」之嚴格對應鏈。

---

## A. Attention Getter — 為何這個問題現在至關重要？

現代加密貨幣衍生品與短線當沖市場具備 24/7 連續交易、高槓桿與高度非平穩性（Non-stationarity），零售與短線交易者在海量盤面訊號與劇烈波動下極易陷入認知過載與情緒偏誤。印度證券交易委員會（SEBI）於 2026 年 8 月發布之實證報告《*Trading Behaviour of Individual Traders in the Equity Derivatives Segment*》分析了 FY26 涵蓋的 786 萬名（78.6 lakh）個人交易者帳戶，揭露了極為嚴峻的市場現實：高達 **87.7%** 的個人衍生品交易者在該年度出現淨虧損，且過去研究亦顯示五年連續交易者中僅 **0.5%** 能每年穩定獲利，前兩年皆虧損者隔年續虧比例更達 **90%–92%**。

在加密貨幣市場中，極端流動性衝擊更加劇了無紀律交易的毀滅性後果。根據 Reuters 於 2025 年 10 月報導，加密市場在兩日內發生劇烈拋售，BTC 跌幅逾 14%，全市場槓桿部位強制平倉規模超過 **190 億美元**。上述監管統計與極端事件共同指出一個核心命題：**在高度不確定性的金融市場中，增加交易訊號數量與累積主觀交易經驗並不自動帶來穩定獲利；真正稀缺且決定生存率的，是具備嚴格風險約束、可追溯證據與明確「不交易（Abstention）」準則的決策可靠性系統。**

---

## B. Background — 應用領域、使用者與技術脈絡

本研究聚焦於高流動性加密貨幣（`BTC/USDT`、`ETH/USDT`）之日內短線交易輔助（`5m` 與 `15m` 時間週期），目標使用者為需要兼顧即時盤面判讀、嚴格倉位風控與決策可稽核性之短線交易員與量化研究人員。

技術演進上，金融決策輔助工具大體經歷三代典範轉移：
1. **第一代（規則型技術指標系統）**：以 EMA 交叉、RSI、MACD 等封閉式公式產生進出場訊號，具備 100% 確定性，但無法感知價格的空間結構（如機構訂單密集區與未回測供需區）。
2. **第二代（統計與機器學習模型）**：透過 XGBoost 等梯度提升樹整合多維技術與微觀結構特徵（Chen & Guestrin, 2016；arXiv:2407.11786），捕捉非線性報酬關係，但仍多將 K 線視為均勻時間切片。
3. **第三代（LLM 多代理人架構）**：以 TradingAgents 等框架為代表，利用大語言模型進行多空辯論與宏觀/新聞情境推理，卻因缺乏確定性數值邊界與時點隔離而面臨可靠性危機。

---

## C. Problem Statement — 真正需要解決的核心問題

本研究並非試圖建構另一個宣稱「預測漲跌必勝」的黑箱模型，而是正面解決以下可操作的系統工程問題：

> **在高微觀雜訊與極端波動的短線交易環境中，如何設計一個將「空間幾何計算、條件機率估計、價格行為語意推理、程式化風控」解耦的 Domain AI Agent，使其在同一份嚴格時點控制（Point-in-Time）市場快照下，穩定產出具備正數學期望值、零數值幻覺且可稽核的 `APPROVED` 或 `WAIT/NO_TRADE` 決策？**

---

## D. Limitations of Existing Approaches — 現有方法的具體技術限制與設計對應

我們將文獻揭露的四大具體技術限制，直接對映至本系統的架構設計決策：

| 既有方法之具體技術限制 | 實證文獻來源 | 本研究對應之設計決策 (Design Decision) |
| :--- | :--- | :--- |
| **L1. 傳統指標滯後且缺乏空間座標**：均線與震盪指標無法表徵供需失衡區間與 Volume Profile 成交量節點，導致震盪盤頻繁出現低風報比假訊號。 | Brooks (2012); Almeida & Gonçalves (2023/2024) | 設計 **Layer 2.1 `Deterministic S&D Zone Engine`**，以 $\mathcal{O}(N)$ 雙指針極值與 $\mathcal{O}(N \log B)$ Volume Profile HVN 精確鎖定未回測供需區塊與 `S/R Flip` 狀態。 |
| **L2. 純 ML 缺乏語意脈絡與機率校準意識**：純 XGBoost 在低波動量縮突現大陽線時易誤判（Volume Trap），且未經校準的分類分數直接代入期望值公式將導致風控失真。 | arXiv:2407.11786; 本研究微觀分析 | 設計 **Layer 2.2 `ML Win-Rate Engine`** 搭配 Platt/Isotonic 機率校準（Brier Score 檢核），並將通過初篩之候選交由 **Layer 3 `LLM CoT Agent`** 結合 Price Action 知識庫過濾假突破。 |
| **L3. 純 LLM 生成隨機性與數值幻覺**：在 `temperature=1` 下，TradingAgents 類架構三個月累積報酬標準差達 `9.0–9.4` 個百分點，且 LLM 進行浮點數停損/倉位計算不可靠。 | Koviazin et al. (ACM ICAIF '26, DOI: 10.1145/3800973.3801029) | 實施 **數值與語意徹底解耦**：LLM 固定 `temperature=0.0` 且僅讀取唯讀 JSON 執行結構證據驗證；所有幾何座標、$RR$、期望值 $E$ 與倉位全數交由確定性 Python 風控引擎計算。 |
| **L4. 歷史資訊洩漏與無紀律開倉**：FinForesight 評估 22 種金融模型時發現 27 組比較中有 25 組存在歷史記憶洩漏，PIT 控制後評估落差縮減 `76%`；同時零售高虧損顯示缺乏強制不交易機制。 | FinForesight (2026); SEBI (2026) | 於 **Layer 1.2** 強制執行 `bar.close_time <= t` 之 PIT 快照隔離；於 **Layer 4.1** 建立雙門檻強制 `WAIT/NO_TRADE` 閘道並追蹤被拒絕訊號的反事實後續表現。 |

---

## E. Proposed Innovation — 技術差異與方法創新

相較於既有僅具備單一功能（僅劃線、僅做 ML 分類、或僅讓 LLM 自由生成買賣建議）的系統，**Hybrid TPA-Agent** 的核心差異在於**系統層級的功能分工與可否證架構**：

1. **三階段串聯而非平權投票**：確定性空間引擎提供無雜訊幾何邊界 $\rightarrow$ 校準 XGBoost 以 $W < W_{\min}$ 快速短路淘汰低勝率雜訊（`< 20 ms` 完成前兩層）$\rightarrow$ LLM 僅針對高機率候選執行語意結構複核，兼顧推論成本與 `< 1.5 s` 端到端延遲預算。
2. **主觀 Price Action 規則之結構化與可重現化**：將操盤手經驗中之 `Pinbar 被動吸收`、`Liquidity Sweep`、`ChoCh` 與 `S/R Flip` 轉譯為可版本控管的知識庫與 OpenSpec 驗收情境，避免純文字 Prompt 漂移。
3. **「不交易 (`WAIT/NO_TRADE`)」作為一等研究對象**：明確記錄每一次拒絕原因，並在實驗中比較被拒絕訊號的後續價格走勢，防止將單純減少交易次數誤判為模型能力提升。

---

## F. Research Contributions — 可驗證與可量測之研究貢獻

本研究提出四項明確對應至 OpenSpec 規格與實驗指標的預期貢獻（所有實證增益待實驗驗證，不預先宣稱結果）：

1. **可重現之解耦式交易代理人原型與 OpenSpec 規格（`RQ3`）**：建立涵蓋五大模組規格與 `DESIGN.md` 的規格驅動（SDD）架構，目標使重複執行決策一致率達到 **> 95%**、程式化數值/風控違規率降至 **0%**（對比純 LLM 基線預期 `< 60%` 一致率）。
2. **客觀供需區與校準機率融合機制（`RQ1`, `RQ2`）**：透過消融實驗（`Pure XGBoost` vs. `S&D + XGBoost` vs. `Hybrid TPA-Agent`），以 ROC-AUC、Brier Score、Profit Factor 與 Sharpe Ratio 量化空間特徵與語意複核之各自增量貢獻。
3. **不交易機制之機會成本與風險價值量化框架（`RQ4`）**：建立 `WAIT/NO_TRADE` 反事實追蹤基準，實證檢驗雙門檻濾網所避免之虧損是否足以補償錯失之獲利機會，目標在扣除 0.04% Taker 手續費與滑點後將 MDD 控制於 **12% 內**。
4. **嚴格 PIT 與極端行情壓力測試協議（`RQ5`）**：整合時序切分、FinForesight 風格 PIT 隔離、2025 年 10 月 190 億美元清算壓力測試與雜湊鎖定前瞻模擬，實證界定 LLM 在短線量化交易中的適用條件與失效邊界。

---

## G. Roadmap — 後續章節組織 (L: List Organization)

- **Related Work ([`docs/related-work.md`](./related-work.md))**：涵蓋 50% AI 技術與 50% 金融領域知識之文獻回顧、Article Summary Table 及技術比較矩陣。
- **Deep Research ([`docs/deep-research.md`](./deep-research.md))**：五大核心思維模式、三大專業爭論（Trade-offs）與十大深度理解問題。
- **Technical Design & OpenSpec ([`DESIGN.md`](../DESIGN.md), [`openspec/`](../openspec/changes/refactor-hybrid-tpa-agent/proposal.md))**：四層八模組架構、數學公式、虛擬碼、資料契約與可執行驗收規格。
