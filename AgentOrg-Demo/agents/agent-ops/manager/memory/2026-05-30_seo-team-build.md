# Retrospective — 建立 SEO Team (2026-05-30)

trace_id: 791d3a90-70ae-4e8a-b31e-91fae7959218

## 做對什麼
- HITL 先暫停確認(純SEO vs FB社群歧義 + Tier3 大規模),避免建錯方向
- Round 1 五個 builder 並行「只建自己目錄、不碰共用檔」→ 零寫衝突
- Round 2 單一 builder 統一處理 CLAUDE.md/skill 整合
- 每個 agent 各自跑 validate-agent.sh,Manager 再獨立重跑驗證(不盲信下屬)

## 做錯/可改進
- builder 誤判 trace_id 空白成因(以為 script 缺槽位,實為 worker 未從 dispatch prompt 帶入)。Governance 已更正
- 四 worker 的 workflow.yaml log_start 格式不一致(P2),未來建 team 應在 brief 中給統一範本

## 下次改進建議
- dispatch worker 時在 prompt 的 worklog block 明確帶入 trace_id 位置參數
- 建 team 前先給 builder 一份「worker workflow.yaml log_start 標準範本」確保一致
- 跨 agent trace_id 鏈路是 demo 既有 gap,值得日後一次性增強 _worklog_helper.py
