# Audit Flow (Research) — seo/technical-auditor

Research 段：抓取與量測目標站點，蒐集可驗證證據。此階段只蒐集事實，不下結論、不排優先序（那是 plan 段）。

## 1. 確認稽核範圍

- 從 Manager brief 取得：目標站點 origin、URL 清單、稽核範圍（全量 / 指定面向）、站點規模脈絡（頁數量級）
- 若缺目標 URL：停止並回報「無稽核目標，無法蒐集證據」，等待重新分派
- 讀取 Manager 提供的站點規劃 / 既有報告作為對照基準（T:\ 路徑用 `read_multiple_files`）

## 2. 可索引性證據蒐集（Indexability）

```bash
curl -s {origin}/robots.txt        # 抓 robots.txt → Grep Disallow / Allow / Sitemap
curl -s {origin}/sitemap.xml       # 抓 sitemap → 檢查 URL 涵蓋與 lastmod
curl -sI {url}                     # 每個目標 URL 的 HTTP 狀態碼與標頭（X-Robots-Tag）
curl -sIL {url}                    # 追蹤 redirect chain（偵測無限/多跳導向）
```
- 以 WebFetch 取頁面 HTML，Grep `noindex`、`canonical`、`rel="canonical"`
- 記錄：每 URL 狀態碼、index/noindex、canonical 目標、是否在 sitemap、是否被 robots 擋

## 3. Core Web Vitals 與速度證據

- WebSearch 取現行 CWV 門檻（LCP ≤2.5s、INP ≤200ms、CLS ≤0.1）
- 蒐集量測值（若有 PSI/lab 數據來源則引用；無則記錄「未量測」並標降級警告）
- 記錄潛在瓶頸線索：render-blocking 資源、大圖、缺 width/height、缺快取標頭

## 4. 結構化資料證據（Schema）

- WebFetch 取 HTML，Grep `application/ld+json` 與 `itemtype`
- 記錄每頁偵測到的 Schema 型別與原始標記片段
- WebSearch 對照 Schema.org 該型別的必填/建議欄位

## 5. 行動友善與 HTTPS 證據

- Grep `viewport` meta；記錄 RWD 線索
- `curl -sI` 檢查 HSTS 標頭、http→https 導向；WebFetch 偵測混合內容（https 頁中的 http 資源）

## 6. 重複內容 / canonical / 爬取預算證據

- 比對 URL 清單找重複/近似頁、參數化 URL、軟 404（200 狀態但內容為錯誤頁）
- 記錄孤兒頁、層級深度、canonical 與 hreflang 衝突
- 依站點規模脈絡標注爬取預算相關線索

## 輸出（交給 plan 段）

結構化證據集（raw evidence），每筆含：面向、URL/位置、實測值或抓取片段、來源指令。**此階段不標嚴重度、不排序。**

## 錯誤處理

| 情況 | 處理方式 |
|------|---------|
| 無目標 URL | 停止並回報，等待重新分派 |
| 站點完全無法連線 | 立即回報 critical，附 curl 錯誤輸出 |
| 部分 URL 抓取失敗 | 記錄失敗 URL，繼續其餘，標「未驗證」 |
| CWV 無量測來源 | 記錄「未量測」，於報告降級為警告 |
| 需精確百分比計算 | 不自行心算，請 Manager 派 shared/calculator |
