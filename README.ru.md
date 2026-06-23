<p align="center">
  <img src="https://cdn.jsdelivr.net/gh/JohnRothan/Anthropic-Java-Skills@main/assets/readme.webp" alt="Java Books to Claude Skills" width="720">
</p>

<p align="center">
  <a href="README.md">English</a> ·
  <a href="README.zh-CN.md">简体中文</a> ·
  <a href="README.ja.md">日本語</a> ·
  <a href="README.ko.md">한국어</a> ·
  <a href="README.es.md">Español</a> ·
  <a href="README.fr.md">Français</a> ·
  <a href="README.de.md">Deutsch</a> ·
  <a href="README.pt-BR.md">Português</a> ·
  <b>Русский</b>
</p>

# Anthropic Java Skills

> Каталог **Agent Skills**, извлечённых из книг по Java и готовых к использованию с Claude / Claude Code. Каждый навык поставляется как на **китайском (zh)**, так и на **английском (en)** языке.

## Структура репозитория

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

Соглашения:

- **Одна книга — одна папка**, названная в kebab-case и размещённая в `skills/`.
- Каждая книга имеет фиксированные подпапки `zh/` и `en/`, **каждая из которых является полноценным самостоятельным навыком** (со своим собственным `SKILL.md` и `references/`).
- `zh` и `en` одной книги используют одно и то же `name` (поле `name` во frontmatter), поэтому **устанавливайте только один язык на каталог навыков** для данной книги, чтобы избежать конфликтов имён.
- Исходные PDF / электронные книги размещаются в `docs/` и исключаются через `.gitignore` — **не коммитьте материалы, защищённые авторским правом**.

## Каталог навыков

| Книга | Папка | Языки | Описание |
|------|--------|-----------|-------------|
| Руководство по разработке на Java от Alibaba (издание Huangshan) | [`skills/alibaba-java-coding-guidelines`](skills/alibaba-java-coding-guidelines) | zh · en | Официальные стандарты кодирования на Java от Alibaba: программирование, исключения и логирование, модульное тестирование, безопасность, MySQL, структура проекта, проектирование. Для написания и проверки кода на Java в соответствии со стандартом. |

> Будут добавлены новые книги.

## Как установить и использовать

Эти навыки **не** загружаются автоматически только потому, что находятся в этом репозитории — это **каталог**. Чтобы включить навык в Claude Code, установите **один нужный язык** в каталог навыков.

**Вариант 1: Копирование**

```bash
# user-level (available everywhere)
cp -r skills/alibaba-java-coding-guidelines/en ~/.claude/skills/alibaba-java-coding-guidelines

# or project-level (one project only)
cp -r skills/alibaba-java-coding-guidelines/zh /path/to/your/project/.claude/skills/alibaba-java-coding-guidelines
```

**Вариант 2: Символическая ссылка (остаётся синхронизированной с этим репозиторием)**

```bash
ln -s "$(pwd)/skills/alibaba-java-coding-guidelines/en" ~/.claude/skills/alibaba-java-coding-guidelines
```

После установки Claude автоматически обращается к навыку, когда вы пишете или проверяете код на Java (согласно его `description`), либо вы можете запустить его явно. Чтобы сменить язык, удалите установленную копию и установите другую.

## Добавление новой книги

1. Создайте `skills/your-book-name/` (в kebab-case) внутри `skills/`.
2. Добавьте подпапки `zh/` и `en/`, каждая с `SKILL.md` + `references/`.
3. `SKILL.md` требует frontmatter: `name` (идентичный для обоих языков одной книги) и `description` (чётко укажите сценарии срабатывания; небольшая «проактивность» повышает частоту срабатывания).
4. Разбейте основной текст по темам в `references/`, добавьте в `SKILL.md` оглавление и шпаргалку с часто используемыми элементами, а также придерживайтесь **прогрессивного раскрытия** (держите `SKILL.md` компактным, подгружайте детали по требованию).
5. Обновите таблицу «Каталог навыков» в этом README и напишите README для конкретной книги `skills/your-book-name/README.md`.
6. Поместите исходную электронную книгу в `docs/` и убедитесь, что она исключена через `.gitignore`.

Вы можете использовать официальный навык `skill-creator` для составления, валидации и оценки новых навыков.

## Лицензия и атрибуция

Содержимое навыков здесь **адаптировано из соответствующих книг**; авторские права принадлежат оригинальным авторам/издателям и предоставляются для обучения и инженерного справочного использования. Не коммитьте защищённые авторским правом исходные электронные книги (`.gitignore` уже исключает `*.pdf`).
