# Changelog

All notable changes to this skill will be documented in this file.

## [1.1.0] - 2026-07-14

### Added
- 三级成熟度模型（L1原型 / L2迭代 / L3发布），按成熟度确定审计范围和严格度
- 4 个用户确认点（成熟度确认 / 报告后选择 / 整改后选择 / 发布确认）
- 整改模式（用户授权后帮修复 P0/P1/P2 问题，最小化改动）
- 严格发布门槛：绝不自动发布，必须用户在确认点4明确选"是，发布"后才提供发布建议
- 成熟度覆盖：用户可强制指定等级
- 新增 references/maturity-model.md（三级成熟度详细模型）
- 各维度检查项标注适用等级（L1/L2/L3）

### Changed
- 审计范围按成熟度动态调整（L1: 3项精简 / L2: 6维度 / L3: 全量8维度）
- 严格度按成熟度分级（L1: 只报Critical / L2: Critical必修+Important建议 / L3: Critical+Important必修）
- L1/L2 阶段不显示发布选项

## [1.0.0] - 2026-07-14

### Added
- 初始版本：8 维度深度审计（S/T/A/E/C/P/D/Q）
- 4 种入口模式：全量/单维度/批量/回归
- 标准化审计报告（严重性分级 + 修复建议 + 优先级）
- 回归审计机制（前后差异对比）
- 独立审计定位：专注深度审计+整改+回归，不负责创建或发布
- 5 个 references 文件：audit-dimensions / security-scan / benchmarking / report-template / regression
