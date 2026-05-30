# Analytics Reporter — Report Flow (Execute/Verify)

R-D-V 第三段：把分析結果產出為對齊 KPI、可行動的成效報告並寫入 output_path。由 workflow.yaml `report` 步驟 route 到此。

## 流程步驟

### 1. 組裝報告
報告結構：
```
performance_report:
  period: { start, end, baseline_period }
  sources: [GSC, GA4, rank_tool]   # 每指標標來源
  kpi_alignment:
    half_year_goals: [ ... ]
    attainment:                    # 達成率經 calculator 驗證；未驗證標「待驗證」
      - metric, current, target, attainment_rate, status(落後/達標/超前)
  rankings:
    movers_up: [...]
    movers_down: [...]
    top3_count, top10_count
  organic_traffic:
    gsc: { impressions, clicks, ctr, avg_position }
    ga4: { sessions, engagement, conversions }
    note: "GA4 與 GSC 指標不可直接相比"
  attribution:
    confirmed: [ change to event + 證據 ]
    correlated_unproven: [ change to 推測 + 需補的驗證資料 ]
  ab_observation:
    isolated: true|false
    conclusion: 有效/無效/僅觀察值
  actionable_signals:
    - 落後需介入: [...]
    - 成長值得加碼: [...]
    - 下滑需調查: [...]
```

### 2. 自我驗證（Verify）
交付前逐項自查：
- 每個數字是否標來源 + 期間 + 比較基準？
- 所有精確數值是否經 calculator 驗證？未驗證的是否已標「待驗證」？
- 是否有把相關當因果未標證據強度的歸因？
- A/B 結論是否在變因未隔離時誤下定論？
- 是否回扣半年 KPI 目標？是否有可行動訊號（非純數據傾倒）？
- 任一項不通過則退回 analyze-flow 對應步驟修正。

### 3. 寫入輸出
- 工具：`Write`
- 路徑：Manager 派遣訊息的 `output_path`（典型 `<CWD>/output/seo/{task_id}/`）。
- 若無 output_path：停止並回報 `SCOPE VIOLATION: missing output_path`，不寫入自己的 agent 資料夾。

### 4. 回報 Manager
- 摘要：KPI 達成度、關鍵成效變化、可行動訊號、待驗證數字清單、資料缺口。

## 注意事項
- 報告結尾必含可行動訊號，否則不算完成。
- 待驗證數字必須明確標示，不得以未驗證數字作為決策結論。
