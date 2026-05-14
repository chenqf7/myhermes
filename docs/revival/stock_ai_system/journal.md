     1|# Journal
     2|
     3|系统初始化：已创建每日盘前建议与收盘验证任务。
     4|
     5|

## 2026-05-13 第一轮盘中模拟建议

- 时间：2026-05-13 23:15:27 UTC+08:00
- 模拟资金：40,000 CNY；首轮建议总仓位：50%（约 20,000 CNY）
- 核心：NVDA 18%、GOOGL 14%、BABA 10%、TSLA 8%；PLTR 观察不买。
- 数据：Stooq/Yahoo 公开行情，X API 未配置，X 信号暂用公开搜索兜底。
- 状态：待美股收盘后验证。

## 2026-05-14 收盘验证：2026-05-13 美股模拟建议

- 验证时间：2026-05-14 06:00 UTC+08:00；数据源：Stooq quote CSV（公开延迟数据，盘后抓取；行情时间约 2026-05-13 22:00:18-22:00:21）。
- 结论：按“日 OHLC 触发止损/目标、未知先后顺序则风控优先”的保守规则，组合亏损约 -470.26 CNY（-1.1757%/总资金）；连续盈利天数归零。
- 收盘盯市口径其实为 +110.53 CNY（+0.2763%），但 GOOGL/TSLA/BABA 日内低点均触发止损，说明仅看收盘会掩盖盘中风险。
- 单票：NVDA 未触发止损/目标，收盘小亏；GOOGL 低点 385.01 跌破 386 止损；TSLA 低点 430.21 跌破 435 止损；BABA 低点 130.33 跌破 136 止损。
- 错误归因：入场条件对开盘低于止损、盘中宽幅震荡、突破前低点未修复的过滤不足；BABA 虽收盘走强，但风控口径不合格。
- X/Twitter：xurl/API 未配置，dexeteryy 本轮未直接验证，可信度不加分。
- 工具仓库：已在 directional-ai-executor/examples/ 增加 paper_validation_2026-05-13.json，作为低风险可验证模拟盘记录样例。

## 2026-05-14 pre-market paper plan

- Time: 2026-05-14 08:00 UTC+08. xurl not found; did not read ~/.xurl; dexeteryy direct feed unavailable.
- Because 2026-05-13 was a risk-rule loss, exposure is reduced from 50 percent to 35 percent and no gap-up chasing is allowed.
- Paper plan: MSFT 12 percent, META 10 percent, NVDA 8 percent, AMD 5 percent, BABA/9988 watch or reduce only.
- directional-ai-executor: 26 unit tests passed; no code change today; next low-risk idea is an OHLC risk-first validation helper.

## 2026-05-14 港股/亚洲时段收盘验证

- 验证范围：仅验证当日计划中的港股/亚洲时段相关建议（BABA / 9988.HK）；MSFT/META/AMD/NVDA 等美股建议仍待美股收盘复盘，不在本轮重复验证。
- 行情：9988.HK 2026-05-14 收 137.90 HKD，较前一港股日收盘 132.80 上涨约 +3.84%；日内高开 143.10、高 144.00、低 137.40，收盘较开盘约 -3.63%，成交 171,292,901 股。数据源为 Yahoo Finance chart；Stooq 对 9988.HK 返回仍停在 2026-05-13，未用于最终口径。
- 建议验证：原建议为 reduce_or_watch、权重 0%、不新增敞口；148-152 减仓/观察区未触及，且收盘低于 140 观察阈值，高开回落说明“不追、不新增”的风险规避信号有效。
- 模拟 P&L：建议仓位为 0%，按 40,000 CNY 总资金计算，本轮港股 P&L = 0.00 CNY；有效信号 1，失效信号 0。
- 工具仓库 directional-ai-executor：路径存在；本轮不做高风险改动。低风险计划：后续可补充 HK/ADR 同名标的映射与 0 仓位风险规避信号的验证样例。
