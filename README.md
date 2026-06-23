# Anthropic Java Skills

> 把 Java 相关书籍转化为可直接给 Claude / Claude Code 使用的 **Agent Skills**，每个 skill 提供**中文（zh）与英文（en）两种版本**。
>
> A catalog of **Agent Skills** distilled from Java books, ready to use with Claude / Claude Code. Every skill ships in both **Chinese (zh)** and **English (en)**.

---

## 目录结构 · Repository layout

```
Anthropic-Java-Skills/
├── README.md
├── skills/                                  # 所有 skill 合集 · all skills
│   └── alibaba-java-coding-guidelines/      # 一本书 = 一个目录 · one book = one folder
│       ├── README.md                        # 该书说明 · per-book readme
│       ├── zh/                              # 中文版 skill · Chinese skill
│       │   ├── SKILL.md
│       │   └── references/
│       └── en/                              # 英文版 skill · English skill
│           ├── SKILL.md
│           └── references/
└── docs/                                    # 源 PDF 等资料（默认 .gitignore）· source PDFs (gitignored)
```

约定 · Conventions:

- **一本书一个目录**，目录名用 kebab-case，放在 `skills/` 下。
- 每本书下固定有 `zh/` 与 `en/` 两个子目录，**各自是一个完整、独立的 skill**（含自己的 `SKILL.md` 和 `references/`）。
- 同一本书的 `zh` 与 `en` 共享同一个 `name`（frontmatter 中的 `name` 字段），因此**同一个 skills 目录里同一本书只装一种语言**，避免重名冲突。
- 源 PDF / 电子书放在 `docs/`，通过 `.gitignore` 排除，**不提交版权材料**。

## 技能清单 · Skill catalog

| 书籍 · Book | 目录 · Folder | 语言 · Languages | 说明 · Description |
|------|------|------|------|
| 阿里巴巴 Java 开发手册（黄山版） | [`skills/alibaba-java-coding-guidelines`](skills/alibaba-java-coding-guidelines) | zh · en | 阿里官方 Java 编码规约：编程规约、异常日志、单元测试、安全、MySQL、工程结构、设计。用于按规约编写与审查 Java 代码。 |

> 更多书籍持续添加中。·More books coming.

## 如何使用 · How to install & use

这些 skill 不会因为放在本仓库就自动加载——本仓库是**合集 / 分发库**。要在 Claude Code 中启用某个 skill，把你需要的**那一种语言**的目录装到 skills 搜索路径下即可。

These skills are **not** auto-loaded just by living in this repo — it is a **catalog**. To enable one in
Claude Code, install the **single language** you want into a skills directory.

**方式一：复制 · Copy**

```bash
# 用户级（全局可用）· user-level (available everywhere)
cp -r skills/alibaba-java-coding-guidelines/zh ~/.claude/skills/alibaba-java-coding-guidelines

# 或项目级（仅某个项目）· or project-level (one project only)
cp -r skills/alibaba-java-coding-guidelines/en /path/to/your/project/.claude/skills/alibaba-java-coding-guidelines
```

**方式二：软链接（便于随仓库更新）· Symlink (stays in sync with this repo)**

```bash
ln -s "$(pwd)/skills/alibaba-java-coding-guidelines/zh" ~/.claude/skills/alibaba-java-coding-guidelines
```

装好后，在 Claude Code 里写或审查 Java 代码时，Claude 会按描述自动调用该 skill；也可以直接用 `/` 触发。
想换语言时，删掉已装目录再装另一种即可。

After installing, Claude consults the skill automatically when you write or review Java code (per its
`description`), or you can trigger it explicitly. To switch language, remove the installed copy and
install the other one.

## 新增一本书 · Adding a new book

1. 在 `skills/` 下新建 `your-book-name/`（kebab-case）。
2. 建 `zh/` 与 `en/` 两个子目录，各放一个 `SKILL.md` + `references/`。
3. `SKILL.md` 需带 frontmatter：`name`（同书两语言一致）、`description`（写清触发场景，可适当“主动”一些以提升触发率）。
4. 把正文按主题拆进 `references/`，`SKILL.md` 给出索引与高频速查，遵循**渐进式披露**（SKILL.md 精简，细节按需读取）。
5. 更新本 README 的「技能清单」表，并在 `skills/your-book-name/README.md` 写本书说明。
6. 源电子书放 `docs/` 并确保被 `.gitignore` 排除。

可借助官方 `skill-creator` 技能来起草、校验与评测新 skill。·You can use the official `skill-creator` skill to draft, validate, and evaluate new skills.

## 许可与版权 · License & attribution

本仓库的 skill 内容**改编自各书原文**，版权归原作者 / 出版方所有，仅供学习与工程实践参考。请勿在此仓库提交受版权保护的原始电子书文件（已通过 `.gitignore` 排除 `*.pdf`）。

Skill content here is **adapted from the respective books**; copyright belongs to the original
authors/publishers and is provided for learning and engineering reference. Do not commit copyrighted
source e-books (the `.gitignore` already excludes `*.pdf`).
