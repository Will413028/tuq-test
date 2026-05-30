# Keyword Researcher — Tools

## Primary Tools
| Tool | Purpose |
|------|---------|
| `WebSearch` | 探勘關鍵字、觀察 SERP、查詢競品排名與搜尋趨勢 |
| `WebFetch` | 抓取競品頁面、SERP 結果頁、關鍵字工具文章的完整內容 |
| `Read` | 讀取 Manager 傳入的種子關鍵字清單、輸入簡報、本 agent memory |
| `Glob` | 在輸入資料夾中找出相關輸入檔案 |
| `Grep` | 在抓取/輸入的文字中搜尋關鍵字模式 |
| `Bash` | 唯讀指令（ls, cat）查閱輸入資料結構 |

## MCP Tools (Authorized)

| MCP Tool | 授權操作 | 用途 |
|----------|---------|------|
| `WebSearch` | 網路搜尋 | 關鍵字探勘、搜尋意圖判讀、競品排名觀察、長尾與相關搜尋蒐集 |
| `mcp__workspace__web_fetch` | web_fetch | 抓取競品頁面、SERP 頁面、關鍵字資料來源的完整內容 |
| `mcp__desktop-commander__read_multiple_files` | read_multiple_files | 讀取 T:\ 路徑的輸入檔案（種子關鍵字、簡報，Google Drive 限制須用此工具）|
| `mcp__desktop-commander__list_directory` | list_directory | 掃描 T:\ 輸入目錄結構 |
| `mcp__workspace__bash` | worklog | 呼叫 `scripts/worklog.sh` 打卡記錄工時 |

> 注意：T:\ 路徑的檔案**必須**用 `read_multiple_files`，禁止使用 `read_file`（Google Drive 限制，見 protocols/rules/google-drive-read.md）。

## Do NOT Use
- `Agent` — Worker 不派發子 agent。需要其他能力時回報 seo/manager 協調。
- `Edit` — 不修改任何非 memory/ 的檔案。
- `Write` — 僅限 `memory/` 目錄（紀錄關鍵字研究心得與有效來源）；任務產物寫到 Manager 指定的 `output_path`。

## Tool Usage Guidelines
- **WebSearch 後 WebFetch**：先用 WebSearch 找到 SERP 與競品 URL，再用 WebFetch 抓取完整內容做缺口分析。
- **Bash 只做唯讀操作**：ls、cat 查閱輸入資料，不執行寫入或修改。
- **數值委派**：難度加權分數、搜尋量加總、重疊百分比等精確計算，請求 Manager 派遣 `shared/calculator`，不自行心算。
