# 8 大审计维度详细检查项

**When to read**: Phase 1-3 执行审计时。本文件列出每个维度的完整 checklist，审计员按维度逐项检查并记录 Findings。

**成熟度分级**：各检查项标注适用等级（L1/L2/L3）。L1 只跑标注 L1 的项，L2 跑 L1+L2 项，L3 跑全部。

---

## 维度 S：结构合规审计（Structure）

| 编号 | 检查项 | 等级 | 方法 | PASS 标准 | 严重性 |
|------|--------|------|------|-----------|--------|
| S1 | frontmatter 必填字段 | L1 | Read SKILL.md 头部 | name + description 必填 | Important if 缺 |
| S2 | name 符合 kebab-case | L1 | 正则 `^[a-z][a-z0-9-]*$` | 全小写+连字符 | Minor |
| S3 | description ≤ 200 字符 | L1 | `len(description)` | ≤200（核心词在前200字符） | Important if >250 |
| S4 | description 三要素 | L1 | 人工+模式匹配 | 做什么+何时触发+Do NOT | Important if 缺 Do NOT |
| S5 | 4 模块齐全 | L1 | Grep `## 任务` `## 输出格式` `## 规则` `## 示例` | 4 标题都存在 | Important if 缺任一 |
| S6 | SKILL.md ≤ 200 行 | L1 | `wc -l SKILL.md` | ≤200（硬上限 300） | Important if >200 |
| S7 | 规则实习生测试 | L2 | 人工审查每条规则 | 每条可直接执行 | Minor if 模糊 |
| S8 | 示例覆盖边界 | L2 | 人工审查示例 | 至少 1 个边界情况 | Minor |
| S9 | 渐进式披露 | L2 | 检查 SKILL.md 是否只放导航 | 详细内容下沉 references/scripts/assets | Important |
| S10 | 目录结构 | L2 | LS 目录 | 标准目录结构完整 | Minor |

---

## 维度 T：安全合规审计（Trust）

**详细扫描模式见 [`security-scan.md`](security-scan.md)**，本表为概览：

| 编号 | 检查项 | 等级 | 方法 | 严重性 |
|------|--------|------|------|--------|
| T1 | Layer 1 凭证泄露 | L2 | Grep 扩展模式 | Critical if 真实凭证 |
| T2 | Layer 2 本地路径 | L2 | Grep `C:\\\|D:\\\|/Users/` | Important |
| T3 | Layer 3 危险命令 | L2 | Grep `curl\|wget\|eval\|exec` | Critical if 外发数据 |
| T4 | Layer 4 YARA 触发词 | L3 | Grep 5 类字面量 | Critical（即使文档中也触发） |
| T5 | Layer 5 SSD3 派生输出 | L3 | 检查代码读敏感文件→写持久化输出 | Important |
| T6 | MCP Tool Poisoning 行为声明 | L3 | description 是否完整声明全部行为 | Important |
| T7 | MCP Least Privilege 权限声明 | L3 | SKILL.md/plugin.json 是否声明权限 | Important |
| T8 | Missing User Warnings | L3 | 有副作用的 Skill README 是否含警告 | Important |
| T9 | Description-Behavior Mismatch | L3 | description 与实际行为一致 | Important |
| T10 | Self-Modification 措辞 | L3 | 避免"update SKILL.md"等 | Minor |
| T11 | 安全敏感方案不文档化 | L3 | 不描述绕过网络限制方案 | Important |
| T12 | Pre-Scan 文件清单 | L2 | LS 检查凭证文件和临时脚本 | Critical if 存在 |
| T13 | 最小权限校验 | L2 | allowed-tools 与任务无关权限删除 | Minor |
| T14 | 国内可用性 | L3 | 依赖 API 国内可访问 | Important if 海外独占 |

---

## 维度 A：触发可靠性审计（Applicability）

| 编号 | 检查项 | 等级 | 方法 | PASS 标准 | 严重性 |
|------|--------|------|------|-----------|--------|
| A1 | 正例触发测试 | L1 | 构造 3 条真实说法（L1精简） | 3 条都 should_trigger: true | Important if <2/3 |
| A2 | 反例触发测试 | L1 | 构造 2 条不应触发的说法 | 2 条都 should_trigger: false | Important if 误触发 |
| A3 | 触发词冲突检测 | L2 | Grep 已安装 Skill 的 description | 无冲突 | Important if 冲突 |
| A4 | 关键词前置 | L2 | 核心触发词在前 200 字符 | 前 200 字符含主关键词 | Minor |
| A5 | Do NOT 有效性 | L2 | Do NOT 声明具体可执行 | 不写"等"模糊范围 | Minor |

**正例构造方法**：
1. 标准说法（与 description 触发词一致）
2. 口语化改写（"做个X" → "帮我搞个X"）
3. 模糊表达（"我需要处理Y" → 隐含触发词）
4. 同义词替换（"审计" → "审查/检查/核验"）
5. 长句包含（"在我做完Z之后，还需要X"）

**反例构造方法**：
1. 相邻领域（"技能创建" 对 skill-auditor 是反例）
2. 字面相似但意图不同（"技能学习" 不是 "技能审计"）
3. Do NOT 明确排除的场景

---

## 维度 E：功能有效性审计（Effectiveness）

| 编号 | 检查项 | 等级 | 方法 | PASS 标准 | 严重性 |
|------|--------|------|------|-----------|--------|
| E1 | 声明一致性 | L2 | 审查 SKILL.md 声明的输出格式是否具体可执行 | 输出格式具体 | Important if 模糊 |
| E2 | 规则可执行性 | L2 | 审查规则是否能指导输出达到声明质量 | 规则可执行 | Important if 不可执行 |
| E3 | 示例覆盖声明能力 | L2 | 审查示例是否覆盖声明的能力范围 | 覆盖 | Minor if 部分缺失 |
| E4 | 增量价值 | L3 | 信息整合/判断建议/质量提升 | 至少一项 | Important if 仅格式转换 |
| E5 | 基线对比 | L3 | 不用 Skill vs 用 Skill | Skill 有增益 | FYI |

**注意**：E 维度不实际执行被审计 Skill 代码，只做声明一致性验证：
- 审查 SKILL.md 声明的输出格式是否具体可执行
- 审查规则是否能指导输出达到声明质量
- 审查示例是否覆盖声明的能力范围

---

## 维度 C：同类竞争审计（Competition）

**详细方法论见 [`benchmarking.md`](benchmarking.md)**，本表为概览（**仅 L3 执行**）：

| 编号 | 检查项 | 等级 | 方法 | 严重性 |
|------|--------|------|------|--------|
| C1 | SkillHub 搜索排名 | L3 | API + 质量排序公式 | FYI |
| C2 | 腾讯9维度比对 | L3 | 与 Top3 比对 | Important if 盲区 |
| C3 | 差异化分析 | L3 | 明确记录差异化 | FYI |
| C4 | 盲区识别 | L3 | 列出 Top3 揭示的盲区 | Important |
| C5 | 重复判定 | L3 | 重复度高 → 建议安装已有 | Important if >70% 重叠 |

---

## 维度 P：平台合规审计（Platform）

**仅 L3 执行**：

| 编号 | 检查项 | 等级 | 方法 | 严重性 |
|------|--------|------|------|--------|
| P1 | TRACE 五维度 | L3 | T/R/A/C/E 综合 | Critical if 任一 FAIL |
| P2 | SkillSpector 9 项 | L3 | YARA/Desc-Mismatch/自修改/CHANGELOG/SSD3/MCP/Least Priv/User Warnings | Critical if 任一 FAIL |
| P3 | SkillHub 文件限制 | L3 | 检查 .gitignore/LICENSE/.claude-plugin/.github | Important if 存在 |
| P4 | ClawHub 自动文件 | L3 | 检查 skill-card.md/.clawhub/ | Important if 存在 |
| P5 | 三平台 frontmatter 兼容 | L3 | ClawHub(name/description) + SkillHub(slug/displayName/version/summary/license) | Important if 缺字段 |

---

## 维度 D：文档一致性审计（Documentation）

| 编号 | 检查项 | 等级 | 方法 | PASS 标准 | 严重性 |
|------|--------|------|------|-----------|--------|
| D1 | SKILL.md 内部引用一致 | L1 | Grep `\[.*\]\(references/.*\.md\)` → 检查文件存在 | 所有引用文件存在 | Important if 断链 |
| D2 | README 与 SKILL.md 一致 | L2 | 对比功能范围 | 一致 | Minor if 不一致 |
| D3 | 中英文 README 同步 | L3 | 对比 README.md 与 README.en.md | 内容同步 | Minor |
| D4 | CHANGELOG 版本号 | L2 | CHANGELOG 最新版本 = frontmatter version | 一致 | Important if 不一致 |
| D5 | plugin.json 与 frontmatter | L2 | 对比 name/version/description | 一致 | Minor |
| D6 | references 文件被引用 | L2 | 检查孤立文件 | 无孤立 | Minor |
| D7 | 示例完整性 | L2 | 示例覆盖 4 模块声明的输出格式 | 覆盖 | Minor |

---

## 维度 Q：代码质量审计（Code Quality）

**检查范围**：`scripts/` 下的 Python / JavaScript / PowerShell 脚本

| 编号 | 检查项 | 等级 | 方法 | PASS 标准 | 严重性 |
|------|--------|------|------|-----------|--------|
| Q1 | 错误处理 | L2 | Grep `try\|except\|catch` | 外部调用有错误处理 | Important if 无 |
| Q2 | 资源管理 | L2 | Grep `with \|finally\|close()` | 文件/网络/进程正确关闭 | Important if 泄漏 |
| Q3 | 命名规范 | L2 | 人工审查 | snake_case(Python)/camelCase(JS) | Minor |
| Q4 | 复杂度 | L2 | 单函数 ≤50 行，嵌套 ≤4 层 | 符合 | Minor if 超标 |
| Q5 | 硬编码 | L2 | Grep 路径/URL/凭证模式 | 使用环境变量/配置 | Critical if 凭证硬编码 |
| Q6 | 日志规范 | L3 | Grep `print(` vs `logging` | 生产脚本用 logging | Minor |
| Q7 | 类型注解 | L3 | 检查 Python 函数签名 | 推荐有注解（非强制） | FYI |
| Q8 | 幂等性 | L3 | 人工审查 | 重复执行安全 | Minor |

**无 scripts/ 目录时**：Q 维度标注"无代码，跳过"，不扣分。

---

## 评分计算

每个维度 0-10 分：
- 9-10: 优秀，无问题或仅 FYI
- 7-8: 良好，少量 Minor
- 5-6: 及格，有 Important
- 3-4: 不及格，多个 Important
- 0-2: 严重问题，有 Critical

**综合分加权平均**：
```
综合分 = (S×1.0 + T×2.0 + A×1.0 + E×1.0 + C×0.5 + P×1.5 + D×0.5 + Q×0.5) / 8.0
```

**一票否决**：T 维度有 Critical → 综合状态 = ❌ FAIL（不论综合分多高）
