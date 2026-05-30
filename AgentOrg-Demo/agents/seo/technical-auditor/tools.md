# Technical Auditor — Tools

## Primary Tools
| Tool | Purpose |
|------|---------|
| `Read` | 讀取站點規劃、URL 清單、既有稽核報告與 Manager 提供的基準檔案 |
| `Bash` | 執行技術量測：`curl -I` 取 HTTP header/狀態碼、抓取 robots.txt / sitemap.xml、檢查 redirect chain、解析回應 |
| `Grep` | 在 robots.txt、sitemap、HTML 原始碼中搜尋 Disallow、noindex、canonical、Schema 標記、hreflang 等指令 |
| `WebFetch` | 抓取目標頁面 HTML 與標頭，檢查 meta robots、canonical、結構化資料、行動視窗設定 |
| `WebSearch` | 查詢最新技術 SEO 規範（Core Web Vitals 門檻、Schema.org 型別、Google 索引政策） |

## MCP Tools (Authorized)

| MCP Tool | 授權操作 | 用途 |
|----------|---------|------|
| `mcp__desktop-commander__read_multiple_files` | read_multiple_files | 讀取 T:\ 路徑的站點規劃、URL 清單、基準稽核檔案（Google Drive 限制須用此工具）|
| `mcp__desktop-commander__list_directory` | list_directory | 掃描 T:\ 目錄結構，確認輸入與輸出路徑 |
| `mcp__workspace__bash` | shell | 呼叫 `scripts/worklog.sh` 打卡、執行 curl/抓取等技術量測指令 |
| `mcp__workspace__web_fetch` | web_fetch | 抓取目標 URL 的 HTML/標頭進行技術診斷 |
| `WebSearch` | search | 查詢最新技術 SEO 規範與門檻值 |

> 注意：T:\ 路徑的檔案**必須**用 `read_multiple_files`，禁止使用 `read_file`（Google Drive 限制，見 protocols/rules/google-drive-read.md）。

## Do NOT Use
- `Agent` — 不派發子 agent（Worker 無此權限）
- `Write` — 不修改網站任何檔案（robots.txt / sitemap / 程式碼）；memory 例外，見下方
- `Edit` — 不修改任何網站或系統檔案（你只診斷，不實作修復）

## Tool Usage Guidelines
- **可索引性檢查範例**：
  - 取 HTTP 狀態與標頭：`curl -sI {url}`（檢查 200/301/302/404、X-Robots-Tag）
  - 抓 robots.txt：`curl -s {origin}/robots.txt` 後 Grep `Disallow`
  - 抓 sitemap：`curl -s {origin}/sitemap.xml` 後檢查 URL 涵蓋與 lastmod
  - 追蹤 redirect chain：`curl -sIL {url}`
- **結構化資料**：以 WebFetch 取頁面 HTML，Grep `application/ld+json` / `itemtype`，再以 WebSearch 對照 Schema.org 型別要求
- **HTTPS/安全**：檢查憑證有效性、混合內容（http 資源出現在 https 頁）、HSTS 標頭
- **Core Web Vitals**：以 WebSearch 取現行門檻（LCP ≤2.5s、INP ≤200ms、CLS ≤0.1），對照量測值判定通過/需改善/差
- **Memory exception**: 可用 `Write` 將稽核學習寫入 `agents/seo/technical-auditor/memory/`，依 memory-protocol.md 規範
