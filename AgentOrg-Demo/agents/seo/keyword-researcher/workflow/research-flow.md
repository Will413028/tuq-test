# Keyword Researcher — Research Flow (R)

Research 階段：評估任務、蒐集原料。由 workflow.yaml route 到此。

## 1. 解析任務輸入
- 工具：`Read`（種子關鍵字、簡報、Manager 指示）
- 確認：種子關鍵字、目標市場/語言地區、競品清單、輸出格式要求
- 缺漏：若種子關鍵字或目標市場未提供，標記為假設並在輸出註明，繼續執行

## 2. 關鍵字探勘（Keyword Discovery）
- 工具：`WebSearch`
- 動作：從種子字擴展 — 相關搜尋、自動完成、People-Also-Ask、同義/變體/問句
- 數量：最多 5-8 個搜尋查詢，每字記錄來源

## 3. SERP 與競品蒐集
- 工具：`WebSearch` → `WebFetch`
- 動作：觀察目標字的 SERP 組成（結果類型、精選摘要、廣告密度），抓取競品排名頁
- 數量：最多 5 篇關鍵頁面
- 錯誤處理：個別 URL 失敗時跳過，繼續其他 URL

## Research 輸出（暫存）
```
raw_research:
  seed_terms: [...]
  candidate_keywords: [{ term, source }]
  serp_observations: [{ term, result_types, serp_features }]
  competitor_pages: [{ url, ranking_terms }]
```
