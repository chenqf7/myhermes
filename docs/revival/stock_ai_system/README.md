# AI 股票情报与模拟盘系统

用于每日跟踪 AI + 股票相关信息、生成模拟盘建议、收盘验证并迭代规则。

> 说明：这里只做研究与模拟盘验证，不构成投资建议，不保证收益。

## 用户偏好

- 主要市场：美股
- 次要市场：港股
- 风险偏好：中性
- 模拟盘总资金：4 万人民币等值
- 股票池：不固定，动态根据 X/Twitter、新闻、财报、宏观、资金面筛选
- X/Twitter 参考：重点关注 dexeteryy 等 AI/股票相关账号
- 股票工具 repo：`/Users/chenchen/Documents/Codex/2026-05-11/github-myhermes/directional-ai-executor`

## 定时任务

- 盘前建议：工作日每天 08:00（北京时间），Cron job `e12e0a1d6832`
- 港股/亚洲时段验证：工作日每天 16:30（北京时间），Cron job `ff242c959eaf`
- 美股收盘验证：每周二到周六 06:00（北京时间），Cron job `507845c413b6`

## 文件

- `paper_portfolio.json`：模拟盘状态、每日建议与 P&L
- `journal.md`：每日简报/复盘流水
- `rules.md`：信号权重、风控、迭代规则

## X/Twitter 配置状态

当前机器未检测到 `xurl` 命令。配置完成前，任务会用公开网页/新闻/RSS/搜索兜底。
