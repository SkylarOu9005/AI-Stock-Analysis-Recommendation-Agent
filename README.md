- - -

# 兩階段思考-金融分析AI代理人系統 (Two-Stage CoT Stock Analysis Agent)

本專案建構了一個基於兩階段鏈狀思考（Two-Stage Chain of Thought, CoT）架構的AI金融分析代理人（AI Agent）。系統模擬專業證券分析師的思維流程，透過自動化抓取即時市場數據、進行多項指標的技術分析，並由大型語言模型（LLM）依據邏輯鏈進行深度推理，最終產出具備高參考價值的投資決策報告與量化推薦建議。

## 系統核心特色 

* **改正原先LLM對於金融分析思考路線的不專業**：傳統 LLM 在面對股票分析時，常因缺乏即時數據或直接跳到結論而產生「幻覺」或給出模糊建議。本專案透過 **AI Agent 工具調用機制**，強制模型必須先查驗數據、分析數據，最後才進行推論。
* **兩階段 Chain of Thought (CoT)**：
* **第一階段：客觀數據蒐集與指標計算**（擷取最新股價、移動平均線 MA、RSI、MACD 等量化指標）。
* **第二階段：多維度邏輯推理與決策生成**（綜合技術面、籌碼面、宏觀邏輯進行深度思考，最終給出明確的投資評等）。


* **Outputs的結構化 (Structured Outputs)**：系統最終將複雜的推理過程與量化分數整理並轉化為清晰易讀的表格或 JSON 格式，方便前端或自動化交易系統串接。

- - -

## 系統架構與運作流程

本系統的核心在於將「數據抓取工具」與「決策大腦」分離，建立有序的 Agent Workflow：

```
[使用者輸入: 股票代碼] 
       │
       ▼
┌────────────────────────────────────────────────────────┐
│ 階段一： 資料擷取agent (Data Retrieval Agent)           │
│ 1. 調用 API 獲取即時與歷史股價數據                       │
│ 2. 計算關鍵指標 (MA 20/60, RSI-14, MACD 趨勢)           │
└────────────────────────────────────────────────────────┘
       │
       ▼ [結構化市場數據指標]
┌────────────────────────────────────────────────────────┐
│ 階段二： 決策分析agent (Reasoning & Decision Agent)     │
│ 1. 執行 Chain-of-Thought 推理 (分析多空訊號是否共振)     │
│ 2. 評估潛在風險與下檔支撐點位                            │
│ 3. 生成最終投資評等 (Buy / Hold / Sell) 與目標區間       │
└────────────────────────────────────────────────────────┘
       │
       ▼
[最終結構化金融分析報告]

```

### 1. 工具整合 (Tools Set)

Agent 配備了專門的 Python 數據分析工具鏈，包含但不局限於：

* 即時金融 API 串接套件
* 量化技術指標計算模組（計算超買/超賣、黃金交叉/死亡交叉等狀態）

### 2. 提示詞工程與 Reasoning 限制

透過優化後的 System Prompt，限制模型在未完整跑完技術指標分析前不得給出結論。在 CoT 歷程中，模型會自主評估：「目前 RSI 顯示超賣，但 MACD 尚未轉正，代表短期可能止跌但未反轉」，這與人類分析師的看盤邏輯一致。

---

## 成果範例

執行分析後，Agent 會產出包含推理鏈（Thought）**與**行動結論（Action）的雙層報告：

> **Agent思考歷程 (Thought Trace)**:
> *"The 20-day MA is higher than the 60-day MA, indicating an intermediate uptrend. However, the current RSI is at 74, signaling an overbought condition. Volume expansion confirms buying institutional interest, but a short-term consolidation is highly probable before testing the key resistance..."*
> **最終決策結論輸出 (Final Output)**:
> * **分析標的**：AAPL / 2330
> * **投資評等**：分批買進 (Accumulate on Dips)
> * **關鍵點位**：支撐位 XXX / 壓力位 YYY
> * **風險提示**：注意即將到來的財報週與總體經濟利率決策。
> 
> 

---

## 環境需求

### 1. 環境準備

請確保您的環境中已安裝 Python 3.10+，並配置相關 LLM 的 API Key 環境變數：

```bash
# 安裝量化金融與 AI Agent 相關核心套件
pip install langchain openai pandas numpy yfinance matplotlib

```

### 2. 快速執行

可以直接在 Google Colab 或本地 Jupyter 環境中執行 `Two_Stage_CoT_Stock_Analysis_Agent.ipynb`：

```python
# 初始化分析 agent
from stock_agent import TwoStageStockAgent

agent = TwoStageStockAgent(api_key="your_llm_api_key")

# 啟動 two-stage chain of thought 分析
report = agent.analyze_stock(ticker="2330.TW")
print(report)

```

---

## 開發者與專案資訊

* **開發者**：歐靜嬡 (Skylar Ou)
* **專案類型**：AI Agent / 量化金融研究 / 提示詞工程應用
* **GitHub repository**：[AI-Stock-Analysis-Recommendation](https://www.google.com/search?q=https://github.com/SkylarOu9005/AI-Stock-Analysis-Recommendation)

---
