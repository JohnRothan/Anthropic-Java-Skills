<p align="center">
  <img src="assets/readme.png" alt="Java Books to Claude Skills" width="720">
</p>

<p align="center">
  <a href="README.md">English</a> ·
  <b>简体中文</b> ·
  <a href="README.ja.md">日本語</a> ·
  <a href="README.ko.md">한국어</a> ·
  <a href="README.es.md">Español</a> ·
  <a href="README.fr.md">Français</a> ·
  <a href="README.de.md">Deutsch</a> ·
  <a href="README.pt-BR.md">Português</a> ·
  <a href="README.ru.md">Русский</a>
</p>

# Anthropic Java Skills

> 从 Java 书籍中提炼而成的 **Agent Skills** 目录，可直接配合 Claude / Claude Code 使用。每个技能都同时提供 **中文（zh）** 和 **英文（en）** 两个版本。

## 仓库结构

```
Anthropic-Java-Skills/
├── README.md                                # this file (English); other languages: README.<lang>.md
├── assets/                                  # images used by the READMEs
├── skills/                                  # all skills
│   └── alibaba-java-coding-guidelines/      # one book = one folder
│       ├── README.md                        # per-book readme
│       ├── zh/                              # Chinese skill
│       │   ├── SKILL.md
│       │   └── references/
│       └── en/                              # English skill
│           ├── SKILL.md
│           └── references/
└── docs/                                    # source PDFs and material (gitignored)
```

约定：

- **一本书对应一个文件夹**，采用 kebab-case 命名，置于 `skills/` 目录下。
- 每本书都有固定的 `zh/` 和 `en/` 子文件夹，**每一个都是完整、独立的技能**（各自带有 `SKILL.md` 和 `references/`）。
- 一本书的 `zh` 和 `en` 共享相同的 `name`（frontmatter 中的 `name` 字段），因此对于同一本书，**每个 skills 目录只安装一种语言**，以避免名称冲突。
- 源 PDF / 电子书放在 `docs/` 目录下，并通过 `.gitignore` 排除——**请勿提交受版权保护的材料**。

## 技能目录

| Book | Folder | Languages | Description |
|------|--------|-----------|-------------|
| 阿里巴巴 Java 开发手册（黄山版） | [`skills/alibaba-java-coding-guidelines`](skills/alibaba-java-coding-guidelines) | zh · en | 阿里巴巴官方 Java 编码规范：编程规约、异常与日志、单元测试、安全、MySQL、工程结构、设计规约。用于按规范编写和评审 Java 代码。 |

> 更多书籍即将加入。

## 如何安装与使用

这些技能**不会**因为存在于本仓库中就被自动加载——这是一个**目录**。要在 Claude Code 中启用某个技能，请将你需要的**单一语言**版本安装到 skills 目录中。

**方式 1：复制**

```bash
# user-level (available everywhere)
cp -r skills/alibaba-java-coding-guidelines/en ~/.claude/skills/alibaba-java-coding-guidelines

# or project-level (one project only)
cp -r skills/alibaba-java-coding-guidelines/zh /path/to/your/project/.claude/skills/alibaba-java-coding-guidelines
```

**方式 2：符号链接（与本仓库保持同步）**

```bash
ln -s "$(pwd)/skills/alibaba-java-coding-guidelines/en" ~/.claude/skills/alibaba-java-coding-guidelines
```

安装完成后，当你编写或评审 Java 代码时，Claude 会根据其 `description` 自动调用该技能，你也可以显式触发它。若要切换语言，请删除已安装的副本，然后安装另一种语言版本。

## 添加一本新书

1. 在 `skills/` 下创建 `skills/your-book-name/`（kebab-case）。
2. 添加 `zh/` 和 `en/` 子文件夹，每个都包含一个 `SKILL.md` + `references/`。
3. `SKILL.md` 需要 frontmatter：`name`（一本书的两种语言之间保持一致）和 `description`（清晰说明触发场景；稍微"主动"一些有助于提高触发率）。
4. 按主题将正文拆分到 `references/` 中，为 `SKILL.md` 提供索引和高频速查表，并遵循**渐进式披露**原则（保持 `SKILL.md` 精简，按需加载细节）。
5. 更新本 README 中的"技能目录"表格，并为该书编写一份 `skills/your-book-name/README.md`。
6. 将源电子书放在 `docs/` 下，并确保它被 `.gitignore` 排除。

你可以使用官方的 `skill-creator` 技能来起草、验证和评估新技能。

## 许可与署名

此处的技能内容**改编自各自对应的书籍**；版权归原作者/出版方所有，仅供学习和工程参考之用。请勿提交受版权保护的源电子书（`.gitignore` 已排除 `*.pdf`）。
