# Analytics Reporter — Gather Flow (Research)

R-D-V 第一段：蒐集分析所需的全部輸入。由 workflow.yaml `gather` 步驟 route 到此。

## 流程步驟

### 1. 確認任務參數
- 從 Manager 派遣訊息取得：`output_path`、報告期間（start/end）、KPI 目標定義、分析範圍（哪些關鍵字/頁面/指標）。
- 若 **缺 `output_path`**：停止並回報 `SCOPE VIOLATION: missing output_path`。
- 若 **缺報告期間或 KPI 定義**：停止並回報 `MISSING_REPORT_PARAMS`，請 Manager 補齊（不自行假設期間或目標）。

### 2. 讀取 KPI 目標與半年成長目標
- 工具：`Read` / `mcp__desktop-commander__read_multiple_files`（T:\ 路徑）
- 取得半年成長目標（如自然流量 +40%、目標關鍵字前三名數量）與比較基準期。

### 3. 蒐集原始資料來源
| 來源 | 內容 | 注意 |
|------|------|------|
| Google Search Console | 曝光、點擊、CTR、平均排名 | 標明期間與抓取日 |
| GA4 | 自然流量 session、engagement、轉換 | 與 GSC 指標不可直接相比 |
| 排名工具匯出 | 目標關鍵字 SERP 位置 | 標明工具與抓取日 |
- 若任一來源缺失：記錄缺口，於報告標明「資料不完整」，不得以猜測補值。

### 4. 蒐集時間線事件（供歸因）
- 內容上線日、技術修正日、已知演算法更新、季節性事件、A/B 實驗起迄。
- 來源：keyword-researcher / content-strategist 提供的上線時間點，或 Manager 傳入。

## 輸出
- 結構化「分析輸入包」：資料來源 + 期間 + 比較基準 + KPI 目標 + 時間線事件 + 資料缺口清單。
- 交給 analyze-flow。

## 注意事項
- 每筆資料必須標來源與期間，後續分析才可溯源。
- 不混用不同來源/期間的數據當同一指標。
