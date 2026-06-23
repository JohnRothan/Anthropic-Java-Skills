# 阿里巴巴 Java 开发手册（黄山版）· Alibaba Java Development Manual (Huangshan Edition)

把阿里巴巴官方《Java 开发手册（黄山版）》转化为可执行的**编码与代码审查指南**的 Agent Skill。
An Agent Skill that turns Alibaba's official *Java Development Manual (Huangshan Edition)* into
actionable **coding and code-review** guidance.

## 内容 · What's inside

覆盖手册七大维度，每条规则保留官方等级标签（【强制】/【推荐】/【参考】）与正反例：
Covers all seven dimensions; every rule keeps its official severity tag
([Mandatory]/[Recommended]/[Reference]) and positive/counter examples:

| # | 维度 · Dimension | zh | en |
|---|------|----|----|
| 1 | 编程规约 · Programming | `zh/references/01-编程规约.md` | `en/references/01-programming.md` |
| 2 | 异常日志 · Exception & Logging | `zh/references/02-异常日志.md` | `en/references/02-exception-and-logging.md` |
| 3 | 单元测试 · Unit Testing | `zh/references/03-单元测试.md` | `en/references/03-unit-testing.md` |
| 4 | 安全规约 · Security | `zh/references/04-安全规约.md` | `en/references/04-security.md` |
| 5 | MySQL 数据库 · MySQL Database | `zh/references/05-MySQL数据库.md` | `en/references/05-mysql-database.md` |
| 6 | 工程结构 · Project Structure | `zh/references/06-工程结构.md` | `en/references/06-project-structure.md` |
| 7 | 设计规约 · Design | `zh/references/07-设计规约.md` | `en/references/07-design.md` |
| — | 专有名词 · Glossary | `zh/references/08-专有名词解释.md` | `en/references/08-glossary.md` |

`SKILL.md` 提供「高频强制规则速查」和参考索引，采用渐进式披露——常见场景无需展开参考文件。
`SKILL.md` provides a high-frequency mandatory-rules cheat sheet plus a reference index, using
progressive disclosure — common cases need no reference file at all.

## 安装 · Install

选择一种语言安装到 skills 目录（详见仓库根 README）：
Install one language into a skills directory (see the repo-root README for details):

```bash
# 中文 · Chinese
cp -r zh ~/.claude/skills/alibaba-java-coding-guidelines
# 英文 · English
cp -r en ~/.claude/skills/alibaba-java-coding-guidelines
```

## 触发示例 · Example triggers

- “帮我写一个用户 Service / DAO 类” · "Write me a user Service / DAO class"
- “review 一下这段 Java 代码符不符合规范” · "Review whether this Java is compliant"
- “建一张订单表的 MySQL DDL” · "Create the MySQL DDL for an orders table"
- “这里的异常和日志该怎么处理” · "How should exceptions and logging be handled here"

## 来源 · Source

阿里巴巴 P3C 官方仓库 · Alibaba P3C official repo: https://github.com/alibaba/p3c
（原始 PDF 不随仓库提交 · the original PDF is not committed to this repo）
