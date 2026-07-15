# Changelog

All notable changes to this skill will be documented in this file.

## [1.2.1] - 2026-07-15

### Fixed
- **安全修复**：嫁接清洗触发词不再跳过确认点 O。所有触发词（含"嫁接清洗"/"清洗嫁接痕迹"）都必须经过确认点 O 用户授权后才能执行 Edit 操作（修复 SkillSpector Finding #4/#6：Missing User Warnings + Intent-Code Divergence）
- originality-check.md Phase O-4 段追加前置条件说明：未授权 = 只输出清单，不修改任何文件
- originality-check.md 主动触发模式表追加安全约束：任何触发词都不能绕过确认点 O

### Changed
- README.md 用户须知段优化：明确"本技能不执行发布，仅提供发布建议由用户手动执行"，消除"不负责发布"与"用户授权后可发布"的措辞矛盾（修复 SkillSpector Finding #1/#2：Description-Behavior Mismatch + Intent-Code Divergence）
- README.md 用户须知段补充原创度清洗子模式（A2）的授权说明（修复 SkillSpector Finding #3）
- README.en.md 同步以上变更

### SkillSpector Findings 评估
- Finding #1/#2（发布职责矛盾）：过度解读，但措辞优化可降低误判
- Finding #3（审计却改文件）：合理担忧，已通过强化授权说明解决
- Finding #4/#6（嫁接清洗跳过确认点）：**确实有问题，已修复** ✅
- Finding #5（触发词太宽泛）：过度解读，不改（审计类技能固有特性）

## [1.2.0] - 2026-07-14

### Added
- D 维度新增 D-O1~O7 嫁接痕迹检查项（7 类：直接继承声明/Do NOT 指向具体技能名/三角分工定位/发布交接措辞/示例点名其他技能/CHANGELOG 历史痕迹/场景描述点名）
- 原创度审计专项触发词：「原创度审计」/「嫁接清洗」/「清洗嫁接痕迹」
- 整改模式新增 A1/A2/A3 子模式：A1 标准整改 / A2 原创度优化（仅清洗嫁接痕迹）/ A3 两者都做
- A2 子模式含 6 阶段流程：扫描 → 分类标记 → 【确认点 O】 → 执行清洗 → 7 项验证 → 回归报告
- 新增 references/originality-check.md（7 类痕迹识别模式 + 清洗策略表 + 7 项验证清单 + 主动触发模式 + 评分影响 + 清洗边界 + 常见误区）
- 新增示例4：原创度审计 + 嫁接清洗（A2 子模式完整流程演示）

### Changed
- D 维度概览追加"嫁接痕迹(D-O1~O7)"检查项
- 触发词段合并原创度专项触发词到主触发词列表
- 规则段合并网络降级+回归前提（规则6），精简表述
- 示例1/2 压缩空行（保持语义不变）

### Design Principle
- 核心判断铁律：能用「场景描述」替代「具体技能名」就替代，不能替代的（自引用/自身触发词）保留
- 清洗只改"措辞"，不改"语义"
- L1 跳过 D-O 检查（原型阶段允许快速派生），L2 检查 Important 类，L3 全部检查

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
