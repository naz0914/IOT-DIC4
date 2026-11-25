# DESIGN.md — Hybrid Technical & Price Action Agent (Hybrid TPA-Agent)

> **版本狀態**：Proposal & OpenSpec Refactoring Stage（規格已定義、尚未進入實作與實證階段）  
> **對應 OpenSpec Change**：[`openspec/changes/refactor-hybrid-tpa-agent/`](./openspec/changes/refactor-hybrid-tpa-agent/proposal.md)  
> **專案團隊**：7115056027 陳柏安、7114056115 侯佳旻、7114068023 陳威碩、7114068027 陳沐德、7115068026 古富綿  
> **專案庫**：`https://github.com/naz0914/IOT-DIC4.git`

---

## 1. Problem & Research Scope

短線加密貨幣合約與日內交易（`BTC/USDT`、`ETH/USDT`，`5m`/`15m` 週期）長期面臨高微觀雜訊、情緒化決策與極端流動性衝擊（例如 Reuters 2025 年 10 月報導之兩日逾 190 億美元強制平倉事件）。監管實證（SEBI, 2026）亦指出高頻個人衍生品交易者虧損比例高達 87.7%，證明單純增加交易訊號或累積交易經驗無法自動轉化為穩定獲利。

現有三大技術路線各自存在結構性缺口：
1. **傳統指標與純 ML**：缺乏不連續空間結構（供需區）座標，震盪行情假訊號頻繁。
2. **純 LLM 金融 Agent**：受生成隨機性影響（Koviazin et al., 2026），易產生浮點數幻覺，且無法保證正數學期望值。
3. **靜態幾何 SNR 演算法**：僅能繪製水平線，缺乏條件勝率估計與動態風控門檻。

**Hybrid TPA-Agent** 的設計目標並非預設複雜模型必定優於簡單基線，而是建立一個**「數值幾何計算、機率估計、語意情境推理、程式化風控」徹底解耦**的可否證研究系統，並在嚴格時點控制（Point-in-Time, PIT）與真實交易成本（Taker `0.04%` + 滑點）下驗證 `RQ1`–`RQ5`。

---

## 2. Requirements & OpenSpec Traceability

本系統之工程需求由 [`openspec/changes/refactor-hybrid-tpa-agent/specs/`](./openspec/changes/refactor-hybrid-tpa-agent/specs/) 定義，涵蓋五大模組規格：

| OpenSpec Capability | 對應核心需求 (Requirements) | 對應研究問題 | 驗證指標 |
| :--- | :--- | :--- | :--- |
| `data-ingestion` | 參數邊界驗證、PIT 快照隔離 (`bar.close_time <= t`)、心跳監控 | `RQ5` | PIT 洩漏測試（0 筆未來資料）、斷線偵測延遲 |
| `sd-zone-detection` | 確定性雙指針極值 + Volume Profile HVN、S/R Flip 狀態機、低流動性過濾 | `RQ1`, `RQ3` | 位元級決定性重現、`N=500` 耗時 `< 15 ms` |
| `winrate-prediction` | 微觀結構特徵萃取、校準後 XGBoost 條件勝率 $W$、`W < W_min` 短路攔截 | `RQ1` | ROC-AUC、Precision、Brier Score、推論 `< 5 ms` |
| `llm-cot-reasoning` | Price Action 知識庫整合、`temperature=0.0` 結構化 JSON CoT、逾時/格式錯誤降級 | `RQ2`, `RQ3` | 重複執行一致率（目標 `> 95%`）、數值錯誤率（目標 `0%`） |
| `risk-management` | ATR 動態 SL/TP、`RR >= 2.0` 與正期望值閘道、`WAIT/NO_TRADE` 反事實追蹤、HITL、三級降級 | `RQ3`, `RQ4`, `RQ5` | Profit Factor、Sharpe、MDD（目標 `≤ 12%`）、被拒絕訊號後續表現 |

---

## 3. System Architecture（四層八模組）

系統架構與視覺化圖表對應 [架構圖.png](./架構圖.png) 與 [流程圖flowchart.png](./流程圖flowchart.png)：

```text
[Layer 1: Input & Data]
  (1.1) User Preference Block ──► (1.2) Data Ingestion Engine (PIT Guard + Heartbeat)
                                            │
                                            ▼  OHLCV Window (N = 200..500, <= t)
[Layer 2: Core Analytical Engines]
  (2.1) Deterministic S&D Zone Engine (Swing H/L + Volume Profile HVN + S/R Flip)
                                            │  demand_zones, supply_zones
                                            ▼
  (2.2) ML Win-Rate Engine (Microstructure Features + Calibrated XGBoost)
                                            │
                     ┌── W < W_min (0.55) ──┴──► Short-circuit: WAIT/NO_TRADE
                     │
                     ▼  W >= W_min
[Layer 3: LLM Reasoning & Domain Knowledge]
  (3.1) Domain Knowledge Base (Pinbar / Sweep / ChoCh / S/R Flip)
                     │
                     ▼
  (3.2) Planner & Reasoning Agent (CoT over JSON only, temperature=0.0)
                     │
                     ▼  {is_valid_setup, evidence, risk_flags}
[Layer 4: Risk Management & Output]
  (4.1) Decision & Risk Engine (ATR SL, Opposing Zone TP, RR >= 2.0, E > 0, Pos, 3-Level Degradation)
                     │
                     ▼
  (4.2) Output Interface (JSON + Markdown Decision Card, HITL Confirmation)
```

---

## 4. Components & Inter-Module Data Contracts

### 4.1 模組職責表

| 編號 | 模組名稱 | 運算性質 | 主要職責 | 延遲預算 |
| :---: | :--- | :--- | :--- | :---: |
| **1.1** | User Preference Block | 確定性驗證 | 設定 `symbol`、`timeframe`、`R_tol`（預設 `1%`，上限 `5%`）、手續費與滑點參數 | `< 1 ms` |
| **1.2** | Data Ingestion Engine | I/O + PIT 守門 | 透過 WebSocket/REST 取得已收盤 OHLCV，嚴格過濾 `close_time > t` 之未來資料並監控心跳 | `< 50 ms` (快取) |
| **2.1** | Deterministic S&D Zone Engine | $\mathcal{O}(N)$ / $\mathcal{O}(N \log B)$ | 雙指針極值掃描、Volume Profile HVN 定位、追蹤 `UNMITIGATED`/`TESTED`/`SR_FLIP`/`INVALIDATED` | `< 15 ms` |
| **2.2** | ML Win-Rate Engine | $\mathcal{O}(T \times D)$ | 計算 Volume Z-score、實體比例、刺穿深度，輸出機率校準後之條件勝率 $W$ | `< 5 ms` |
| **3.1** | Domain Knowledge Base | 結構化規則庫 | 封裝 S/R Flip、Pinbar 被動吸收、Liquidity Sweep、ChoCh 判讀準則 | `< 5 ms` |
| **3.2** | Planner & Reasoning Agent | LLM API (`temp=0`) | 僅讀取唯讀 JSON 與規則庫，產出嚴格 Schema 驗證之多空結構論證 | `800–1200 ms` |
| **4.1** | Decision & Risk Engine | 確定性風控數學 | 計算 ATR 停損、停利、`RR`、含成本期望值 $E$、倉位 `Pos` 與三級降級攔截 | `< 5 ms` |
| **4.2** | Output Interface | 序列化與 HITL | 輸出含 PIT 時間戳與設定雜湊之 JSON / Markdown 報告，預設等待人工確認 | `< 5 ms` |

> **端到端延遲預算**：不含網路等待，全管線決策週期控制在 **1.5 秒以內**，滿足 `5m` / `15m` K 線收盤決策需求。

### 4.2 模組間核心介面契約 (Data Contracts)

```json
// ZoneRecord (Module 2.1 -> 2.2, 3.2, 4.1)
{
  "zone_id": "DEM_BTC_15m_20261111T0800",
  "type": "DEMAND",
  "lower": 63800.0,
  "upper": 64000.0,
  "hvn_price": 63910.0,
  "status": "UNMITIGATED",
  "created_at": "2026-11-11T08:00:00Z"
}

// DecisionReport (Module 4.1 -> 4.2)
{
  "status": "APPROVED",
  "reason": "Positive expectancy and valid liquidity sweep",
  "entry": 63950.0,
  "sl": 63750.0,
  "tp": 65150.0,
  "rr": 6.0,
  "expectancy_r": 3.34,
  "position_size": 0.05,
  "pit_timestamp": "2026-11-11T10:15:00Z",
  "config_hash": "sha256:..."
}
```

---

## 5. Data & Point-in-Time (PIT) Governance

- **目標市場與週期**：Binance `BTC/USDT`、`ETH/USDT` 之 `5m` 與 `15m` K 線（兩年期回測區間 `2023–2024` 開發與基準對比，另納入 `2025-10` 極端清算事件作為壓力測試）。
- **時間切分原則**：嚴格依時間順序切分 Train / Validation / Test，禁止任何隨機 K-fold 打散。
- **PIT 洩漏防護**：參照 FinForesight（2026）之時點控制設計，決策時刻 $t$ 僅允許存取 `close_time <= t` 之 K 線與結構狀態；任何外部事件或 regime 標籤若缺乏公開時間戳或晚於 $t$ 一律排除。
- **交易成本模型**：統一納入 Taker 手續費（單邊 `0.04%`）與滑點假設，禁止使用零成本回測結果宣稱正期望值。

---

## 6. AI / Analytics & Domain Knowledge

### 6.1 機率模型與校準 (Module 2.2)
- 採用 **XGBoost** 處理供需區微觀結構特徵之非線性交互關係（Chen & Guestrin, 2016；arXiv:2407.11786）。
- **機率校準（Calibration）**：原始樹模型輸出須於驗證集進行 Platt Scaling 或 Isotonic Calibration，並以 **Brier Score** 檢驗校準品質，確保後續代入期望值公式之 $W$ 具備機率意義。

### 6.2 領域知識整合 (Module 2.1 & 3.1)
- 立基於拍賣市場理論（Auction Market Theory）與價格行為學（Brooks, 2012）。
- 將主觀型態轉譯為可稽核規則：**S/R Flip**（帶量跌破後角色反轉）、**Pinbar 被動吸收**（長影線刺穿 + 高 Volume Z-score + 實體收回）、**Liquidity Sweep**（掃蕩止損流動性後迅速收回）、**ChoCh**（次級結構反轉確認）。
- **解耦鐵律**：LLM 不允許自行推算或覆寫 `entry`、`sl`、`tp`、`RR` 或 `W`。

---

## 7. Mathematical Formulation & Algorithm

### 7.1 風控數學公式
對多頭（Long）設定：
$$RR = \frac{P_{\text{tp}} - P_{\text{entry}}}{P_{\text{entry}} - P_{\text{sl}}}$$
以單筆停損風險為 $1.0 R$ 之簡化期望值：
$$E = (W \times RR) - ((1 - W) \times 1.0)$$
部位規模（Position Size）：
$$\text{Pos} = \frac{\text{Equity} \times R_{\text{tol}}}{|P_{\text{entry}} - P_{\text{sl}}|}$$

> **參數待測聲明**：初始門檻 $W \ge 0.55$ 與 $RR \ge 2.0$ 屬於**待驗證超參數（Parameters to be tested）**，須於實驗中進行敏感度掃描並扣除手續費與滑點後再行評估。

### 7.2 核心演算法 (`Algorithm 1`)
完整具行號虛擬碼見 [`README.md` 第 7 節](./README.md#7-決策流程與演算法-decision-flow--algorithm-1) 與 [`期末專題提案_Hybrid_TPA_Agent(最新檔).pdf`](./期末專題提案_Hybrid_TPA_Agent(最新檔).pdf) 第 9 頁（第 01–24 行）。

### 7.3 三級異常降級協定 (Failure Recovery)
1. **Level 1（輕度異常：心跳延遲 `> 2s` 或 LLM 逾時 / JSON Schema 驗證失敗）**：立即凍結新訊號核准，強制輸出 `WAIT/NO_TRADE`。
2. **Level 2（在途單防護）**：檢查交易所伺服器端硬性停損單是否已掛妥，鎖定禁止加碼。
3. **Level 3（嚴重異常：行情斷流 `> 30s` 或黑天鵝極端波動熔斷）**：發出緊急告警並依設定執行對沖或市價防禦性平倉。

---

## 8. Implementation Mapping (Planned Code Layout)

依據 OpenSpec 任務清單 [`openspec/changes/refactor-hybrid-tpa-agent/tasks.md`](./openspec/changes/refactor-hybrid-tpa-agent/tasks.md)，規劃中之模組對映如下（將於規格審核通過後執行 `/opsx:apply` 實作）：

| 設計層級 | 規劃原始碼路徑 | 對應 OpenSpec Spec |
| :--- | :--- | :--- |
| Layer 1 | `src/data/config.py`, `src/data/ingestion.py` | `specs/data-ingestion/spec.md` |
| Layer 2.1 | `src/zones/sd_engine.py` | `specs/sd-zone-detection/spec.md` |
| Layer 2.2 | `src/ml/features.py`, `src/ml/winrate.py` | `specs/winrate-prediction/spec.md` |
| Layer 3 | `src/agent/knowledge_base.py`, `src/agent/planner.py` | `specs/llm-cot-reasoning/spec.md` |
| Layer 4 | `src/risk/engine.py`, `src/output/reporter.py` | `specs/risk-management/spec.md` |
| Verification | `tests/`, `experiments/` | 4 階段回測與消融實驗腳本 |

---

## 9. How to Run (Current Proposal & Spec Stage)

目前處於 **Proposal & OpenSpec Specification** 階段，可驗證規格結構完整性：

```bash
# 1. 安裝 OpenSpec CLI (需 Node.js >= 22.12.0)
npm install -g @fission-ai/openspec@latest

# 2. 檢視與驗證本專案 OpenSpec 規格
openspec list
openspec status --change refactor-hybrid-tpa-agent
openspec validate refactor-hybrid-tpa-agent --strict
```

*(程式碼實作與回測指令將於 `/opsx:apply` 完成 `src/` 與 `experiments/` 後補充於此節。)*

---

## 10. Planned Experiments & Expected Targets (Not Yet Executed)

- **四階段驗證**：(1) 時序切分回測、(2) 嚴格 PIT 時點控制、(3) 極端行情壓力測試（2025-10 清算事件）、(4) 事前雜湊鎖定之前瞻模擬。
- **五組對照**：Baseline 1 (`EMA+RSI`)、Baseline 2 (`Pure XGBoost`)、Baseline 3 (`Pure LLM`)、Ablation (`S&D + XGBoost`)、Proposed (`Hybrid TPA-Agent`)。
- **預期目標（Hypothesized Targets，非實測結果）**：預期 Profit Factor `~1.94`、Sharpe `~2.15`、MDD 控制於 `-12.0%` 內、多輪執行一致率 `> 95%`、數值錯誤率 `0%`。

---

## 11. Limitations & Research Integrity

1. **外部實證外推邊界**：SEBI（2026）統計對象為印度股票衍生品市場，不可直接等同加密貨幣市場確切比例；TradingAgents（Koviazin et al., 2026）與 FinForesight（2026）之任務週期與資產類別亦與 `5m/15m` 加密貨幣合約存在差異，須於本專案實驗中獨立驗證。
2. **流動性與 API 依賴**：低深度時段 Volume Profile HVN 可能失真，且 LLM 端受外部 API 延遲與供應商模型更新影響。
3. **可否證承諾**：若實驗顯示 LLM 情境推理帶來的風險調整後增益不足以抵銷其延遲與 API 成本，本設計將如實報告負向結果並界定 LLM 之適用邊界。
