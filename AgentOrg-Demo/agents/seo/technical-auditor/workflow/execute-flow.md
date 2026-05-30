# Execute Flow — seo/technical-auditor

Execute 段：將 plan 段的分級排序 issue 清單，產出標準格式技術稽核報告，交回 Manager。本 agent 只產出報告（診斷與建議），不實作修復。

## 1. 產出稽核報告

```
## 技術 SEO 稽核報告

### 稽核對象
- 站點 / URL 範圍：{origin / url 清單}
- 站點規模脈絡：{頁數量級}
- 稽核面向：{indexability / CWV / schema / mobile / https / duplicate}
- 量測來源與日期：{來源}

### 執行摘要（對齊半年自然搜尋 KPI）
- Top-N 優先修復項（最能推進 KPI）：{1..N}
- 整體技術健康度判定：健康 / 需改善 / 有阻擋索引風險

### 面向別判定
| 面向 | 判定 | 重點發現 |
|------|------|---------|
| 可索引性 | PASS / 需改善 / FAIL | {說明} |
| Core Web Vitals | 佳 / 需改善 / 差 / 未量測 | LCP/INP/CLS {實測 vs 門檻} |
| 結構化資料 | PASS / 需改善 / FAIL | {型別與缺漏} |
| 行動友善 | PASS / 需改善 / FAIL | {viewport / 觸控 / RWD} |
| HTTPS / 安全 | PASS / 需改善 / FAIL | {憑證 / 混合內容 / HSTS} |
| 重複內容 / 爬取預算 | PASS / 需改善 / FAIL | {canonical / 參數 / 孤兒頁} |

### 問題清單（依優先序）
| # | 面向 | URL/位置 | 證據 | 嚴重度 | 影響 | 修復建議 | 實作歸屬 |
|---|------|---------|------|--------|------|---------|---------|
| 1 | {面向} | {url} | {curl/抓取片段/量測值} | critical/major/minor | {對索引排名影響} | {可施工建議} | sw team |

### 建議（給 Manager）
- 需轉派 sw team 實作的修復項與順序
- 需 keyword/content/analytics 協作的延伸項
```

## 2. 自我檢查（交付前）

- 每個 issue 都附可驗證證據（無證據的問題一律移除或降為「需進一步量測」）
- 每個 issue 都有嚴重度與優先序（無「問題流水帳」）
- critical 項在清單最前
- 報告中無自行改碼 / 自行修復的內容（只診斷與建議）
- 執行摘要明確對齊半年自然搜尋 KPI

## 3. 回報 Manager

- 交付稽核報告 + 執行摘要
- 標明哪些修復需 Manager 轉派 sw team
- 若稽核過程觸發 Scope Guard 或需 calculator，於回報中註明

## 錯誤處理

| 情況 | 處理方式 |
|------|---------|
| plan 段清單為空（無問題） | 產出「技術健康，無重大問題」報告，附已檢查面向 |
| 報告產出工具失敗 | 停止並回報已完成與失敗的步驟，不交付半成品 |
| 被要求直接修復 | 觸發 Scope Guard，回報應由 sw team 實作 |
