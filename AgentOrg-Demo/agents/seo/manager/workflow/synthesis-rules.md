# SEO Manager — Synthesis Rules

After all worker agents return, SEO Manager follows these rules:

## 1. Check Status

- `completed` → include in synthesis
- `failed` → retry once with refined prompt. If still fails, report gap to user with partial results.

## 2. Verify Outputs

For each worker output:
- 產出路徑存在（Glob / Bash ls），檔案 size > 0
- keyword-researcher：keyword_map 含意圖分群、搜尋量/競爭度、長尾機會
- technical-auditor：audit_report 為診斷與建議（**不得**含實際 code 修改）
- content-strategist：content_plan 含 topic cluster、大綱、內部連結
- analytics-reporter：報表含基線數值與對 KPI 面向的對照

驗證失敗 → retry 對應 worker 一次。需要精確數值時派 `shared/calculator` 覆核。

## 3. KPI Alignment Synthesis

每項產出必須對齊北極星 KPI「半年內提升自然搜尋成效」。報告中明示每項產出對應的面向與預期影響：

| 產出 | KPI 面向 | 預期影響 |
|------|---------|---------|
| keyword_map | 排名 / 自然流量 | 鎖定高機會關鍵字，導引內容方向 |
| audit_report | 索引/技術健康 | 修復阻礙索引與爬取的技術問題 |
| content_plan | 自然流量 / 轉換 | topic cluster 擴大覆蓋，提升轉換 |
| baseline_report | 全面（量測基準） | 建立可追蹤的成效基線 |

## 4. Synthesize Report

Report to user includes:
- 各產出檔案 / 結果路徑（每項一行）
- KPI 對齊說明（依 §3 表格）
- 行動建議優先序（高/中/低 + 理由）

## 5. 工時打卡明細格式

依 soul.md 原則 #13：

```
| Agent | input_summary | output_summary | started_at | ended_at | duration_s | status |
|-------|---------------|----------------|------------|----------|------------|--------|
| keyword-researcher | ... | ... | ... | ... | ... | completed |
| technical-auditor | ... | ... | ... | ... | ... | completed |
| content-strategist | ... | ... | ... | ... | ... | completed |
| analytics-reporter | ... | ... | ... | ... | ... | completed |
| **總計** | 4 agents | | | | {total}s | {pass}/{fail} |
```

- 時間取自 worklog JSON 的實際時間戳，不得粗估
- 所有被派遣的 agent 都必須出現；缺 worklog 或 failed 者標註原因

## 6. Failure Recovery

| Situation | Action |
|-----------|--------|
| keyword-researcher 失敗 | Retry once；若再失敗，僅用 shared/researcher 結果繼續，並標註覆蓋不足 |
| technical-auditor 失敗 | Retry once；若再失敗，交付已完成部分並提示稽核缺口 |
| content-strategist 失敗 | Retry once with more structured prompt（附 keyword_map 重點） |
| analytics-reporter 失敗 | Retry once；若再失敗，回報缺基線數據，建議補數據源 |
| 全部失敗 | 誠實回報，列出每棒失敗原因與建議下一步 |
