<p align="center">
  <img src="https://cdn.jsdelivr.net/gh/JohnRothan/Anthropic-Java-Skills@master/assets/readme.webp" alt="Java Books to Claude Skills" width="720">
</p>

<p align="center">
  <b>English</b> ·
  <a href="README.zh-CN.md">简体中文</a> ·
  <a href="README.ja.md">日本語</a> ·
  <a href="README.ko.md">한국어</a> ·
  <a href="README.es.md">Español</a> ·
  <a href="README.fr.md">Français</a> ·
  <a href="README.de.md">Deutsch</a> ·
  <a href="README.pt-BR.md">Português</a> ·
  <a href="README.ru.md">Русский</a>
</p>

# Anthropic Java Skills

> A catalog of **Agent Skills** distilled from Java books, ready to use with Claude / Claude Code. Every skill ships in both **Chinese (zh)** and **English (en)**.

## Repository layout

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

Conventions:

- **One book, one folder**, named in kebab-case, placed under `skills/`.
- Each book has fixed `zh/` and `en/` subfolders, **each a complete, standalone skill** (with its own `SKILL.md` and `references/`).
- A book's `zh` and `en` share the same `name` (the `name` field in the frontmatter), so **install only one language per skills directory** for a given book to avoid name collisions.
- Source PDFs / e-books go under `docs/` and are excluded via `.gitignore` — **do not commit copyrighted material**.

## Skill catalog

| Book | Folder | Languages | Description |
|------|--------|-----------|-------------|
| Alibaba Java Development Manual (Huangshan Edition) | [`skills/alibaba-java-coding-guidelines`](skills/alibaba-java-coding-guidelines) | zh · en | Alibaba's official Java coding standards: programming, exception & logging, unit testing, security, MySQL, project structure, design. For writing and reviewing Java code per the standard. |

> More books coming.

## How to install & use

These skills are **not** auto-loaded just by living in this repo — it is a **catalog**. To enable one in
Claude Code, install the **single language** you want into a skills directory.

**Option 1: Copy**

```bash
# user-level (available everywhere)
cp -r skills/alibaba-java-coding-guidelines/en ~/.claude/skills/alibaba-java-coding-guidelines

# or project-level (one project only)
cp -r skills/alibaba-java-coding-guidelines/zh /path/to/your/project/.claude/skills/alibaba-java-coding-guidelines
```

**Option 2: Symlink (stays in sync with this repo)**

```bash
ln -s "$(pwd)/skills/alibaba-java-coding-guidelines/en" ~/.claude/skills/alibaba-java-coding-guidelines
```

After installing, Claude consults the skill automatically when you write or review Java code (per its
`description`), or you can trigger it explicitly. To switch language, remove the installed copy and
install the other one.

## Adding a new book

1. Create `skills/your-book-name/` (kebab-case) under `skills/`.
2. Add `zh/` and `en/` subfolders, each with a `SKILL.md` + `references/`.
3. `SKILL.md` needs frontmatter: `name` (identical across both languages of a book) and `description` (state the trigger scenarios clearly; being a little "proactive" improves trigger rate).
4. Split the body by topic into `references/`, give `SKILL.md` an index and a high-frequency cheat sheet, and follow **progressive disclosure** (keep `SKILL.md` lean, load details on demand).
5. Update the "Skill catalog" table in this README and write a per-book `skills/your-book-name/README.md`.
6. Put the source e-book under `docs/` and make sure it's excluded by `.gitignore`.

You can use the official `skill-creator` skill to draft, validate, and evaluate new skills.

## License & attribution

Skill content here is **adapted from the respective books**; copyright belongs to the original
authors/publishers and is provided for learning and engineering reference. Do not commit copyrighted
source e-books (the `.gitignore` already excludes `*.pdf`).
