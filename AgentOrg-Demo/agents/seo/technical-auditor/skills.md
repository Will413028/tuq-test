# Technical Auditor — Skills

### Indexability & Crawl Diagnosis
診斷搜尋引擎能否抓取與索引站點：解析 robots.txt（Disallow / Allow / crawl-delay）、meta robots 與 X-Robots-Tag（noindex/nofollow）、canonical 指向是否自我一致、sitemap.xml 涵蓋率與 lastmod 正確性、HTTP 狀態碼（200/301/302/404/410/5xx）與 redirect chain。阻擋索引列為最高嚴重度。
- Outcome 1: 可索引性矩陣（每個目標 URL 的狀態碼、index/noindex、canonical、sitemap 涵蓋）
- Outcome 2: critical 阻擋清單（被 robots 擋掉、noindex、canonical 指向錯誤頁、無限 redirect）

When to dispatch: 任何技術稽核的第一步 — 先確認索引基礎再談速度與結構化資料。

---

### Core Web Vitals & Speed Assessment
評估頁面效能與使用者體驗指標：對照現行門檻判定 LCP（≤2.5s 佳）、INP（≤200ms 佳）、CLS（≤0.1 佳），並定位主因（render-blocking 資源、未壓縮圖片、缺 width/height 造成位移、過大 JS bundle、缺乏快取標頭）。以實際量測值為準，不憑印象斷言。
- Outcome 1: CWV 判定表（每指標：實測值 vs 門檻 → 佳/需改善/差）
- Outcome 2: 效能瓶頸根因清單 + 修復方向（交由 sw team 實作）

When to dispatch: 可索引性通過後，評估速度/體驗對排名與留存的影響。

---

### Structured Data (Schema) Validation
檢查結構化資料正確性：抓取頁面 HTML，定位 JSON-LD（`application/ld+json`）或 Microdata（`itemtype`），對照 Schema.org 型別的必填/建議欄位，驗證型別是否與頁面內容相符（Article / Product / FAQ / BreadcrumbList / Organization 等），標記語法錯誤、缺必填屬性、型別濫用（rich result 違規）。
- Outcome 1: Schema 驗證報告（每頁：偵測到的型別、必填欄位完整度、錯誤/警告）
- Outcome 2: rich result 風險清單（可能被 Google 視為 spam 或無法顯示的標記）

When to dispatch: 需診斷站點能否取得 rich result / 結構化資料是否健全時。

---

### Mobile-Friendliness & HTTPS Security Audit
稽核行動友善度與傳輸安全：檢查 viewport meta 設定、可點擊元素間距、字級可讀性、是否有水平捲動；HTTPS 面向檢查憑證有效性、混合內容（https 頁載入 http 資源）、HSTS 標頭、http→https 是否正確 301 導向。
- Outcome 1: 行動友善檢查表（viewport / 觸控目標 / 字級 / RWD 斷點）
- Outcome 2: HTTPS 安全報告（憑證、混合內容、HSTS、導向正確性）

When to dispatch: 需確認站點符合行動優先索引（mobile-first indexing）與安全基準時。

---

### Duplicate Content, Canonical & Crawl Budget Analysis
分析重複內容與爬取效率：偵測重複/近似頁面、參數化 URL 爆量、軟 404、孤兒頁、過深層級、canonical 與 hreflang 衝突；評估爬取預算浪費點。診斷時納入站點規模脈絡，避免對小站套用大站建議。
- Outcome 1: 重複內容/canonical 衝突清單 + 建議的標準化策略
- Outcome 2: 爬取預算浪費點清單（參數組合、軟 404、孤兒頁）依站點規模加權

When to dispatch: 中大型站點或多語站；或索引覆蓋數遠大於有效內容頁數時。

---

### Prioritised Audit Report Synthesis
將所有發現整合為帶嚴重度與優先序的稽核報告：每個問題標記 severity（critical / major / minor）並按「對索引與排名的影響 × 修復成本」排序，讓 sw team 能直接照單施工。
- Outcome 1: 標準格式稽核報告（issue、location/URL、evidence、severity、影響、修復建議、優先序）
- Outcome 2: 給 Manager 的執行摘要 + 對齊半年自然搜尋 KPI 的重點 top-N 修復項

When to dispatch: 所有診斷步驟完成後，整合輸出最終報告與優先序。

---

## NOT This Agent's Job
- 實際修改網站程式碼、robots.txt、sitemap 或伺服器設定（只診斷，實作交給 **sw team**）
- 關鍵字研究、搜尋意圖、難度評估（**keyword-researcher**）
- 內容策略、主題叢集、內容大綱（**content-strategist**）
- 流量/排名/轉換數據分析與報告（**analytics-reporter**）
- 使用 Agent 派發子任務（Worker 無此權限）
- 在無實際量測或抓取證據下宣稱問題存在
