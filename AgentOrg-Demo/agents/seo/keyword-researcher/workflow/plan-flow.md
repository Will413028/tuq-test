# Keyword Researcher — Plan Flow (D)

Plan 階段：將原料結構化、設計分析計畫。承接 research-flow 的 raw_research。

## 1. 去重與意圖分類
- 對 candidate_keywords 去重
- 為每字標注搜尋意圖（informational / navigational / commercial / transactional）
- 依據：對應的 SERP 結果類型反推使用者目的

## 2. 缺口與重疊規劃
- 比對我方目標字 vs 競品 ranking_terms
- 規劃 gap 清單（競品有、我方缺席）與 overlap 清單兩欄

## 3. 長尾叢集規劃（Clustering）
- 將候選字依主題 + 意圖歸群成 keyword cluster
- 每群指定 head term 與下屬長尾

## 4. 難度評估計畫
- 為每群/每字規劃難度判斷依據（首頁網域權威度、競品內容深度、SERP 特性）
- 標記哪些需要精確加權分數 → 委派 `shared/calculator`（透過 Manager）

## Plan 輸出（執行藍圖）
```
analysis_plan:
  intent_buckets: { informational: [...], commercial: [...], ... }
  gap_overlap_plan: { gaps: [...], overlaps: [...] }
  clusters: [{ head_term, members, intent }]
  difficulty_criteria: [...]
```
