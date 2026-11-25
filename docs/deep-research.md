# Deep Research — Hybrid TPA-Agent

> 本章依循 **期末專題提案作業規範第三章（Deep Research）** 補齊為完整架構：**五大核心思維模式（5 Core Mindsets）**、**三大重要專業爭論（3 Trade-offs / Controversies）** 與 **十大深度理解問題（10 Deep Understanding Questions）**。

---

## 1. 五個核心思維模式 (Five Core Mental Models)

### Mindset 1：供需失衡與拍賣市場微觀結構思維 (Order-Flow & Auction Market Microstructure)
- **意義**：市場價格並非隨機漫步的純數值曲線，而是買賣雙方未成交限價單（Resting Liquidity）與市價單持續拍賣撮合的結果；重大價格位移源自特定價位區間的機構供需失衡（Supply/Demand Zones）。
- **證據**：拍賣市場理論與價格行為學（Brooks, 2012）及加密貨幣微觀結構研究（Almeida & Gonçalves, 2023/2024）均顯示，高成交量節點（HVN）與未回測結構邊界會形成顯著的流動性聚集與支撐壓力反轉（S/R Flip）。
- **對系統設計的影響**：驅動 **Module 2.1 (`Deterministic S&D Zone Engine`)** 捨棄滯後均線，直接以雙指針 Swing High/Low 與 Volume Profile HVN 建立可追蹤 `UNMITIGATED` / `TESTED` / `SR_FLIP` 狀態機的空間座標系統。

### Mindset 2：數學正期望值與機率校準思維 (Positive Expectancy & Probability Calibration)
- **意義**：短線交易不存在 100% 勝率預測；單筆策略是否值得參與取決於條件勝率 $W$、幾何風險報酬比 $RR$ 與交易成本共同構成的期望值 $E = (W \times RR) - ((1 - W) \times 1.0) - \text{Cost} > 0$。若 $W$ 未經機率校準（Calibration），期望值公式將失去數學意義。
- **證據**：梯度提升樹（Chen & Guestrin, 2016）輸出之原始置信分數常有過擬合或偏差；同時加密市場 Taker 手續費（`0.04%`）與滑點會在高頻交易中迅速侵蝕微薄邊際利潤。
- **對系統設計的影響**：驅動 **Module 2.2** 引入 Platt/Isotonic 校準與 Brier Score 檢核，並於 **Module 4.1** 以待測雙門檻（候選 $W \ge 0.55$、$RR \ge 2.0$）與單筆風險百分比 `R_tol` 計算倉位。

### Mindset 3：決策可靠性與「不交易 (`WAIT`)」優先思維 (Decision Reliability & Abstention First)
- **意義**：在雜訊主導的盤面中，「拒絕低品質訊號」比「捕捉每一次波動」更能決定長期生存率。系統價值來自風險曝險紀律，而非訊號數量。
- **證據**：SEBI（2026）實證顯示 FY26 高達 **87.7%** 個人衍生品交易者虧損，且頻繁短週期操作加劇資本消耗；Reuters（2025-10）兩日逾 190 億美元清算事件更證明極端行情下不當開倉的毀滅性。
- **對系統設計的影響**：將 **`WAIT/NO_TRADE`** 設計為一等決策狀態（於 `W < W_min`、`RR < RR_min`、結構無效或 API 延遲 `> 2s` 時強制觸發），並要求實驗追蹤被拒絕訊號的反事實結果（`RQ4`）。

### Mindset 4：數值運算與語意推理徹底解耦思維 (Numerical-Semantic Decoupling)
- **意義**：確定性數學演算法、統計機率模型與大型語言模型各有嚴格的能力邊界；必須讓確定性程式掌握所有浮點幾何與風控數值，讓 LLM 僅負責離散的結構語意推理。
- **證據**：Koviazin et al.（ACM ICAIF '26）證實純 LLM 代理人受解碼隨機性影響嚴重（`temperature=1` 時報酬標準差達 `9.0–9.4` 個百分點），且 LLM 原生並不擅長精確浮點座標運算。
- **對系統設計的影響**：建立四層單向唯讀資料契約（`ZoneRecord` → `MLPrediction` → `AgentReasoning` → `DecisionReport`），禁止 LLM 修改或生成價格、停損點與倉位大小，並固定 `temperature = 0.0` 以追求 `> 95%` 一致率。

### Mindset 5：嚴格時點控制與無洩漏實證思維 (Point-in-Time & Leakage-Free Empirical Rigor)
- **意義**：任何未隔離未來資訊（Look-ahead Bias）或未扣除真實摩擦成本的回測績效均不可信；研究系統必須具備可否證性，能如實揭露負向結果與失效邊界。
- **證據**：FinForesight（2026）在 27 組金融時間比較中發現 25 組存在歷史洩漏偏差，實施 PIT 過濾後評估分數差距縮減 **76%**。
- **對系統設計的影響**：驅動 **Module 1.2** 強制執行 `bar.close_time <= t` 快照隔離，並要求實驗必須包含四階段驗證（時序切分、PIT、2025-10 壓力測試、雜湊鎖定前瞻模擬）。

---

## 2. 三個最重要的專業爭論 (Three Major Controversies & Trade-offs)

### Controversy 1：高訊號覆蓋率 vs. 嚴格濾網與不交易紀律 (`Trade Frequency vs. Capital Preservation`)
- **觀點 A（高覆蓋派）**：短線市場機會轉瞬即逝，過高門檻（如 `W >= 0.55` 且 `RR >= 2.0`）會錯失大量中勝率趨勢波段，降低資金周轉率。
- **觀點 B（風控優先派）**：短線震盪雜訊與雙邊手續費/滑點會懲罰過度交易，只有高風報比與高確定性結構才具備扣除成本後的正期望值。
- **雙方證據**：SEBI（2026）指出高頻參與短週期合約的零售交易者 87.7% 虧損（支持 B）；但本專題第 9 題亦指出若門檻過苛（如強求 $W \ge 70\%$），交易頻率會指數衰減至週均 1 筆，嚴重壓抑總期望利潤（支持 A 的擔憂）。
- **目前共識**：濾網門檻不能憑直覺寫死，必須連同被拒絕訊號的後續損益（Counterfactual Outcomes）一起衡量。
- **尚未解決的問題**：在不同波動率體制（高波動單邊 vs. 低波動盤整）下，最適 $(W_{\min}, RR_{\min})$ 邊界應如何自適應校準而不產生過擬合？

### Controversy 2：LLM 語意彈性推理 vs. 金融決策重現性與延遲代價 (`Contextual Flexibility vs. Reproducibility & Latency`)
- **觀點 A（LLM 賦能派）**：LLM 能結合 Price Action 語意知識（如流動性掠奪後的收盤收回型態、宏觀/黑天鵝脈絡），過濾純數值 ML 無法辨識的結構陷阱。
- **觀點 B（純確定性派）**：LLM 推理延遲高（`800–1200 ms`）、具 API 成本與版本漂移風險，即使設為 `temperature=0` 在外部閉源模型更新後仍可能產生非預期變異，應僅用確定性規則與 ML。
- **雙方證據**：Koviazin et al.（2026）證實 LLM 交易決策變異顯著（支持 B）；但純 ML 在低流動性假突破（Volume Trap）與分佈外黑天鵝中缺乏結構防禦力（支持 A）。
- **目前共識**：LLM 不得執行數值計算或直接下單，僅應在低延遲確定性層（`< 20 ms`）初篩通過後擔任結構複核角色。
- **尚未解決的問題**：`S&D + XGBoost + LLM` 帶來的風險調整後增量效益（`RQ2`），在扣除 API 成本與延遲後是否穩定超越 `S&D + XGBoost` 消融模型？

### Controversy 3：極短週期高解析度機會 vs. 微觀雜訊與交易成本侵蝕 (`Sub-Minute Granularity vs. Microstructure Friction`)
- **觀點 A（高頻粒度派）**：`1m` 甚至 Tick 級數據提供更早的進場點與理論上極窄的停損空間，能提升資金效率與高頻樣本數。
- **觀點 B（結構週期派）**：`1m` 級別充斥做市商高頻雜訊，供需區結構極度不穩定，且加密貨幣 Taker `0.04%` 雙邊手續費加滑點在窄停損下佔比過高，徹底摧毀期望值。
- **雙方證據**：Almeida & Gonçalves（2023/2024）強調微觀價差與交易成本對日內加密貨幣策略淨收益具決定性影響；過小停損區間使費用佔風險比例急劇上升。
- **目前共識**：非造市商（Non-MM）之方向性散戶/輔助系統應避開 `1m` 雜訊區，採用具結構穩定性的 `5m`/`15m` 週期。
- **尚未解決的問題**：在 `5m` 與 `15m` 之間，不同市場流動性狀態（如亞洲盤 vs. 歐美盤重疊時段）是否需要動態切換週期或調整滑點緩衝？

---

## 3. 十個深度理解問題與邊界檢驗 (Ten Deep Understanding Questions)

1. **[Why] 價格帶量跌破需求區時為何支撐轉為壓力 (S/R Flip)？**  
   - **推理**：買方未成交限價單被空頭全數吞噬，多頭就地套牢；當價格反彈至原區間時，套牢保本賣壓與順勢空單共振形成供給牆。  
   - **驗證標準**：OpenSpec `sd-zone-detection` 測試跌破後狀態是否由 `DEMAND` 轉為 `SR_FLIP`。
2. **[What-if] 低波動環境突然出現大陽線突破，ML 有何偏誤？**  
   - **推理**：量縮期市場深度極淺，微小單即可拉高 Volume Z-score，導致 XGBoost 高估勝率，實則缺乏機構接力而易演變為流動性陷阱。  
   - **驗證標準**：檢查低流動性過濾器與 LLM `invalidation_flags` 是否能標記並拒絕該訊號。
3. **[Trade-off] 縮小時間週期（如改用 1 分鐘 K 線）有何負面影響？**  
   - **推理**：交易機會雖增，但高頻雜訊導致假供需區倍增，且 `0.04%` 雙邊手續費與滑點將嚴重侵蝕期望值。  
   - **驗證標準**：敏感度分析對比 `1m`/`5m`/`15m` 扣除成本後之淨報酬與 Profit Factor。
4. **[Failure Case] 流動性枯竭時成交量分佈為何失效？**  
   - **推理**：假日或低深度時段少量籌碼即造成價格真空跳動，使 Volume Profile HVN 標記出無實質機構訂單支撐的偽區塊。  
   - **驗證標準**：OpenSpec `Thin-Liquidity Zone Rejection` 情境檢查低滾動量區間是否拒絕建區。
5. **[Evidence] 判斷價格回測為「真反彈」抑或「假跌破」之關鍵證據？**  
   - **推理**：三項連動證據：(1) 長下影線刺穿但收盤收回需求區上緣（Pinbar）；(2) 刺穿時放量但實體極小（Passive Absorption）；(3) 次級結構轉變（ChoCh）。  
   - **驗證標準**：Module 3.1 知識庫與 Module 3.2 `evidence` 欄位可追溯比對上述三項條件。
6. **[Comparison] 相較均線交叉策略，供需區在風報比上的核心優勢？**  
   - **推理**：均線具滯後性，進場點遠且停損難緊貼結構邊界（RR 常 `< 1.5`）；供需區於回踩區間邊界切入、停損緊貼外側、目標看向下一反向區間，可構建高幾何 RR。  
   - **驗證標準**：對比 Baseline 1 (`EMA+RSI`) 與 Hybrid TPA-Agent 之平均實現 RR 分布。
7. **[Design Decision] 為何供需區計算必須與 LLM 推理徹底解耦？**  
   - **推理**：LLM 不擅長精確浮點數幾何座標運算且具取樣變異；將空間矩陣交由確定性 Python、僅將唯讀 JSON 交給 LLM 複核，可使數值錯誤率維持 `0%`。  
   - **驗證標準**：對比 Baseline 3 (`Pure LLM`) 與 Hybrid TPA-Agent 之數值錯誤率與多輪一致率（`RQ3`）。
8. **[Real-world Scenario] API 斷流或延遲時系統如何降級？**  
   - **推理**：實施三級降級：Level 1（延遲 `> 2s`）凍結新開倉；Level 2 檢查伺服器端硬性停損單；Level 3（斷流 `> 30s`）觸發告警與防禦性平倉/對沖。  
   - **驗證標準**：OpenSpec `risk-management` 與 `data-ingestion` 模擬斷線/逾時測試皆強制回傳 `WAIT/NO_TRADE`。
9. **[Trade-off] 要求極端高勝率（如 $W \ge 70\%$）有何代價？**  
   - **推理**：濾網過苛使樣本數呈指數下降，資金使用率嚴重低落，總期望利潤反而不如均衡門檻。  
   - **驗證標準**：進行 $W_{\min} \in [0.50, 0.70]$ 閾值掃描並觀察總淨報酬與交易次數曲線。
10. **[Failure Case] 遭遇黑天鵝暴跌時 ML 為何失效？**  
    - **推理**：宏觀巨震引發概念飄移（Concept Drift）與分佈外（OOD）行情，K 線形態尚未反應完畢價格已穿倉，須仰賴硬性停損、風險熔斷與異常降級機制保護本金。  
    - **驗證標準**：Stage 3 以 2025 年 10 月 190 億美元清算極端行情區間進行壓力測試。
