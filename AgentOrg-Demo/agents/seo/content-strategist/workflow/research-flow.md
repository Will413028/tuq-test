# Content Strategist — Research Flow

R-D-V 第一段：Research。評估輸入、釐清搜尋意圖、盤點既有內容。由 workflow.yaml route 到此。

## 流程步驟

### 1. 確認 output_path 與任務範圍
- 從 Manager 派遣訊息讀取 `output_path`、目標主題/網站、目標關鍵字組。
- 若缺 `output_path`：停止並回報 `SCOPE VIOLATION: missing output_path`。

### 2. 讀取 keyword-researcher 輸出
- 工具：`Read` / `mcp__desktop-commander__read_multiple_files`（T:\ 路徑必用後者）。
- 來源：keyword-researcher 的關鍵字、搜尋量、難度、分群資料。
- 若資料缺失：以最小可用資訊繼續，並在報告標記 `keyword_data_missing: true`，回報 Manager 建議先派 keyword-researcher。

### 3. 盤點既有內容與競品
- 讀取既有網站內容清單、現有 pillar/cluster 結構（若有）。
- 用 `mcp__workspace__web_fetch` 擷取競品 pillar/cluster 頁面結構，分析主題涵蓋與內鏈策略。

### 4. 驗證搜尋意圖（SERP 觀察）
- 用 `WebSearch` 檢視目標關鍵字的實際 SERP（內容類型、SERP feature）。
- 為每個關鍵字判定意圖：informational / navigational / commercial / transactional。
- **界線**：WebSearch 僅用於意圖驗證與 SERP 觀察，**不得**用於原創關鍵字探勘或搜尋量估算（那是 keyword-researcher 的職責）。

## 輸出（傳遞給 Plan 段）
```
research_summary:
  topic: [主題]
  keyword_groups: [來自 keyword-researcher 的分群]
  intent_map: [關鍵字 → 意圖 → 內容類型]
  existing_content_gaps: [缺口清單]
  competitor_insights: [競品叢集/內鏈觀察]
  flags: [keyword_data_missing 等]
```

## 注意事項
- 任何精確數值計算（難度加權、字數估算）請求 Manager 派 `shared/calculator`，不自行心算。
