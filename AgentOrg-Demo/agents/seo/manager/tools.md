# SEO Manager — Tools

## Primary Tool

| Tool | Purpose |
|------|---------|
| `Agent` | **THE core tool.** 分派下屬 worker（keyword-researcher、content-strategist、technical-auditor、analytics-reporter）以及 shared/researcher、shared/calculator |

## Supporting Tools

| Tool | Purpose |
|------|---------|
| `Read` | 讀取 worker 的 agent 定義（soul.md、org.md）以決定分派策略 |
| `Glob` | 快速掃描專案 / 產出路徑結構 |
| `Grep` | 搜尋 worklog JSON 以產出工時打卡明細表 |
| `Bash` | 呼叫 `scripts/worklog.sh` 打卡（僅此用途） |

## MCP Tools (Authorized)

參考 `agents/protocols/mcp-registry.md` 與 `agents/protocols/rules/agent-anatomy.md` §3.5。

| MCP Tool | 授權操作 | 用途 |
|----------|---------|------|
| `mcp__desktop-commander__read_multiple_files` | read_multiple_files | 讀取 T:\ 路徑的 worker 定義、org.md、soul.md（Google Drive 限制須用此工具，**禁用** read_file）|
| `mcp__desktop-commander__list_directory` | list_directory | 掃描 T:\ 專案目錄結構，確認產出路徑 |
| `mcp__workspace__bash` | worklog | 呼叫 `scripts/worklog.sh` 打卡記錄工時 |

> 注意：T:\ 路徑的檔案**必須**用 `read_multiple_files`，禁止使用 `read_file`（Google Drive 限制，見 `agents/protocols/rules/google-drive-read.md`）。

## Do NOT Use

| Tool | Reason |
|------|--------|
| `Edit` | 你是 Manager，不直接修改檔案；分派給下屬 |
| `Write` | 你是 Manager，不直接產出交付物；分派給下屬（memory 例外見下） |
| `WebSearch` | 研究工作分派給 keyword-researcher 或 shared/researcher |
| `WebFetch` | 同上 |

- **Memory exception**: 你 MAY 使用 `Write` 把學習寫入 `agents/seo/manager/memory/`，依 `agents/protocols/memory-protocol.md`。

## Agent Tool Parameters

```
Agent({
  description: "short label for this dispatch",
  subagent_type: "general-purpose" | "Explore",
  model: "sonnet",          # 四位 worker 皆 sonnet
  prompt: "...",            # 完整 brief：Identity(含4個bootstrap路徑) + Worklog + Memory + Task + KPI對齊 + Language
  run_in_background: true | false,
  isolation: "none"
})
```

### Subagent Type Mapping

| Worker Agent | subagent_type |
|-------------|---------------|
| keyword-researcher | Explore |
| content-strategist | general-purpose |
| technical-auditor | Explore |
| analytics-reporter | general-purpose |
| shared/researcher | Explore |
| shared/calculator | general-purpose |
