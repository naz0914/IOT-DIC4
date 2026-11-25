# Related Work — Hybrid TPA-Agent (M–O Refactored)

> 本章依循 **期末專題提案作業規範（50% AI Technology + 50% Domain Knowledge）** 與 **2020 A-Z SOP（M–O）** 編撰，所有文獻均經核實，未編造任何 DOI 或不存在之實驗比較。

---

## 1. 文獻分類回顧：AI 技術與領域知識並重 (50% AI + 50% Domain)

### 1.1 AI Technology（50%）：梯度提升樹、LLM 多代理人與評估防洩漏

1. **梯度提升樹與加密貨幣預測 (`[1]`, `[2]`)**  
   Chen & Guestrin (2016) 提出 **XGBoost** (`[1]`)，透過二階泰勒展開近似與稀疏感知分裂演算法，在表格型結構化特徵上具備高效能與低推論延遲（$\mathcal{O}(T \times D)$）。後續研究如 *Cryptocurrency Price Forecasting Using XGBoost Regressor and Technical Indicators* (`[2]`, DOI: `10.48550/arXiv.2407.11786`) 將 XGBoost 應用於加密貨幣技術指標（EMA、MACD、RSI）建模。  
   - **方法假設與限制**：此類方法將 K 線序列視為均勻採樣特徵向量，缺乏對不連續空間價格區塊（供需失衡區）與盤口流動性脈絡的語意理解；在震盪盤整或量縮假突破時，單靠指標特徵易產生高偽陽性訊號。  
   - **與本系統關聯**：Hybrid TPA-Agent 保留 XGBoost 低延遲非線性擬合優勢（Module 2.2），但改以確定性供需區幾何特徵作為輸入並強制進行機率校準（Brier Score），再以 LLM 執行上下文二次過濾。

2. **LLM 金融多代理人架構與重現性危機 (`[3]`)**  
   以 TradingAgents 為代表的 LLM 多代理人系統透過角色分工（分析師、研究員、風控官）進行文本辯論與交易建議生成。然而，Koviazin, Mudarisov, Polyachenko & State (2026) 於 ACM ICAIF '26 發表之 *Reproducibility in the TradingAgents Framework* (`[3]`, DOI: `10.1145/3800973.3801029`) 實證指出：在 `temperature = 1` 下，GPT-4o 與 Qwen3 等模型在三個月測試期間的累積報酬標準差高達 **9.0 至 9.4 個百分點**，且即使嘗試調整溫度，部分推理模型仍可能忽略外部取樣參數。  
   - **方法假設與限制**：純 LLM Agent 直接從非結構化文本產出交易動作與數值參數，受生成隨機性與數值幻覺影響甚鉅，單一隨機種子的回測表現無法證實架構優越性。  
   - **與本系統關聯**：本系統拒絕讓 LLM 直接計算價格、勝率或倉位，強制設定 `temperature = 0.0` 與模型版本鎖定，並將重複執行一致率（目標 `> 95%`）列為 `RQ3` 核心指標。

3. **歷史資訊洩漏與時點控制 (`[4]`)**  
   *FinForesight* (2026, `[4]`) 在美股、A 股與港股四類金融預測任務上評估 22 種 LLM 與研究工作流，揭露了嚴重的評估偏差：在 27 組時間區間比較中，有 **25 組**因預訓練參數記憶或無約束檢索導致歷史題目分數虛高；引入嚴格 **Point-in-Time (PIT)** 與 LLM 洩漏過濾後，歷史與即時預測平均分數差距縮減 **76%**。  
   - **方法假設與限制**：FinForesight 主要針對基本面、事件方向與排名等較長週期預測任務，與 `5m/15m` 高頻價格結構不同，但其 PIT 驗證準則對任何金融 LLM 系統皆具普遍適用性。  
   - **與本系統關聯**：Module 1.2 強制實作 `bar.close_time <= t` 快照隔離，並將前瞻雜湊鎖定模擬納入 `RQ5` 四階段驗證。

### 1.2 Domain Knowledge（50%）：微觀市場結構、價格行為學與交易摩擦

4. **加密貨幣市場微觀結構與交易成本侵蝕 (`[5]`)**  
   Almeida & Gonçalves (2023/2024) 于 *Annals of Operations Research* 發表之系統性文獻回顧 *Cryptocurrency market microstructure: a systematic literature review* (`[5]`, DOI: `10.1007/s10479-023-05627-5`) 統整 138 篇研究，深入剖析交易所價格發現、買賣價差、訂單簿深度與交易摩擦（手續費與滑點）。  
   - **領域限制**：加密貨幣日內微觀雜訊極高且流動性跨時段差異顯著，若高頻策略未納入手續費與滑點模型，回測正報酬極易在實盤中被交易成本完全吞噬。  
   - **與本系統關聯**：Hybrid TPA-Agent 放棄高雜訊的 `1m` 週期（鎖定 `5m/15m`），並在風控與回測層統一扣除 `0.04%` Taker 手續費與滑點。

5. **主觀價格行為學與客觀支撐阻力演算法 (`[6]`)**  
   Brooks (2012) 在 *Reading Price Charts Bar by Bar* (`[6]`) 系統化闡述了裸 K 價格行為學（如 Pinbar 被動吸收、支撐壓力互換 S/R Flip、流動性陷阱與次級結構轉折）。既有開源與量化支撐阻力演算法（如 Local Extrema、KDE 核密度估計、ZigZag、Pivot Points）雖能繪製靜態水平線，但無法動態區分「未回測供需吸收區間」與「已失效區間」，亦缺乏機率校準與期望值閘道。  
   - **與本系統關聯**：Module 2.1 將「未回測供需區 + Volume Profile HVN + S/R Flip」形式化為確定性狀態機，Module 3.1 則將 Brooks 價格行為脈絡規則封裝供 LLM 驗證。

6. **零售高頻交易行為與風控失靈實證 (`[7]`)**  
   印度證券交易委員會 SEBI DEPA (2026) 之《*Trading Behaviour of Individual Traders in the Equity Derivatives Segment*》(`[7]`) 統計 FY26 共 786 萬名個人衍生品交易者，指出 **87.7%** 出現淨虧損，且近 97% 交易者偏好高風險短週期買權部位。  
   - **外推限制**：該報告樣本為印度股票衍生品市場，不能直接等同加密貨幣永續合約之精確虧損率，但其揭露「過度交易、缺乏止損與風控紀律導致持續虧損」之行為機制具有高度警示意義。  
   - **與本系統關聯**：直接催生本系統之「強制 `WAIT/NO_TRADE` 不交易機制」與單筆 `R_tol` 倉位上界。

---

## 2. Article Summary Table（文獻綜述總表）

| Ref. | Year | Research Problem | AI Method | Domain Knowledge | Dataset / Corpus | Key Findings | Documented Limitations |
| :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **[1] Chen & Guestrin** | 2016 | 大規模稀疏表格特徵之高效非線性分類與回歸 | XGBoost (Gradient Boosted Trees) | 特徵工程與正則化模型複雜度控制 | 公開大規模基準資料集（Higgs, Allstate 等） | 二階近似與稀疏感知演算法兼顧高準確度與極低推論延遲 | 本身不具備時序空間座標或交易風控語意，須依賴領域特徵與機率校準 |
| **[2] arXiv:2407.11786** | 2024 | 加密貨幣短期價格走勢與波動預測 | XGBoost Regressor | 技術指標結構（EMA, MACD, RSI） | 歷史加密貨幣價格時間序列 | 梯度提升樹能有效捕捉技術指標與價格變動之非線性關係 | 缺乏供需空間區間表徵與交易摩擦/不交易決策機制 |
| **[3] Koviazin et al. (ACM ICAIF)** | 2026 | LLM 多代理人交易系統之結果重現性 | Multi-Agent LLM (GPT-4o, Qwen3) | 金融基本面/技術/情緒角色分工與風控辯論 | 股票歷史行情與新聞文本 | `temperature=1` 下三個月累積報酬標準差達 `9.0–9.4` 個百分點；部分推理模型忽略溫度設定 | 高生成隨機性、數值計算不穩定，單一隨機種子結果無法代表系統真實績效 |
| **[4] FinForesight** | 2026 | 金融預測 LLM 之歷史記憶與檢索資訊洩漏 | LLM + RAG / Deep Research | Point-in-Time 金融基本面與市場預測 | 美股、A 股與港股 27 組時間區間、22 種模型 | 25/27 組歷史題目虛高；加入 PIT 與洩漏過濾後分數落差縮減 `76%` | 聚焦中長週期基本面與事件方向，未涵蓋高頻 K 線幾何與倉位管理 |
| **[5] Almeida & Gonçalves** | 2023/2024 | 加密貨幣市場微觀結構與交易摩擦統整 | 量化計量與微觀結構分析（綜述 138 篇文獻） | 價格發現、買賣價差、流動性深度與手續費滑點 | 加密貨幣交易所現貨與衍生品文獻庫 | 揭示高頻時序非平穩性與手續費/滑點對日內策略淨報酬的決定性侵蝕 | 屬綜述研究，未提出結合 LLM 與確定性供需區的混合代理人原型 |
| **[6] Brooks** | 2012 | K 線價格行為判讀與機構供需失衡結構 | 專家規則體系（非 AI 實作） | Price Action、Pinbar 吸收、S/R Flip、Liquidity Sweep | 跨市場歷史 K 線圖表案例 | 系統化定義價格結構轉折與高風報比進出場空間依據 | 高度依賴交易員主觀判讀，缺乏自動化程式規格與機率統計驗證 |
| **[7] SEBI (DEPA)** | 2026 | 個人衍生品交易者之行為與長期損益分布 | 實證監管統計分析 | 衍生品風險暴露、合約到期結構與虧損行為 | FY25–FY26 印度 F&O 市場 786 萬名個人帳戶 | FY26 達 **87.7%** 個人交易者淨虧損；過度高頻短週期交易加劇虧損 | 樣本限於印度股票衍生品市場，外推至加密貨幣市場時比例需重新檢驗 |

---

## 3. 四大短線交易架構維度對比矩陣 (M–O Comparison Matrix)

| 研究領域 / 代表架構 | 空間結構感知 | 數據計算與勝率 | 脈絡推理與風控 | 核心局限性 (Limitation) |
| :--- | :---: | :---: | :---: | :--- |
| **傳統指標量化 / 純 ML** *(e.g., EMA+RSI, Pure XGBoost)* | **無** (僅依賴數值切片) | **強** (非線性擬合) | **無** (缺乏語意脈絡) | 缺乏空間座標，震盪與量縮假突破訊號頻繁 |
| **純 LLM 金融 Agent** *(e.g., TradingAgents)* | **無** (僅依賴文本 Prompt) | **弱** (數值幻覺風險) | **強** (宏觀/邏輯推理) | 生成隨機性高 (`9.0–9.4pp` 標準差)，無法確保正期望值 |
| **自動化 SNR 幾何演算法** *(e.g., KDE / Pivot Points)* | **中** (僅靜態水平線) | **無** (無預測能力) | **無** (無動態風控) | 無法辨識動態供需吸收與即時勝率校準 |
| **本專題提案架構 (Hybrid TPA-Agent)** | **強** (確定性供需區引擎) | **強** (校準 XGBoost $W$) | **強** (受約束 CoT + 確定性風控) | 高度依賴歷史流動性深度與 API 穩定性 |

---

## 4. 研究缺口與定位總結 (Research Gap → Proposed Contributions)

$$\text{Existing Research} \longrightarrow \text{Documented Limitations} \longrightarrow \text{Research Gap} \longrightarrow \text{Hybrid TPA-Agent} \longrightarrow \text{Testable Contributions}$$

1. **Existing Research**：純 ML 具統計計算力但缺空間結構；純 LLM 具語意推理但受隨機性與幻覺困擾；傳統 Price Action 具空間直覺但缺客觀規格與 PIT 驗證。
2. **Research Gap**：現有文獻尚未在嚴格時點控制（PIT）與真實交易成本下，驗證「**確定性供需區空間計算 × 校準機率模型 × 受約束 LLM 語意複核 × 強制不交易閘道**」四者解耦後，是否能實質提升決策一致率與風險調整後淨績效。
3. **Testable Contributions**：本系統透過 OpenSpec 規格化此解耦架構，並以 4 階段實驗（時序切分、PIT 隔離、2025-10 清算壓力測試、前瞻模擬）與消融實驗逐一檢驗 `RQ1`–`RQ5`。
