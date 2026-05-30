# Keyword Researcher — Execute Flow (V)

Execute 階段：依 analysis_plan 產出最終結構化關鍵字地圖並自我驗證。

## 1. 產出意圖分類表
依 plan 的 intent_buckets，輸出帶意圖標籤的關鍵字表，每字附判讀依據（SERP 主要結果類型）。

## 2. 產出缺口/重疊清單
輸出 gap 清單（競品排名、我方缺席的機會字，含可切入角度）與 overlap 清單，兩欄分明。

## 3. 產出長尾叢集地圖
每群含 head_term、成員長尾、共同意圖，並附 cluster 優先級草案。

## 4. 產出難度評估
每字/每群標難度等級（低/中/高），**每個分級必附判斷依據**。精確加權分數委派 `shared/calculator`（透過 Manager），不自行心算。

## 5. 自我驗證（逐項檢查）
- 每個關鍵字是否都有意圖標籤？
- gap/overlap 是否兩欄分明？
- 長尾是否全部歸入 cluster（無散裝）？
- 每個難度分級是否都附依據與來源？
- 每個數據點是否都可溯源？
未通過項目回到對應階段補齊。

## 最終輸出結構
```
keyword_map:
  topic: [...]
  intent_classified: [{ term, intent, evidence, source }]
  gap_analysis: { gaps: [...], overlaps: [...] }
  clusters: [{ head_term, long_tails, intent, priority }]
  difficulty: [{ term, level, basis, source }]
  quick_wins: [低難度高機會字]
  summary: [一段整合性結語，含已知資料缺口]
```
產物寫到 Manager 指定的 `output_path`，回傳摘要給 seo/manager。
