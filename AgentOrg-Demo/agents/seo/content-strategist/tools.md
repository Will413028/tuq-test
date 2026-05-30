# Content Strategist — Tools

## Primary Tools
| Tool | Purpose |
|------|---------|
| `Read` | 讀取 keyword-researcher 的關鍵字/分群輸出、現有網站內容清單、競品內容、既有內容策略草稿 |
| `Write` | 將內容策略藍圖、內容日曆、on-page 規格寫入 Manager 派遣訊息中的 `output_path` 指定路徑（典型為 `<CWD>/output/seo/{task_id}/`，詳見 `agents/protocols/rules/output-placement.md`）；**不得**寫入自己的 agent 資料夾（memory/ 除外）|
| `Edit` | 在同一次任務內迭代修訂自己剛產出的策略草稿（不修改其他 agent 的產物）|

## MCP Tools (Authorized)

| MCP Tool | 授權操作 | 用途 |
|----------|---------|------|
| `mcp__desktop-commander__read_multiple_files` | read_multiple_files | 讀取 T:\ 路徑的 keyword-researcher 輸出、競品內容、既有策略檔（Google Drive 限制須用此工具，見 `agents/protocols/rules/google-drive-read.md`）|
| `mcp__desktop-commander__write_file` | write_file | 將內容策略藍圖/內容日曆/on-page 規格寫入 T:\ 指定路徑 |
| `mcp__desktop-commander__list_directory` | list_directory | 掃描 T:\ 目錄結構，確認輸入/輸出路徑 |
| `mcp__workspace__bash` | worklog | 呼叫 `scripts/worklog.sh` 打卡記錄工時 |
| `WebSearch` | SERP 與意圖驗證 | 檢視目標關鍵字的實際 SERP 結果，驗證搜尋意圖、內容類型與 SERP feature（用於策略規劃，非關鍵字探勘）|
| `mcp__workspace__web_fetch` | 競品內容擷取 | 擷取競品 pillar/cluster 頁面結構，分析其主題涵蓋與內鏈策略 |

> 注意：T:\ 路徑的檔案**必須**用 `read_multiple_files`，禁止使用 `read_file`（Google Drive 限制）。

## Do NOT Use
- `Agent` — 不直接呼叫其他 agent（跨 agent 協調由 seo/manager 負責）
- `Bash` — 不執行 shell 指令（worklog 打卡透過 `mcp__workspace__bash`；系統查詢非本職）
- `mcp__desktop-commander__read_file` — T:\ 路徑只回傳 metadata，一律改用 `read_multiple_files`
- 寫入自己的 agent 資料夾作為任務產物存放處（違反 `agents/protocols/rules/output-placement.md`）— 策略藍圖一律寫 Manager 傳來的 `output_path`（memory/ 除外，那是 agent 內部快取）
- 任何撰寫實際部落格全文、執行技術稽核或產出數據報告的工具操作（非本職）

## Tool Usage Guidelines
- **Read 來源順序**：先讀 keyword-researcher 的關鍵字/分群輸出 → 再讀既有網站內容/競品 → 才開始規劃策略
- **WebSearch 用途界線**：僅用於驗證搜尋意圖與觀察 SERP，**不得**用於原創關鍵字探勘或搜尋量估算（那是 keyword-researcher 的職責）
- **Write 目標路徑**：策略藍圖一律寫入 Manager 派遣訊息中的 `output_path`。**若 Manager 未傳 `output_path`，停止執行並回報 `SCOPE VIOLATION: missing output_path`**。檔名依任務慣例（如 `content-strategy.md`、`content-calendar.md`、`onpage-spec.md`）。
- **每次 Write 前先確認路徑**，避免覆蓋既有策略檔
