# Plan Flow — seo/technical-auditor

Plan 段：將 audit 段蒐集的 raw evidence 轉為帶嚴重度與優先序的問題清單。此階段做判定與排序，不抓取新資料、不撰寫最終報告版面（那是 execute 段）。

## 1. 證據歸類為問題（Issue）

對 audit 段的每筆證據，判斷是否構成問題並歸入面向：
- Indexability（robots / noindex / canonical / sitemap / 狀態碼）
- Core Web Vitals / 速度
- Structured Data（Schema）
- Mobile-friendliness
- HTTPS / 安全
- Duplicate content / canonical / 爬取預算

每個 issue 必須附 audit 段的可驗證證據（不得無證據建構問題）。

## 2. 標記嚴重度（Severity）

| 嚴重度 | 定義 |
|--------|------|
| critical | 阻擋索引（robots 擋、noindex、canonical 指錯頁、無限 redirect、5xx、憑證失效）→ 直接傷害搜尋可見度 |
| major | 顯著影響排名/體驗（CWV「差」、大量重複內容、混合內容、Schema 必填缺失、非行動友善） |
| minor | 體驗或最佳化建議（CWV「需改善」、Schema 建議欄位缺、輕微版面） |

## 3. 排序（影響 × 修復成本）

- 對每個 issue 評估「對索引與排名的影響」與「修復成本」
- 排序原則：高影響 + 低成本優先；critical 永遠在前
- 納入站點規模脈絡：小站不套用大站爬取預算建議
- 需精確加權分數 / 通過率百分比 → 不自行心算，請 Manager 派 `shared/calculator`

## 4. 對齊 KPI

- 對照 SEO 團隊半年 North Star KPI（提升自然搜尋成效）
- 標出「最能推進 KPI」的 top-N 修復項，供 execute 段在執行摘要中突顯

## 5. 標注實作歸屬

- 每個 issue 標注修復實作歸屬（多數為 **sw team**，經 Manager 轉派）
- 本 agent 只規劃，不自行改碼

## 輸出（交給 execute 段）

已分級、已排序的 issue 清單，含：面向、URL/位置、證據、severity、影響評估、修復建議、優先序、實作歸屬。

## 錯誤處理

| 情況 | 處理方式 |
|------|---------|
| 證據不足以判定 | 標「需進一步量測」，不臆測嚴重度 |
| 需精確計算排序分數 | 委派 shared/calculator，不心算 |
| 範圍超出 SEO 技術診斷 | 觸發 Scope Guard，回報 Manager |
