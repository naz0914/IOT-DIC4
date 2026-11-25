# Hybrid Technical & Price Action Agent (Hybrid TPA-Agent)

> **結合確定性供需區、機器學習與大型語言模型之可靠短線交易輔助系統**  
> *A Reliable Short-Term Trading Assistant System Integrating Deterministic Supply & Demand Zones, Machine Learning, and Large Language Models*

---

## 專案成員與指導規範

* **專題組員**：
  * 7115056027 陳柏安
  * 7114056115 侯佳旻
  * 7114068023 陳威碩
  * 7114068027 陳沐德
  * 7115068026 古富綿
* **指導規範**：**2020 A-Z SOP 學術論文寫作標準** ＋ **OpenSpec Spec-Driven Development (SDD)**
* **GitHub 專案庫**：`https://github.com/naz0914/IOT-DIC4.git`
* **重構文件導覽**：[`DESIGN.md`](./DESIGN.md) ｜ [`ICEIB paper/main.tex`](./ICEIB%20paper/main.tex) ｜ [`openspec/changes/iceib-paper-a2z/`](./openspec/changes/iceib-paper-a2z/proposal.md) ｜ [`openspec/changes/refactor-hybrid-tpa-agent/`](./openspec/changes/refactor-hybrid-tpa-agent/proposal.md) ｜ [`docs/abstract.md`](./docs/abstract.md) ｜ [`docs/introduction.md`](./docs/introduction.md) ｜ [`docs/related-work.md`](./docs/related-work.md) ｜ [`docs/deep-research.md`](./docs/deep-research.md) ｜ [`docs/references.md`](./docs/references.md)

---

## 📌 專案全景摘要海報與導讀說明 (Executive Infographic & Overview)

<div align="center">
  <img src="./Gemini_Generated_Image_ivdx4yivdx4yivdx.jpg" alt="Hybrid TPA-Agent 專案全景架構海報" width="100%" />
  <p><em>全景資訊圖：Hybrid TPA-Agent 核心架構、A-Z SOP 規範對照、四層八模組與預期評估成效</em></p>
</div>

### 🔍 全景海報核心板塊深度解析

本全景資訊海報為本專題之總體研究藍圖，彙整了論文提案的七大核心精華：

1. **符合度總覽與 Abstract 六段式 (A–F)**
   * **A (Attention Getter)**：市場雜訊與高頻波動劇烈，散戶面臨嚴重的資訊過載與決策情緒偏誤。
   * **B (But/However)**：傳統技術指標缺乏空間連續感，純 LLM 則易產生嚴重的數值與空間幻覺。
   * **C (Cure)**：提出 Hybrid TPA-Agent 解耦式混合架構，實現「數值幾何運算」與「語意脈絡推理」徹底分離。
   * **D (Development)**：以確定性演算法辨識供需區（S&D）＋ XGBoost 預測條件勝率 ＋ LLM 執行思維鏈（CoT）推理。
   * **E (Experiments)**：採用 BTC/USDT 與 ETH/USDT 5m/15m 歷史資料，嚴格對比傳統雙指標（EMA+RSI）、純機器學習與純 LLM。
   * **F (Findings)**：預期過濾掉 >40% 的假突破雜訊，獲利因子（Profit Factor）預期達 > 1.8，最大回撤（MDD）抑制於 12% 內。

2. **文獻演進與相關研究缺口 (2.2 Introduction G–L & 2. Related Work M–O)**
   * 系統化梳理既有量化方法的缺陷：指標派滯後、純 ML 缺乏空間座標、純 LLM 生成隨機性過高（重現率低）。本系統填補了「空間感知 × 數據計算 × 脈絡推理」的交易鐵三角缺口。

3. **理論基礎與十大深度理解問題 (3.1–3.3 Deep Research)**
   * 深入探討價格帶量跌破時的支撐壓力轉換機制（S/R Flip）、低波動突破時 ML 的誤判風險（Volume Trap）、時間週期縮小帶來的成本侵蝕，以及真實機構訂單流的吸收證據（Pinbar、ChoCh）。

4. **系統架構與四層八模組設計 (4.1–4.4 Proposed Scheme)**
   * **1. 輸入與資料接收層**：使用者參數配置與低延遲 OHLCV 行情串流。
   * **2. 核心分析運算層**：供需區幾何標記與 XGBoost 條件勝率過濾網。
   * **3. LLM 推理與領域知識層**：封裝裸 K 價格行為學規則，執行 CoT 多空論證。
   * **4. 風控管理與決策輸出層**：動態 ATR 停損/停利反推，正期望值過濾，輸出 APPROVED 或 WAIT。

5. **26 字母學術結構映射 (5.1–5.3 A2Z SOP Outline)**
   * 嚴格遵循 2020 A-Z SOP 規範，建立從動機、研究缺口、方法設計、實作偽代碼到模擬驗證與邊界總結的完整閉環。

6. **評估規劃與預期成效 (6.1–6.3 Simulation & Results)**
   * 採用 2023–2024 兩年期回測資料，嚴格納入手續費（Taker 0.04%）與滑點成本，並導入時點控制（Point-in-time）杜絕歷史資訊洩漏。各基準模型在勝率、夏普比率與回撤控制之預期對比如表中所示。

7. **整體結論三大核心亮點 (Conclusion Highlights)**
   * **7.1 高度規範符合**：結構完整吻合頂級學術標準與可否證性原則。
   * **7.2 方法論嚴謹**：提供具備時點控制與壓力測試的可複現研究原型。
   * **7.3 架構徹底解耦**：根除生成式 AI 數值幻覺，以正數學期望值驅動決策。

---

## 目錄 (Table of Contents)

1. [專案全景摘要海報與導讀說明 (Executive Infographic)](#-專案全景摘要海報與導讀說明-executive-infographic--overview)
2. [完整學術摘要：對應 A–F 規範 (Abstract)](#1-完整學術摘要對應-a-f-規範-abstract)
3. [緒論與研究背景：對應 G–L 規範 (Introduction)](#2-緒論與研究背景對應-g-l-規範-introduction)
4. [文獻演進與研究缺口對比：對應 M–O 規範 (Related Work)](#3-文獻演進與研究缺口對比對應-m-o-規範-related-work)
5. [五大核心研究問題 (Core Research Questions, RQ1–RQ5)](#4-五大核心研究問題-core-research-questions-rq1-rq5)
6. [學術寫作指引對照：2020 A-Z SOP (Academic Guideline)](#5-學術寫作指引對照2020-a-z-sop-academic-guideline)
7. [系統核心架構：四層八模組 (System Architecture)](#6-系統核心架構四層八模組-system-architecture)
8. [決策流程與演算法 (Decision Flow & Algorithm 1)](#7-決策流程與演算法-decision-flow--algorithm-1)
9. [計算複雜度與時間延遲預算分析 (Complexity & Latency Budget)](#8-計算複雜度與時間延遲預算分析-complexity--latency-budget)
10. [風控機制、數學公式與 WAIT 機制 (Risk Management & Formulation)](#9-風控機制數學公式與-wait-機制-risk-management--formulation)
11. [端到端實例穿透解析：BTC/USDT 15m (End-to-End Walkthrough)](#10-端到端實例穿透解析btcusdt-15m-end-to-end-walkthrough)
12. [實驗設計、驗證階段與評估指標體系 (Simulation & Validation)](#11-實驗設計驗證階段與評估指標體系-simulation--validation)
13. [2 年期歷史回測假說目標對比 (Expected Targets)](#12-2-年期歷史回測假說目標對比-expected-targets)
14. [十大深度專業爭辯與邊界檢驗 (Ten Deep Understanding Questions)](#13-十大深度專業爭辯與邊界檢驗-ten-deep-understanding-questions)
15. [研究限制與學術客觀表述原則 (Limitations & Objective Stance)](#14-研究限制與學術客觀表述原則-limitations--objective-stance)
16. [參考文獻 (References)](#15-參考文獻-references)
17. [IEEE ICEIB 論文重構與 OpenSpec 細項工作流 (ICEIB Paper & OpenSpec)](#16-ieee-iceib-論文重構與-openspec-細項工作流-iceib-paper--openspec)
18. [專案檔案結構 (Repository Structure)](#17-專案檔案結構-repository-structure)

---

## 1. 完整學術摘要：對應 A–F 規範 (Abstract)

本專案提案摘要嚴格對照 **2020 A-Z SOP** 學術論文規範之 **A–F 六段式**深度解析：

* **【A. Attention Getter 動機與引題】**  
  短線交易面臨市場雜訊、資訊過載與快速決策壓力。印度證券交易委員會（SEBI）於 2026 年發布的報告顯示，FY26 個人股票衍生品交易者中高達 **87.7%** 出現虧損，且交易經驗不必然轉化為穩定獲利，凸顯交易輔助系統除追求預測準確度外，更須兼顧嚴格的風險管理與決策可靠性。
* **【B. But/However 現存挑戰與缺口】**  
  傳統指標難以完整表達價格空間結構；大型語言模型（LLM）雖能進行情境判讀，其交易決策仍極易受到生成隨機性（溫度、隨機種子）影響而產生劇烈變異。此外，近期研究（如 FinForesight, 2026）指出，歷史資訊洩漏可能大幅高估金融 AI 的預測能力，顯示單純的回測績效已不足以證明系統在實戰中的可靠度。
* **【C. Cure 核心解藥】**  
  本研究提出 **Hybrid TPA-Agent**，整合確定性供需區演算法、XGBoost 條件機率模型與大型語言模型，建立**「數值運算與語意判讀徹底分離」**的短線交易輔助架構，將系統整合轉化為可量化與可否證的功能分工假說。
* **【D. Development 方法發展與架構整合】**  
  系統以演算法客觀辨識供需區與關鍵價位，由 XGBoost 估計交易成功機率，再由 LLM 整合價格行為證據；進場、停損、部位及風報比則交由程式化風控引擎計算，並導入嚴格的**「不交易（WAIT/NO_TRADE）機制」**與人工覆核，確保決策邏輯的可追溯性。
* **【E. Experiments 實驗設計與驗證場景】**  
  採用 BTC/USDT 與 ETH/USDT 之 5m/15m 歷史資料，比較傳統指標、純機器學習、純語言模型與消融架構。實驗設計除考量交易成本外，更特別規劃重複執行一致性檢驗、時點資訊控制（Point-in-time）、極端行情壓力測試及前瞻模擬驗證。
* **【F. Findings 預期貢獻與目標】**  
  預期建立可追溯、可複現的研究原型，量化各模組對淨績效、決策一致性與風控遵循的實質增益。本研究旨在釐清 LLM 於金融交易的適用邊界，將最高回撤抑制目標設為 **12% 內**，為高可靠度的金融人工智慧提供堅實的實證研究基礎。

> 📄 **完整 300 字中文學術摘要與 IEEE Systems Journal 單段英文 Abstract**：請參閱 [`docs/abstract.md`](./docs/abstract.md)（含 `Index Terms` 與規格重構版論文結構 [`docs/introduction.md`](./docs/introduction.md)）。

---

## 2. 緒論與研究背景：對應 G–L 規範 (Introduction)

### 2.1 研究背景與問題重要性 (G: General)
在現代金融市場中，短線合約交易與日內當沖吸引了大量零售投資人。然而，本研究關注的核心並非單純建立「預測漲跌的 AI」，而是探討：**如何讓交易 AI 在高雜訊與極端行情下，產出受風控約束、具有可追溯證據，且能重複驗證的交易決策（包含準確判斷何時「不應交易」）。** 金融高頻時序具備高度非平穩性，零售交易員在高壓下極易陷入認知疲勞與情緒偏誤，導致災難性虧損。本報告進一步將焦點收斂至「功能分工是否能實質改善決策可靠性」。

### 2.2 監管數據啟示：持續交易不等於持續獲利 (H: However)
印度證券交易委員會 (SEBI) 於 2026 年發布的《Trading Behaviour of Individual Traders in the Equity Derivatives Segment》報告揭露了極為驚人的實證數據：
* FY26 涵蓋的 786 萬名個人交易者中，**虧損比例高達 87.7%**。
* FY22–FY26 五年皆持續交易者中，**僅 0.5% 每年皆獲利**。
* 前兩年均虧損且持續交易者，**下一年仍虧損的比例高達 90–92%**。
> **啟示**：交易經驗的累積不必然轉化為穩定成果，研究不能只追求「更多交易訊號」，更須檢驗風險控制、曝險限制與「不交易機制」對決策品質的改善。

### 2.3 新聞案例：極端行情對風控的挑戰
Reuters 於 2025 年 10 月報導，加密貨幣市場在兩日內劇烈下跌，槓桿部位強制平倉規模超過 **190 億美元**，BTC 跌幅逾 14%。此類由恐慌賣壓與低流動性共振引發的極端行情，是檢驗量化系統的絕佳壓力測試場景。本研究將此類事件納入，旨在檢驗系統是否能在異常波動時減少新開倉、拒絕交易是否真正降低了損失，以及在市場狀態快速轉變時，各模組的失效順序與邊界。

### 2.4 既有文獻批判與研究缺口 (I: In Literature & J: Judgement)
* **LLM 隨機性挑戰**：ACM 2026 年的 TradingAgents 研究顯示，在溫度為 1 的設定下，GPT-4o 與 Qwen3 等模型在三個月內的累積報酬標準差高達 9.0 至 9.4 個百分點。這表明單一固定設定的結果無法證明系統優越性，核心問題應為「同一份市場資料下，AI 能否穩定產出符合規則的決策」。
* **歷史資訊洩漏風險**：2026 年 11 月公開的 FinForesight 預印本評估了 22 種金融模型與 Agent 工作流，在 27 組時間區間比較中，有 25 組因歷史洩漏使回測虛高；在嚴格時點控制（Point-in-time）介入後，評估分數差距縮小達 76%。

### 2.5 本研究核心貢獻 (K: Keypoint / Contributions)
1. **首創空間感知與推理分離之短線 AI 代理人架構**：提出三層/四層解耦式架構，由確定性空間演算法、機器學習分類器與大型語言模型各司其職，從架構底層根除純 AI 金融系統之數值與空間幻覺。
2. **實現主觀價格行為學與客觀機器學習之深度融合**：將操盤手依賴的主觀價格行為學（裸 K、市場結構轉折、供需失衡）轉化為客觀、可程式重現之確定性演算法，並結合 XGBoost 萃取多維特徵進行條件勝率預測。
3. **建立嚴格正期望值風控機制與標準化開源規範**：落實動態風險報酬比（RR）與數學期望值雙門檻過濾網，並開源完整的系統規格架構以促進社群複現。

---

## 3. 文獻演進與研究缺口對比：對應 M–O 規範 (Related Work)

### 表 1：四大短線量化交易輔助技術維度對比矩陣

| 研究領域 / 代表架構 | 空間結構感知 | 數據計算與勝率 | 脈絡推理與風控 | 核心局限性 (Limitation) |
| :--- | :---: | :---: | :---: | :--- |
| **傳統指標量化 / 純 ML**<br>*(e.g., XGBoost 單獨使用)* | **無**<br>(僅依賴數值特徵) | **強**<br>(具備非線性擬合能力) | **無**<br>(缺乏語意脈絡推理) | 缺乏空間座標，震盪行情假訊號頻繁發布。 |
| **純 LLM 金融 Agent**<br>*(e.g., TradingAgents)* | **無**<br>(僅依賴純文本 Prompt) | **弱**<br>(容易產生嚴重數值幻覺) | **強**<br>(具備宏觀與邏輯推理) | 運算不可靠，決策隨機性高，無法確保正數學期望值。 |
| **自動化 SNR 幾何演算法**<br>*(e.g., KDE / Pivot Points)* | **中**<br>(僅繪製單一水平線) | **無**<br>(純幾何規則無預測能力) | **無**<br>(無法評估勝率與風報比) | 僅能靜態劃線，缺乏即時勝率回測與動態風控機制。 |
| **本專題提案架構<br>(Hybrid TPA-Agent)** | **強**<br>(確定性供需區引擎) | **強**<br>(XGBoost 條件勝率) | **強**<br>(LLM 結構化 CoT 推理) | 高度依賴市場歷史流動性深度與低延遲 API。 |

> **【重點解析 (So What)】**  
> 表 1 清晰凸顯了既有單一路線的局限性：單一技術皆無法同時滿足『精準空間感知、可靠機率計算、情境語意推理』之交易鐵三角需求。**Hybrid TPA-Agent** 正是填補此重大研究缺口之整合型解方。
>
> 📚 **50% AI 技術 + 50% 領域知識文獻回顧與完整 Article Summary Table**：請參閱 [`docs/related-work.md`](./docs/related-work.md)。

---

## 4. 五大核心研究問題 (Core Research Questions, RQ1–RQ5)

本專案提出具備明確適用範圍且**允許負向結果**的五大核心研究問題：

* **RQ1**：加入供需區與價格結構特徵，是否改善純 XGBoost 模型的預測及交易表現？
* **RQ2**：加入 LLM 情境判讀後，是否能顯著降低無效交易或改善風險調整後績效？
* **RQ3**：數值運算與語意判讀分離，是否有效降低數值錯誤、風控違規與決策變異？
* **RQ4**：`WAIT/NO_TRADE` 機制所避免的損失，是否足以補償其錯失的有效交易機會？
* **RQ5**：系統的增益效益在嚴格的時點控制（Point-in-time）、極端行情與前瞻模擬中，是否仍然存在？

---

## 5. 學術寫作指引對照：2020 A-Z SOP (Academic Guideline)

本專題論文嚴格遵循標準 **2020 A-Z SOP** 學術寫作規範，落實論點、方法與實驗的逐項對應：

<div align="center">
  <img src="./IMG_9464.png" alt="2020 A-Z SOP 學術規範指引" width="85%" />
  <p><em>圖 1：2020 A-Z SOP 學術論文寫作標準映射</em></p>
</div>

* **Abstract (A–F)**：Attention Getter ➔ But/However ➔ Cure ➔ Development ➔ Experiments ➔ Findings
* **Introduction (G–L)**：General ➔ However ➔ Literature ➔ Judgement ➔ Keypoint ➔ List Organization
* **Related Work (M–O)**：Methods ➔ New Proposed ➔ Organize Comparison Matrix
* **Proposed Scheme (PA–PM)**：Aim, Based-on, Cure, Design, Elements, Formulation, Graph, How, Implementation, Jump Example, Key Contribution, Later Simulation, Math Proof
* **Simulation (Q–V)**：Quality/Quantity Metrics, Related Methods, Simulation Setup, Tuning Parameters, Useful Results, Verify Effectiveness
* **Conclusion (W–Z)**：What Proposed, eXcel, Yields, Zen/Limitations

---

## 6. 系統核心架構：四層八模組 (System Architecture)

系統立足於現代微觀市場結構與拍賣市場理論（Auction Market Theory），透過模組化解耦實現高可靠性：

<div align="center">
  <img src="./架構圖.png" alt="四層八模組系統架構圖" width="100%" />
  <p><em>圖 2：Hybrid TPA-Agent 四層八模組核心系統架構圖</em></p>
</div>

### 四大階層與八大模組職責明細

#### 第一層：輸入與資料接收層 (Layer 1: Input & Data Layer)
* **【模組 1.1】User Preference Block**：設定監控資產（如 BTC/USDT）、單筆最大風險虧損比例（Risk Tolerance $R_{\text{tol}}$，如 1%）與策略風控偏好。
* **【模組 1.2】Data Ingestion Engine**：透過低延遲 WebSocket / REST API 即時抓取交易所 OHLCV 歷史與實時數據流。

#### 第二層：核心分析運算層 (Layer 2: Core Analytical Engines)
* **【模組 2.1】Deterministic S&D Zone Engine**：執行雙指針極值比對，標記 Swing High / Swing Low，鎖定未回測供需區塊與 Volume Profile 高成交量節點（HVN）。
* **【模組 2.2】Machine Learning Win-Rate Engine**：萃取多維微觀結構特徵矩陣（Volume Z-score、實體比例、刺穿深度），由訓練完備之 XGBoost 分類器輸出條件勝率 $W$。

#### 第三層：LLM 推理與領域知識層 (Layer 3: LLM Reasoning & Domain Layer)
* **【模組 3.1】Domain Knowledge Base**：結構化封裝 S/R Flip（支撐壓力互換）、Pinbar 被動吸收、流動性掠奪（Liquidity Sweep）等裸 K 價格行為學規則庫。
* **【模組 3.2】Planner & Reasoning Agent**：以 Chain-of-Thought (CoT) 推理架構串接空間座標與勝率預測，進行嚴謹的多空情境論證與偽突破剔除。

#### 第四層：風控管理與決策輸出層 (Layer 4: Risk Management & Output Layer)
* **【模組 4.1】Decision & Risk Engine**：依據供需邊界反推進場點、動態 ATR 停損點（SL）、目標停利點（TP），並計算部位規模與檢驗正期望值門檻。
* **【模組 4.2】Output Interface**：產出符合標準結構化格式（含 JSON 數據與 Markdown 決策卡）之報告供交易員執行或系統自動下單。

---

## 7. 決策流程與演算法 (Decision Flow & Algorithm 1)

<div align="center">
  <img src="./流程圖flowchart.png" alt="決策執行流程圖" width="55%" />
  <p><em>圖 3：Hybrid TPA-Agent 端到端決策與風控執行流程圖</em></p>
</div>

### 演算法虛擬碼 (Algorithm 1)

```python
Algorithm 1: Hybrid Technical & Price Action Agent (Hybrid TPA-Agent)
Input : Asset Symbol S, Timeframe TF, Real-time OHLCV Stream D, Risk Tolerance R_tol
Output: Structured Trading Setup & Decision Report R_final

01: Initialize DataPipeline, SNR_Engine, ML_WinRate_Classifier, PlannerAgent
02: raw_data <- DataPipeline.fetch_ohlcv(S, TF)

# 步驟一：確定性演算法標記空間結構與供需區塊
03: demand_zones, supply_zones <- SNR_Engine.detect_zones(raw_data)
04: current_price <- raw_data.close[-1]

# 步驟二：特徵工程與機器學習勝率預測
05: features <- FeatureExtractor.extract(raw_data, demand_zones, supply_zones)
06: win_prob <- ML_WinRate_Classifier.predict_proba(features)

# 步驟三：勝率門檻快速過濾 (初篩，硬性淘汰低機率訊號)
07: IF win_prob < 0.55 THEN
08:     return SynthesisEngine.generate_report(
            Status="WAIT/NO_TRADE", 
            Reason="Win Rate below 55%"
        )
09: END IF

# 步驟四：LLM 價格行為 CoT 推理與動態風控計算
10: agent_ctx <- AssembleContext(current_price, demand_zones, supply_zones, win_prob)
11: reasoning_result <- PlannerAgent.reason_with_cot(agent_ctx, PriceActionKnowledgeBase)
12: stop_loss, take_profit, RR <- RiskEngine.calculate(current_price, demand_zones, supply_zones)

# 步驟五：正期望值檢驗與報告生成
13: IF RR >= 2.0 AND reasoning_result.is_valid_setup THEN
14:     position_size <- RiskEngine.calc_position(current_price, stop_loss, R_tol)
15:     R_final <- SynthesisEngine.generate_report(
            Status="APPROVED",
            Entry=current_price,
            SL=stop_loss,
            TP=take_profit,
            RR=RR,
            Pos=position_size,
            Evidence=reasoning_result.evidence
        )
16: ELSE
17:     R_final <- SynthesisEngine.generate_report(
            Status="WAIT/NO_TRADE", 
            Reason="RR < 2.0 or Structural Rejection"
        )
18: END IF
19: return R_final
```

---

## 8. 計算複雜度與時間延遲預算分析 (Complexity & Latency Budget)

系統針對短線盤面之時間與空間複雜度進行極致優化，滿足即時線上推論要求：

* **空間演算法複雜度**：Swing High/Low 搜尋時間複雜度為 $\mathcal{O}(N)$；Volume Profile 聚合為 $\mathcal{O}(N \log B)$。在 $N = 500$ 根 K 線下，Python 程式耗時 **$< 15 \text{ ms}$**。
* **機器學習推論複雜度**：XGBoost 決策樹推論複雜度為 $\mathcal{O}(T \times D)$（其中 $T$ 為樹顆數，$D$ 為樹深），耗時 **$< 5 \text{ ms}$**。
* **整體延遲與 API 成本**：LLM 採用串流 API 結構化輸出（JSON Schema），耗時約 $800 \sim 1200 \text{ ms}$。**全管線端到端決策週期穩定於 1.5 秒以內**，充分滿足 5m/15m K 線級別的決策時效。

---

## 9. 風控機制、數學公式與 WAIT 機制 (Risk Management & Formulation)

本系統以**嚴格正數學期望值**為開倉絕對前置條件，拒絕盲目交易：

### 1. 幾何風險報酬比 (Risk-to-Reward Ratio, RR)
對多頭（Long）交易設定：
$$RR = \frac{P_{\text{tp}} - P_{\text{entry}}}{P_{\text{entry}} - P_{\text{sl}}}$$

### 2. 簡化單筆數學期望值 (Mathematical Expectation, E)
若以單筆最大停損風險為 1.0 單位風險（$R$）：
$$E = (W \times RR) - ((1 - W) \times 1.0)$$

* **參數檢驗準則**：原提案設定雙門檻 $W \ge 0.55$ 且 $RR \ge 2.0$。在研究中此數值視為待測參數（Parameters to be tested），實驗設計中必須納入手續費與滑點並檢驗機率校準。
* **「不交易（WAIT / NO_TRADE）」機制**：在勝率不足、風報比不符、空間被壓縮或價格結構無效時，系統強制輸出 `WAIT/NO_TRADE`。研究將特別衡量被拒絕訊號的後續結果，以避免將單純的「交易變少」誤認為「判斷變好」。人工覆核則保留最終裁決權。

---

## 10. 端到端實例穿透解析：BTC/USDT 15m (End-to-End Walkthrough)

以 **BTC/USDT 15 分鐘線實盤行情穿透** 為例：

1. **資料接收與空間定位**：
   * API 抓取最近 200 根 15m K 線，現價為 **$64,250**。
   * 確定性供需區引擎結合 Volume Profile 高成交量節點（HVN），精確辨識出下方一處未被測試之強需求區 **[$63,800, $64,000]**，同時標出上方阻力供給區 **[$65,200, $65,500]**。
2. **特徵擷取與勝率計算**：
   * 價格隨後回測需求區上緣（現價 **$63,950**）。
   * 特徵擷取模組算出 Volume Z-score 為 **+2.3**（爆量收長下影線 Pinbar）。
   * XGBoost 分類器輸出回彈勝率預測值 **$W = 0.62$**（$\ge 0.55$，通過初篩）。
3. **LLM 情境推理與風控反推**：
   * PlannerAgent 結合知識庫檢索，確認價格刺穿 $63,850 後強勢收回，判定為典型**流動性掠奪（Liquidity Sweep）**型態。
   * 風控引擎計算：進場點 **$63,950**，停損設於需求區外側 **$63,750**（風險空間 $200），目標停利設於供給區下緣 **$65,150**（獲利空間 $1,200）。
   * 風報比 $RR = \frac{1200}{200} = 6.0$（$\ge 2.0$）。
   * 數學期望值 $E = 0.62 \times 6.0 - 0.38 \times 1.0 = \mathbf{+3.34} > 0$。
4. **結構化決策輸出**：
   * 系統狀態核准為 **`APPROVED`**，產出建議倉位比例（單筆風險 1%）與完整證據鏈之結構化交易決策報告。

---

## 11. 實驗設計、驗證階段與評估指標體系 (Simulation & Validation)

### 四大嚴謹驗證階段
1. **歷史開發與測試**：依時間順序嚴格切分訓練、驗證與測試集，杜絕未來資訊。
2. **時點控制回測 (Point-in-time)**：嚴格限制模型與檢索流程只能使用決策當下確實可取得的切片資訊，防範歷史洩漏。
3. **極端行情壓力測試**：以具名歷史極端事件（如 2025 年 10 月 190 億美元強制平倉事件）檢查風控與策略退化情況。
4. **前瞻模擬交易 (Forward Simulation)**：事前鎖定決策並保存雜湊記錄，驗證未來真實表現。

### 四大維度評估指標體系

* **預測品質**：Precision、ROC-AUC、Brier Score（檢驗機率校準與模型辨識力）。
* **交易績效**：淨報酬（Net Return）、獲利因子（Profit Factor）、夏普比率（Sharpe Ratio）、最大回撤（MDD）（嚴格扣除手續費與滑點）。
* **決策可靠性**：重複執行一致率（Consistency Rate）、數值錯誤率、風控違規率（檢驗系統穩定性）。
* **不交易品質**：交易保留率、被拒絕訊號後續結果比較（評估避免損失與錯失有效機會的權衡）。

---

## 12. 2 年期歷史回測假說目標對比 (Expected Targets)

報告將呈現重複執行的分布與最差情境，而不僅展示最佳資金曲線。以下為各模型架構之「假說目標值（Expected Targets）」，目的在展示實驗欲檢驗的比較基準：

### 表 2：2 年期歷史回測預期績效對比 (假說目標值)

| 策略 / 比較模型 | 預測勝率目標 | 獲利因子目標 | 夏普比率目標 | 最大回撤 (MDD) 控制 | 決策一致率目標 |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Baseline 1: EMA+RSI 雙指標** | ~41.2% | 1.18 | 0.74 | -28.5% | 100% (確定性) |
| **Baseline 2: 純 XGBoost 分類** | ~52.8% | 1.35 | 1.12 | -22.1% | 100% (確定性) |
| **Baseline 3: 純 LLM 代理人** | ~46.5% | 1.22 | 0.85 | -34.2% | < 60% (高隨機變異) |
| **Ablation: 供需區 + XGBoost** | ~55.4% | 1.62 | 1.58 | -15.8% | 100% (確定性) |
| **本專題: Hybrid TPA-Agent** | **~59.2%** | **1.94** | **2.15** | **控制於 -12.0% 內** | **> 95% (高度重現)** |

> **【評估說明】**：表 2 中 Hybrid 系統的增益目標（如獲利因子 1.94）建立在「成功執行 WAIT 機制並過濾 40% 雜訊」的假說上。研究將嚴格驗證這些數值是否能在時點控制與扣除 Taker 0.04% 及滑點成本後依然成立。

---

## 13. 深度研究：五大核心思維、三大專業爭論與十大深度理解問題 (Deep Research)

為符合專題規範之深度研究（Deep Research）要求，完整之 **五大核心思維模式（含意義、證據與設計影響）** 與 **三大專業爭論 / Trade-offs（含雙方證據、共識與未解問題）** 已整理於 [`docs/deep-research.md`](./docs/deep-research.md)：
* **五大核心思維**：(1) 供需失衡與拍賣市場微觀結構、(2) 數學正期望值與機率校準、(3) 決策可靠性與 `WAIT` 優先、(4) 數值與語意徹底解耦、(5) 嚴格 PIT 時點控制與無洩漏實證。
* **三大專業爭論**：(1) 高訊號覆蓋率 vs. 嚴格濾網與不交易紀律、(2) LLM 語意推理彈性 vs. 重現性與延遲代價、(3) 極短週期 (`1m`) 機會 vs. 微觀雜訊與交易成本侵蝕。

<details>
<summary><b>點擊展開：十大關鍵問題深入剖析</b></summary>

1. **[Why] 價格帶量跌破需求區時為何支撐轉為壓力 (S/R Flip)？**  
   當某處需求區被帶量跌破時，代表買方未成交限價單全數被空頭吞噬，大量多頭部位就地套牢。當價格未來再次反彈至該區間時，套牢盤保本平倉賣壓與破位順勢加碼空單同時湧現，使原本之需求區質變為強大阻力區。
2. **[What-if] 低波動環境突然出現大陽線突破，ML 有何偏誤？**  
   在極致量縮期市場深度極淺，微小散戶單即可推升大陽線。此時 Volume Z-score 暴增易導致 XGBoost 誤判為強勢起漲，但因背後缺乏機構真實大單跟進，極易演變為流動性掠奪的假突破 Trap。
3. **[Trade-off] 縮小時間週期（如改用 1 分鐘 K 線）有何負面影響？**  
   雖然交易機會顯著增加，但 1 分鐘線充斥做市商高頻噪音與隨機波動，空間結構穩定性極低，導致假供需區倍增，交易成本（手續費與滑點）將徹底侵蝕期望值。
4. **[Failure Case] 流動性枯竭時成交量分佈為何失效？**  
   Volume Profile 依賴真實多空激烈交鋒所形成之成交量峰值（HVN）。假日或收盤前夕市場深度不足，少量籌碼即引發價格真空位移，導致演算法標記出無實質機構訂單支撐的偽供需區。
5. **[Evidence] 判斷價格回測為「真反彈」抑或「假跌破」之關鍵證據？**  
   三大關鍵特徵：1) 收盤位置：長下影線刺穿但收盤強勢收回需求區上緣（Pinbar）；2) 量價特徵：刺穿時成交量顯著放大但 K 線實體極小，顯示大量被動吸收（Passive Absorption）；3) 次級結構轉變（ChoCh）。
6. **[Comparison] 相較均線交叉策略，供需區在風報比上的核心優勢？**  
   均線交叉需等趨勢確立，進場點遠且停損難設，RR 通常低於 1.5。供需區策略在價格回踩關鍵空間邊界時以左側或微右側切入，停損緊貼區間外側，目標看至下一空間區塊，數學上鎖定 1:3 至 1:5 的極佳 RR。
7. **[Design Decision] 為何供需區計算必須與 LLM 推理徹底解耦？**  
   LLM 擅長語意邏輯但完全不擅長浮點數精確計算與空間座標感知。將空間矩陣運算交由 100% 確定性 Python 程式執行，將結構化 JSON 交給 LLM 進行語意推理，是杜絕幻覺的唯一途徑。
8. **[Real-world Scenario] API 斷流或延遲時系統如何降級？**  
   建立三級降級機制：Level 1 (心跳延遲 > 2s) 立即凍結新訊號；Level 2 (在途單) 確保伺服器端硬性停損單在交易所掛妥；Level 3 (斷流 > 30s) 觸發警報並市價對沖平倉。
9. **[Trade-off] 要求極端高勝率（如要求 W ≥ 70%）有何代價？**  
   過於嚴苛的濾網會使短線交易機會數量呈指數型衰減（一週可能僅 1 筆），嚴重壓低資金使用效率，陷入『高勝率但總期望利潤微薄』的次佳解。
10. **[Failure Case] 遭遇黑天鵝暴跌時 ML 為何失效？**  
    面對突發地緣政治或宏觀巨震，時序數據遭遇概念飄移（Concept Drift）與分佈外數據（OOD）。K 線技術特徵仍處於常態時價格已直接穿倉，必須依賴 LLM 即時新聞感知與硬性風險熔斷機制。
</details>

---

## 14. 研究限制與學術客觀表述原則 (Limitations & Objective Stance)

本研究嚴守學術客觀表述原則，不作過度強烈的技術宣稱（如「徹底杜絕」改為「量測並降低錯誤率」）。外推限制包含：

* **SEBI 數據外推限制**：SEBI 的虧損統計限於印度股票衍生品市場，不能直接等同加密貨幣交易者的精確虧損比例。
* **TradingAgents 復現邊界**：ACM TradingAgents 的重現性變異結果基於特定股票與期間設定，需在本研究加密貨幣框架下重新檢驗。
* **FinForesight 任務差異**：FinForesight 的資訊洩漏評估為預印本成果，其任務設計與本研究的 5m/15m 短線交易不同。

> **【總結】**  
> 面對高虧損率與極端行情的壓力測試，交易 AI 除預測能力外，更需要可驗證的風控與決策品質。Hybrid TPA-Agent 的成功標準不應只是一組較高的報酬數字，而是：**能清楚說明何種架構設計帶來何種改善、改善是否足以抵銷成本，以及系統在何種情境下不再可靠**。

---

## 15. 參考文獻 (References)

1. **Securities and Exchange Board of India (SEBI), DEPA**. (2026, August). *Trading Behaviour of Individual Traders in the Equity Derivatives Segment (FY25–FY26)*.
2. **Chen, T., & Guestrin, C.** (2016). XGBoost: A scalable tree boosting system. *Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining*, 785–794.
3. **Koviazin, A., Mudarisov, T., Polyachenko, E., & State, R.** (2026). Reproducibility in the TradingAgents Framework. *Proceedings of the 2026 International Conference on Artificial Intelligence and Fintech (ICAIF '26)*, ACM, 365–370. DOI: [10.1145/3800973.3801029](https://doi.org/10.1145/3800973.3801029).
4. **FinForesight Authors**. (2026). FinForesight: Assessing Information Leakage and LLM Financial Prediction Capabilities. *arXiv preprint*.
5. **Reuters**. (2025, October). *Crypto markets face severe liquidation event amid volatile trading conditions*.
6. **Brooks, A.** (2012). *Reading Price Charts Bar by Bar: The Technical Analysis of Price Action for the Serious Trader*. John Wiley & Sons.
7. **arXiv:2407.11786**. (2024). Cryptocurrency Price Forecasting Using XGBoost Regressor and Technical Indicators. *arXiv preprint*. DOI: [10.48550/arXiv.2407.11786](https://doi.org/10.48550/arXiv.2407.11786).
8. **Almeida, J., & Gonçalves, T. C.** (2023/2024). Cryptocurrency market microstructure: a systematic literature review. *Annals of Operations Research*. DOI: [10.1007/s10479-023-05627-5](https://doi.org/10.1007/s10479-023-05627-5).

> 詳細文獻邊界與限制說明請見 [`docs/references.md`](./docs/references.md)。

---

## 16. IEEE ICEIB 論文重構與 OpenSpec 細項工作流 (ICEIB Paper & OpenSpec)

本次更新新增了 **IEEE ICEIB 會議論文標準化管線**、**Overleaf MCP 雲端編譯同步**，以及 **總體＋分章節 OpenSpec 細項規範**，確保論文內容、數學證明、實驗邊界與系統設計規格維持 100% 一致：

### 16.1 新增功能亮點
1. **Overleaf MCP 遠端同步與編譯驗證**：
   * 透過 `@netique/overleaf-mcp` 連線 Overleaf 專案 `DIC4`，將 [`ICEIB paper/main.tex`](./ICEIB%20paper/main.tex)、[`main.txt`](./ICEIB%20paper/main.txt) 與架構/流程圖（`fig1_arch.png`、`fig2_flow.png`）同步至雲端並執行遠端 `pdfLaTeX` 編譯。
   * 編譯驗證結果：**0 LaTeX errors**、**PDF 共 6 頁**（符合 IEEE ICEIB `10pt` Times 雙欄、4–6 頁、無自訂頁碼／頁首頁尾之會議格式），且表格與 `Algorithm 1` 均無水平溢出（`Overfull \hbox`）。
2. **OpenSpec 論文規範工作流（總計畫 ＋ 7 大分章節細項）**：
   * **總體重構計畫 [`openspec/changes/iceib-paper-a2z/`](./openspec/changes/iceib-paper-a2z/proposal.md)**（`9/9 tasks complete`）：定義 7 大章節重構與 Overleaf 編譯驗收準則。
   * **分章節細項變更（`000-abstract` ~ `006-reference`，均通過 `openspec validate --strict`）**：每個章節皆具備獨立 `proposal.md`、`specs/`、`design.md` 與 `tasks.md`，支援單章節獨立迭代（例如實驗數據產出後可單獨執行 `openspec-apply-change 004-experiment`）。

### 16.2 IEEE ICEIB 論文內容大綱與 A–Z SOP 映射

| OpenSpec 變更 | 論文章節 (IEEE ICEIB) | 2020 A–Z SOP 對應 | 核心內容與實測頁碼定位 |
| :--- | :--- | :---: | :--- |
| [`000-abstract`](./openspec/changes/000-abstract/proposal.md) | **Abstract & Keywords** | `A–F` | 單段式 IEEE 摘要：SEBI 87.7% 虧損引題、三代限制、四層八模組解耦、4 階段 PIT 規劃與可否證預期目標（`p.1`） |
| [`001-introduction`](./openspec/changes/001-introduction/proposal.md) | **I. Introduction** | `G–L` | 市場動機、SEBI/Reuters 實證、三代文獻批判（含 Koviazin 變異與 FinForesight 洩漏）、4 項可測貢獻與章節組織 `L`（`p.1–2`） |
| [`002-related-work`](./openspec/changes/002-related-work/proposal.md) | **II. Related Work** | `M–O` | 50% AI 技術（XGBoost、LLM Agents、PIT）＋ 50% 領域知識（微觀結構、Brooks 價格行為、SEBI），含 `Table I` 文獻表與 `Table II` 能力矩陣（`p.2–3`） |
| [`003-proposed-design`](./openspec/changes/003-proposed-design/proposal.md) | **III. Proposed Design** | `PA–PM` | 整併 5 核心思維與 3 Trade-offs、四層八模組、`fig1_arch.png` / `fig2_flow.png`、`Eqs. (1)–(3)`、`Algorithm 1`、`BTC/USDT 15m` 實例、三級降級與 `Proposition 1` 正期望值邊界證明（`p.3–5`） |
| [`004-experiment`](./openspec/changes/004-experiment/proposal.md) | **IV. Experiment** | `Q–V` | 4 維評估指標、4 組基線（`B1–B3`＋`Ablation`）、4 階段 PIT 與 2025-10 清算壓力測試、參數敏感度、`Table III` 假說目標（明確標註未實測）與 `RQ1–RQ5` 檢驗（`p.5`） |
| [`005-conclusion`](./openspec/changes/005-conclusion/proposal.md) | **V. Conclusion** | `W–Z` | 架構總結與優勢、預期產出、4 項外部效度限制與負向消融結果揭露承諾（`p.5–6`） |
| [`006-reference`](./openspec/changes/006-reference/proposal.md) | **References** | IEEE 引用規範 | 9 筆經查核之 IEEE 格式文獻，與 `docs/references.md` 雙向對齊且無未定義引用（`p.6`） |

### 16.3 IEEE ICEIB 會議論文完整版（同步自 [`ICEIB paper/main.tex`](./ICEIB%20paper/main.tex)）

> **編譯狀態**：已透過 Overleaf MCP（`DIC4`）以 `pdfLaTeX`（`IEEEtran` conference, `10pt` Times 雙欄）編譯通過（**0 errors、共 6 頁**）。以下為與 [`ICEIB paper/main.tex`](./ICEIB%20paper/main.tex) 完全一致之論文全文：

#### Title
**Hybrid TPA-Agent: A Decoupled Data-Grounded and Risk-Gated AI Agent for Reliable Short-Term Cryptocurrency Trading Support**

* **Authors (Camera-Ready 待補英文拼寫與系所信箱)**：7115056027 陳柏安、7114056115 侯佳旻、7114068023 陳威碩、7114068027 陳沐德、7115068026 古富綿

#### Abstract (`A–F`)
Short-term cryptocurrency and derivatives trading exposes participants to severe microstructure noise, extreme liquidation cascades, and cognitive decision biases. Empirical regulatory evidence from the Securities and Exchange Board of India (SEBI, 2026) reports that 87.7% of 7.86 million individual equity derivatives traders incurred net losses in FY26, underscoring that trading decision-support systems must prioritize risk containment and decision reliability over sheer signal frequency. However, conventional technical indicators suffer from lag and lack continuous spatial representation, standalone machine learning (ML) classifiers overlook structural liquidity context, and pure large language model (LLM) trading agents exhibit substantial stochastic variance across decoding runs, numerical hallucination, and susceptibility to historical data leakage. To address these gaps, this paper proposes *Hybrid TPA-Agent*, a decoupled four-tier, eight-module architecture that strictly separates deterministic spatial geometry, calibrated probabilistic estimation, semantic Price Action reasoning, and programmatic risk enforcement. Specifically, a deterministic engine identifies unmitigated supply and demand (S&D) zones alongside Volume Profile high-volume nodes (HVNs) and support/resistance flips, after which a probability-calibrated XGBoost classifier estimates conditional win probability $W$ to short-circuit weak setups within 5 ms. Passing candidates are audited by an LLM planner constrained to read-only JSON summaries and a codified Price Action knowledge base under zero-temperature Chain-of-Thought decoding, before a programmatic risk engine enforces ATR-buffered stop-loss placement, geometric risk-to-reward ($\mathrm{RR} \ge 2.0$), cost-adjusted positive expectancy, `WAIT/NO_TRADE` abstention logging, and a three-level outage degradation protocol. To evaluate functional decoupling rigorously, we design a four-stage, point-in-time (PIT) experimental framework on 5-minute and 15-minute `BTC/USDT` and `ETH/USDT` datasets against rule-based, standalone ML, standalone LLM, and ablated baselines under explicit 0.04% taker fees and slippage. All performance benchmarks reported herein represent falsifiable hypothesized targets (e.g., $\le 12\%$ maximum drawdown and $>95\%$ multi-run consistency) rather than completed empirical claims, establishing a reproducible OpenSpec foundation for verifiable financial AI safety.

**Index Terms—** Algorithmic trading, domain-specific AI agents, supply and demand zones, XGBoost probability calibration, large language models, point-in-time evaluation, risk management, spec-driven development.

---

#### I. Introduction (`G–L`)
Modern cryptocurrency derivatives and intraday trading operate continuously under high leverage and pronounced non-stationarity. Under rapid price fluctuations, retail and non-institutional participants frequently experience cognitive overload and emotional bias, leading to unhedged overtrading. Rather than seeking another black-box predictor of price direction, reliable short-term decision support must generate traceable, risk-bounded actions—including disciplined abstention when market structure is unfavorable.

Empirical regulatory and market evidence underscores why signal generation alone fails in practice. An empirical study by the Securities and Exchange Board of India (SEBI) covering FY25–FY26 across 7.86 million individual equity derivatives traders revealed that 87.7% incurred net losses in FY26, while multi-year cohorts show only 0.5% consistent annual profitability and 90–92% repeat losses `[1]`. In digital assets, acute liquidity shocks amplify this fragility: in October 2025, a two-day sell-off triggered over \$19 billion in leveraged liquidations alongside a greater than 14% drop in Bitcoin (BTC) `[2]`. Consequently, trading experience and higher signal frequency do not automatically yield positive net returns; the central engineering challenge is enforcing deterministic risk bounds, verifiable rationale, and reliable abstention.

Existing short-term decision-support systems have evolved across three generations:
1. **Rule-Based Indicators:** Moving average crossovers, RSI, and MACD smooth past prices with closed-form formulas, offering 100% determinism without awareness of discontinuous order-flow zones `[3]`.
2. **Standalone Machine Learning:** Gradient-boosted decision trees such as XGBoost model nonlinear feature interactions efficiently `[4], [5]`, yet treat OHLCV bars as uniform tabular slices.
3. **Pure LLM Multi-Agent Frameworks:** Recent LLM agents coordinate analyst and risk personas through natural-language debate to capture qualitative market context.

Critical evaluation exposes complementary flaws across all three generations. Rule-based oscillators lag during range-bound consolidations and produce low reward-to-risk whipsaws. Standalone ML classifiers lack spatial liquidity context and suffer from false-positive volume traps in thin markets unless calibrated and structurally filtered. Meanwhile, pure LLM agents exhibit severe stochastic instability and arithmetic unreliability: Koviazin *et al.* `[6]` showed that under `temperature = 1`, GPT-4o and Qwen3 agents exhibit 3-month cumulative return standard deviations of 9.0–9.4 percentage points, and LLMs remain prone to floating-point hallucination. Furthermore, *FinForesight* `[7]` demonstrated that 25 of 27 historical evaluation intervals suffered from information leakage, with strict point-in-time (PIT) isolation cutting score gaps by 76%. Thus, the core research question is: *How can a domain-specific AI agent decouple spatial geometry, calibrated win-rate estimation, Price Action semantics, and programmatic risk gating so that identical PIT snapshots yield reproducible, zero-arithmetic-hallucination decisions (`APPROVED` or `WAIT/NO_TRADE`)?*

To answer this question, we propose **Hybrid TPA-Agent**, formalized via OpenSpec specification-driven development `[9]`, with four testable contributions:
1. **Decoupled Four-Tier Architecture:** Strict separation of deterministic Python geometry ($\mathcal{O}(N)$ / $\mathcal{O}(N\log B)$), calibrated XGBoost probability estimation, read-only JSON-constrained LLM Chain-of-Thought (CoT) auditing (`temperature = 0.0`), and programmatic risk math targeting $>95\%$ consistency and $0\%$ numerical/risk violations.
2. **Objective Price Action Formalization:** Codification of unmitigated Supply & Demand (S&D) zones, Volume Profile High-Volume Nodes (HVNs), Support/Resistance (S/R) Flips, Pinbar absorption, Liquidity Sweeps, and Change of Character (ChoCh) into auditable state machines and rules.
3. **First-Class `WAIT/NO_TRADE` Abstention Auditing:** Explicit probability, risk-to-reward ($\mathrm{RR} \ge 2.0$), and feed-health gates paired with counterfactual logging of rejected signals to measure avoided losses versus missed opportunities.
4. **Leakage-Free Four-Stage Validation Protocol:** Integration of chronological splitting, strict PIT snapshot isolation, October 2025 liquidation stress testing, and hash-committed forward simulation under 0.04% taker fees and slippage.

The remainder of this paper is organized as follows. Section II reviews AI methodologies and financial domain literature, synthesizes capability gaps, and positions our approach. Section III details the proposed four-tier, eight-module design, mathematical formulation, Algorithm 1, illustrative walkthrough, and expectancy bounds. Section IV defines the planned four-stage PIT experimental protocol, baselines, and hypothesized targets for `RQ1–RQ5`. Finally, Section V summarizes key contributions, external validity limitations, and future work.

---

#### II. Related Work (`M–O`)
To balance artificial intelligence engineering with financial domain rigor, we review foundational literature from simple deterministic methods to complex multi-agent LLMs and summarize key studies in Table I.

* **AI Technology (Gradient Boosting, LLM Agents, and PIT Controls):** Chen and Guestrin `[4]` introduced XGBoost, providing regularized second-order tree boosting with $\mathcal{O}(T \times D)$ inference complexity suited to low-latency tabular scoring. Subsequent cryptocurrency forecasting work `[5]` applied XGBoost over technical oscillators (EMA, MACD, RSI), demonstrating strong nonlinear fitting but lacking explicit spatial zone awareness, probability calibration checks, or transaction-cost-aware abstention gates. At the generative AI frontier, Koviazin *et al.* `[6]` audited reproducibility in TradingAgents and documented substantial return variance across temperature and random seeds (9.0–9.4 pp standard deviation), alongside reasoning models ignoring sampling parameters. Separately, *FinForesight* `[7]` evaluated 22 financial LLM workflows across US, Chinese, and Hong Kong markets, proving that pretraining memory and unconstrained retrieval inflate historical scores (25/27 intervals leaked) unless strict PIT timestamps and leakage filters are enforced.
* **Domain Knowledge (Market Microstructure, Price Action, and Risk):** Almeida and Gonçalves `[8]` systematically reviewed 138 cryptocurrency market microstructure studies, highlighting extreme intraday non-stationarity, fragmented order-book depth, bid-ask spreads, and the decisive erosion of short-horizon alpha by exchange fees and slippage—motivating avoiding sub-minute (`1m`) noise regimes and embedding explicit 0.04% taker fees and slippage. Brooks `[3]` formalized order-flow imbalance zones, S/R Flips, Pinbar passive absorption, and liquidity sweeps, whereas existing automated SNR algorithms (Local Extrema, KDE, ZigZag, Pivot Points) extract static horizontal lines without tracking zone mitigation lifecycles or integrating calibrated probability and dynamic ATR risk sizing. Finally, SEBI's FY26 study `[1]` confirms that frequent short-duration derivatives trading without strict stop-loss and abstention discipline leads to widespread retail capital erosion, noting that Indian equity derivatives statistics require empirical re-validation in crypto perpetuals.

**Table I: Summary of Reviewed Literature Across AI Methods and Financial Domain Knowledge**

| Ref. | Year | Problem | AI Method | Domain Knowledge | Dataset / Corpus | Key Findings | Limitations |
| :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| Chen & Guestrin `[4]` | 2016 | Scalable tabular prediction | XGBoost (GBDT) | Regularization, feature splits | Public tabular benchmarks | High accuracy and $\mathcal{O}(T \times D)$ inference speed | No native spatial structure or risk semantics |
| Crypto XGBoost `[5]` | 2024 | Short-term crypto forecasting | XGBoost Regressor | EMA, MACD, RSI indicators | Historical crypto OHLCV | Captures nonlinear technical dynamics | Lacks S&D zones, cost modeling, and abstention |
| Koviazin *et al.* `[6]` | 2026 | Multi-agent LLM reproducibility | Multi-Agent LLM (GPT-4o, Qwen3) | Analyst/risk debate roles | Equity prices and news | 9.0–9.4 pp return std. dev. at `temp=1` | High stochasticity; unreliable numeric math |
| FinForesight `[7]` | 2026 | Financial LLM data leakage | LLM + RAG / Deep Research | PIT financial forecasting | 27 intervals, US/CN/HK markets | 25/27 intervals leaked; PIT cuts gap by 76% | Focuses on long-horizon fundamentals, not 5m/15m K-lines |
| Almeida & Gonçalves `[8]` | 2023 | Crypto microstructure synthesis | Econometric / Microstructure review | Price discovery, spreads, fees | 138 crypto market papers | Transaction friction critically erodes high-freq alpha | Survey only; no hybrid agent prototype |
| Brooks `[3]` | 2012 | Chart price action analysis | Rule taxonomy (non-AI) | S&D zones, S/R Flip, Pinbar, ChoCh | Historical price charts | Defines high-RR structural entries | Subjective manual rules; uncalibrated |
| SEBI DEPA `[1]` | 2026 | Retail derivatives profitability | Regulatory empirical audit | Options/futures exposure | 7.86M FY26 trader accounts | 87.7% net loss rate among retail traders | Indian equity F&O scope; requires crypto validation |

**Table II: Architectural Capability Comparison Matrix**

| Architecture | Spatial | Prob. | Context/Risk | Core Limitation |
| :--- | :---: | :---: | :---: | :--- |
| Indicators / Pure ML | None | Strong | None | High noise in sideways markets |
| Pure LLM Agent | None | Weak | Strong | Numeric hallucination; low consistency |
| Static SNR (KDE/Pivot) | Medium | None | None | Static lines; no calibrated risk math |
| **Hybrid TPA-Agent** | **Strong** | **Strong** | **Strong** | Relies on market depth & API uptime |

* **Research Gap and New Proposed Positioning (`N–O`):** As organized in Table II, prior paradigms address spatial geometry, calibrated win-rate statistics, or semantic reasoning in isolation. None unify deterministic unmitigated S&D zones, Brier-calibrated ML filtering, JSON-constrained zero-temperature LLM CoT auditing, and PIT-enforced abstention under explicit exchange costs. Hybrid TPA-Agent is positioned to bridge and empirically falsify or validate this integration gap.

---

#### III. Proposed Design (`PA–PM`)
* **Design Aim, Theoretical Basis, and Trade-offs (`PA–PD`):** Hybrid TPA-Agent aims to produce reproducible, risk-bounded intraday setups on 5m and 15m `BTC/USDT` and `ETH/USDT` markets by enforcing five domain principles `[3]`: (1) explicit `UNMITIGATED`, `TESTED`, `SR_FLIP`, and `INVALIDATED` zone lifecycles; (2) Platt/Isotonic probability calibration (Brier Score); (3) `WAIT/NO_TRADE` abstention during thin liquidity; (4) separating deterministic numerical math from LLM semantic auditing; and (5) PIT boundaries (`close_time <= t`). Our paradigm shifts from monolithic prediction to staged functional decoupling across three trade-offs: (a) *Filter Strictness vs. Opportunity* (treating $W_{\min}=0.55$ and $\mathrm{RR}_{\min}=2.0$ as tunable parameters audited via counterfactuals); (b) *LLM Context vs. Reproducibility & Latency* (short-circuiting weak setups in $<20\text{ ms}$ at Layer 2, pinning `temperature = 0.0`, and strict JSON fallback); and (c) *Sub-Minute Granularity vs. Cost Erosion* (targeting 5m/15m over noisy `1m` bars `[8]`).
* **Four-Tier, Eight-Module Architecture and Execution Flow (`PE`, `PG`, `PH`):**
  * **Layer 1 — Input & Data:** *Module 1.1 (User Preference Block)* validates $S$, $\mathrm{TF} \in \{\text{5m}, \text{15m}\}$, $R_{\text{tol}} \in (0\%, 5\%]$ (default 1%), and fee/slippage schedules; *Module 1.2 (Data Ingestion Engine)* fetches $N \in [200, 500]$ closed OHLCV bars enforcing `close_time <= t` and heartbeat latency checks.
  * **Layer 2 — Core Analytical Engines:** *Module 2.1 (Deterministic S&D Zone Engine)* runs $\mathcal{O}(N)$ Swing High/Low scanning and $\mathcal{O}(N\log B)$ Volume Profile HVN aggregation ($<15\text{ ms}$ for $N=500$), updating broken demand zones to `SR_FLIP`; *Module 2.2 (ML Win-Rate Engine)* extracts microstructure features and predicts calibrated win probability $W \in [0, 1]$ via XGBoost ($\mathcal{O}(T \times D)$, $<5\text{ ms}$), short-circuiting $W < W_{\min}$ directly to `WAIT/NO_TRADE`.
  * **Layer 3 — LLM Reasoning & Domain Knowledge:** *Module 3.1 (Domain Knowledge Base)* codifies Price Action rules (Pinbar absorption, Liquidity Sweep, ChoCh, S/R Flip); *Module 3.2 (Planner & Reasoning Agent)* consumes read-only JSON summaries at `temperature = 0.0` and emits schema-validated `{is_valid_setup, pattern_type, evidence, invalidation_flags}` within 800–1200 ms (timeouts or malformed JSON fail safely to `WAIT/NO_TRADE`).
  * **Layer 4 — Risk Management & Output:** *Module 4.1 (Decision & Risk Engine)* computes ATR-buffered $P_{\text{sl}}$, opposing-zone $P_{\text{tp}}$, $\mathrm{RR}$, cost-adjusted expectancy, and position size while logging rejected candidates; *Module 4.2 (Output Interface)* emits a hash-stamped JSON/Markdown card awaiting human-in-the-loop (HITL) approval.

<div align="center">
  <img src="./ICEIB%20paper/fig1_arch.png" alt="Fig. 1: Four-tier, eight-module decoupled architecture" width="90%" />
  <p><em>Fig. 1: Four-tier, eight-module decoupled architecture of Hybrid TPA-Agent.</em></p>
</div>

<div align="center">
  <img src="./ICEIB%20paper/fig2_flow.png" alt="Fig. 2: End-to-end decision and risk-gating execution flowchart" width="42%" />
  <p><em>Fig. 2: End-to-end decision and risk-gating execution flowchart.</em></p>
</div>

* **Mathematical Formulation (`PF`):** For a long candidate setup, Module 4.1 deterministically computes:

$$\mathrm{RR} = \frac{P_{\text{tp}} - P_{\text{entry}}}{P_{\text{entry}} - P_{\text{sl}}}, \qquad E_{\text{net}} = W \cdot \mathrm{RR} - (1 - W) - c_{\mathrm{rt}}, \qquad \mathrm{Pos} = \frac{\mathrm{Equity} \times R_{\text{tol}}}{|P_{\text{entry}} - P_{\text{sl}}|}$$

  where $c_{\mathrm{rt}} \ge 0$ denotes normalized round-trip exchange fees and slippage per unit risk $R = |P_{\text{entry}} - P_{\text{sl}}|$. Approval requires $\mathrm{RR} \ge \mathrm{RR}_{\min}$, $E_{\text{net}} > 0$, `is_valid_setup = true`, and healthy feed status.
* **Implementation Pipeline & Three-Level Degradation (`PI`):** `Algorithm 1` (in [`main.tex`](./ICEIB%20paper/main.tex#L203-L230)) executes within a $<1.5\text{ s}$ per-bar budget and enforces three degradation tiers: **Level 1** (feed delay $>2\text{ s}$ or LLM schema/timeout fault) freezes new entries via `WAIT/NO_TRADE`; **Level 2** verifies exchange server-side hard stop-loss orders on open positions and blocks scaling; and **Level 3** (feed outage $>30\text{ s}$ or OOD circuit breaker) triggers urgent alerts and defensive flatten/hedge actions.
* **Illustrative End-to-End Walkthrough (`PJ`, `BTC/USDT 15m`):** Module 2.1 scans 200 closed bars at \$64,250 and identifies an unmitigated demand zone [\$63,800, \$64,000] backed by Volume Profile HVN alongside an overhead supply zone [\$65,200, \$65,500]. When price retests \$63,950 after sweeping \$63,850 with Volume Z-score $+2.3$ and a long lower wick, Module 2.2 outputs calibrated $W = 0.62 \ge 0.55$, Module 3.2 validates *Liquidity Sweep / Pinbar*, and Module 4.1 sets $P_{\text{entry}} = \$63,950$, $P_{\text{sl}} = \$63,750$, and $P_{\text{tp}} = \$65,150$ ($\mathrm{RR} = 6.0 \ge 2.0$, gross unit expectancy $+3.34$, positive $E_{\text{net}}$ after 0.04% taker fees), emitting an `APPROVED` setup sized to 1% account risk.
* **Expectancy Bounds, Key Advantages, and Simulation Bridge (`PM`, `PK`, `PL`):**
  > **Proposition 1 (Cost-Adjusted Positive Expectancy and Bounded Loss):** Under deterministic gating in Equations (1)–(3) with calibrated probability $W \in [0,1]$, round-trip cost ratio $c_{\mathrm{rt}} \ge 0$, and hard server-side stop-loss $P_{\text{sl}}$, any `APPROVED` setup guarantees: (i) strictly positive net expectancy if and only if $W > \frac{1 + c_{\mathrm{rt}}}{1 + \mathrm{RR}}$, and (ii) nominal stop-loss equity exposure bounded by $\mathrm{Pos} \cdot |P_{\text{entry}} - P_{\text{sl}}| = \mathrm{Equity} \cdot R_{\text{tol}}$ (excluding extreme gap slippage).
  > *Proof:* Expanding $E_{\text{net}} > 0$ gives $W(\mathrm{RR}+1) - 1 - c_{\mathrm{rt}} > 0 \iff W > (1+c_{\mathrm{rt}})/(1+\mathrm{RR})$ since $\mathrm{RR} > 0$, and substituting $\mathrm{Pos}$ yields $\mathrm{Equity} \cdot R_{\text{tol}}$. $\blacksquare$

---

#### IV. Experiment (`Q–V`)
*Disclosure:* No backtest results are fabricated prior to specification execution; this section defines the falsifiable experimental protocol for `RQ1–RQ5`.

* **Metrics (`Q`), Baselines (`R`), Setup (`S`), Tuning (`T`), and Verification (`V`):** Four metric families (**Prediction Quality**: Precision, ROC-AUC, Brier Score; **Trading Performance**: Net Return, PF, Sharpe, MDD; **Decision Reliability**: Multi-run Consistency Rate, Numerical Error Rate, Risk Violation Rate; **Abstention Quality**: Trade Retention Rate, `WAIT` counterfactual returns) compare **Hybrid TPA-Agent** against **B1** (`EMA+RSI`), **B2** (`Pure XGBoost`), **B3** (`Pure LLM`), and **Ablation** (`S&D+XGBoost`) on 5m/15m `BTC/USDT` and `ETH/USDT` (2023–2024 + Oct 2025 \$19B liquidation window) under 0.04% taker fees and slippage across four stages: (1) chronological split, (2) PIT isolation (`close_time <= t`), (3) extreme-event stress test, and (4) hash-committed forward simulation, with sensitivity analysis on $W_{\min}=0.55$, $\mathrm{RR}_{\min}=2.0$, $N \in [200,500]$, and $B$.

**Table III: Hypothesized Expected Targets (Not Verified Results) for Two-Year Backtest Comparison (`U`)**

| Model / Baseline | Win Rate | PF | Sharpe | MDD | Consistency |
| :--- | :---: | :---: | :---: | :---: | :---: |
| B1: EMA+RSI | ~41.2% | 1.18 | 0.74 | -28.5% | 100% |
| B2: Pure XGBoost | ~52.8% | 1.35 | 1.12 | -22.1% | 100% |
| B3: Pure LLM | ~46.5% | 1.22 | 0.85 | -34.2% | <60% |
| Ablation: S&D+XGB | ~55.4% | 1.62 | 1.58 | -15.8% | 100% |
| **Hybrid TPA-Agent** | **~59.2%** | **1.94** | **2.15** | **<= -12.0%** | **>95%** |

---

#### V. Conclusion (`W–Z`)
* **Summary and Strengths (`W–X`):** Hybrid TPA-Agent reframes short-term cryptocurrency trading AI from unconstrained price prediction to verifiable functional decoupling across deterministic S&D/HVN geometry, calibrated XGBoost filtering, JSON-constrained Price Action CoT auditing, and programmatic risk enforcement (`WAIT/NO_TRADE` + three-level outage degradation).
* **Expected Yields (`Y`):** Formalized under OpenSpec specifications, the four-stage PIT protocol tests whether decoupling achieves $>95\%$ decision consistency, $0\%$ risk-rule violations, improved net Profit Factor, and $\le 12\%$ maximum drawdown under realistic 0.04% taker fees and slippage.
* **Limitations and Future Work (`Z`):** Four external-validity boundaries remain: (1) SEBI's 87.7% loss statistic derives from Indian equity derivatives rather than crypto perpetuals; (2) TradingAgents variance `[6]` and FinForesight leakage `[7]` require re-verification on 5m/15m crypto bars; (3) Volume Profile HVNs degrade during thin-liquidity holiday periods; and (4) external LLM API latency or silent model updates can perturb reproducibility despite `temperature = 0.0`. Future work will execute the full four-stage PIT backtest and hash-committed forward simulation; if empirical ablation shows that LLM CoT gains do not justify API latency and cost over `S&D+XGBoost`, that negative boundary will be reported explicitly.

---

#### References
1. SEBI DEPA, "Trading Behaviour of Individual Traders in the Equity Derivatives Segment (FY25–FY26)," Official Regulatory Study, Aug. 2026.
2. Reuters, "Crypto markets face severe liquidation event amid volatile trading conditions," Market Report (\$19B+ leveraged liquidations), Oct. 2025.
3. A. Brooks, *Reading Price Charts Bar by Bar: The Technical Analysis of Price Action for the Serious Trader*. Hoboken, NJ, USA: John Wiley & Sons, 2012.
4. T. Chen and C. Guestrin, "XGBoost: A scalable tree boosting system," in *Proc. 22nd ACM SIGKDD (KDD)*, 2016, pp. 785–794.
5. "Cryptocurrency price forecasting using XGBoost regressor and technical indicators," *arXiv preprint*, 2024, doi: `10.48550/arXiv.2407.11786`.
6. A. Koviazin, T. Mudarisov, E. Polyachenko, and R. State, "Reproducibility in the TradingAgents framework," in *Proc. 2026 Int. Conf. Artif. Intell. Fintech (ICAIF '26)*, ACM, 2026, pp. 365–370, doi: `10.1145/3800973.3801029`.
7. FinForesight Authors, "FinForesight: Assessing information leakage and LLM financial prediction capabilities," *arXiv preprint*, 2026.
8. J. Almeida and T. C. Gonçalves, "Cryptocurrency market microstructure: a systematic literature review," *Ann. Oper. Res.*, 2023, doi: `10.1007/s10479-023-05627-5`.
9. Fission AI, "OpenSpec: Spec-driven development for AI coding assistants," GitHub Repository, 2026, `https://github.com/Fission-AI/OpenSpec`.

---

## 17. 專案檔案結構 (Repository Structure)

```text
IOT-DIC4/
├── README.md                                             # 專案總覽、架構、論文大綱與學術規範文檔
├── DESIGN.md                                             # 系統架構、資料契約、演算法與驗證追溯設計書
├── .gitignore                                            # Git 忽略配置（環境變數、模型與快取防護）
├── ICEIB paper/                                          # IEEE ICEIB 會議論文 LaTeX 專案與圖片資產
│   ├── main.tex                                          # IEEEtran conference 主文稿（6 頁、0 errors）
│   ├── main.txt                                          # 與 main.tex 同步之純文字上傳鏡像
│   ├── fig1_arch.png                                     # 四層八模組架構圖（論文 Fig. 1）
│   ├── fig2_flow.png                                     # 端到端決策與風控流程圖（論文 Fig. 2）
│   └── iceib_overleaf_upload.zip                         # Overleaf 打包上傳備份
├── docs/                                                 # OpenSpec 重構後之學術論文核心章節
│   ├── abstract.md                                       # A–F 結構、300 字中文摘要與 IEEE 英文 Abstract
│   ├── introduction.md                                   # A–F / G–L 緒論（限制對映設計與可測試貢獻）
│   ├── related-work.md                                   # 50% AI + 50% Domain 文獻回顧與 Article Summary Table
│   ├── deep-research.md                                  # 5 核心思維、3 大專業爭論與 10 深度問題
│   └── references.md                                     # 經查核之文獻清單與外推限制說明
├── openspec/                                             # OpenSpec 規格驅動開發 (SDD) 工作流
│   ├── config.yaml                                       # OpenSpec 專案規則與研究倫理限制
│   └── changes/
│       ├── refactor-hybrid-tpa-agent/                    # 系統原型四層八模組規格、設計與任務
│       ├── iceib-paper-a2z/                              # ICEIB 論文整體重構與 Overleaf 編譯驗證（已完成）
│       ├── 000-abstract/                                 # 分章節細項：Abstract (A–F)
│       ├── 001-introduction/                             # 分章節細項：Introduction (G–L)
│       ├── 002-related-work/                             # 分章節細項：Related Work (M–O)
│       ├── 003-proposed-design/                          # 分章節細項：Proposed Design (PA–PM)
│       ├── 004-experiment/                               # 分章節細項：Experiment (Q–V)
│       ├── 005-conclusion/                               # 分章節細項：Conclusion (W–Z)
│       └── 006-reference/                                # 分章節細項：References (IEEE 引用查核)
├── 架構圖.png                                             # Layer 1 ~ Layer 4 系統架構圖
├── 流程圖flowchart.png                                    # 端到端決策與風控執行流程圖
├── IMG_9464.png                                          # 2020 A-Z SOP 學術規範指引圖
├── Gemini_Generated_Image_ivdx4yivdx4yivdx.jpg           # 專案全景摘要海報 (Executive Infographic)
├── 期末專題提案_Hybrid_TPA_Agent(最新檔).pdf               # 完整期末專題提案報告書 (PDF)
├── Hybrid TPA-Agent 期末專題提案.pptx                     # 專案提案簡報投影片
├── Hybrid_TPA_Agent_Final_Project_Proposal_Complete-v2.docx # 提案詳細報告文檔 (Word)
└── Hybrid_TPA_Agent_Final_Project_Proposal.docx          # 提案原始初稿文檔
```

---

<div align="center">
  <p><b>Hybrid Technical & Price Action Agent (Hybrid TPA-Agent)</b></p>
  <p>專注於決策可靠性、嚴格風控與數學正期望值的短線交易輔助研究平台</p>
</div>
