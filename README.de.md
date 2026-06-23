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
  <b>Deutsch</b> ·
  <a href="README.pt-BR.md">Português</a> ·
  <a href="README.ru.md">Русский</a>
</p>

# Anthropic Java Skills

> Ein Katalog von **Agent Skills**, destilliert aus Java-Büchern und einsatzbereit für Claude / Claude Code. Jeder Skill wird sowohl auf **Chinesisch (zh)** als auch auf **Englisch (en)** ausgeliefert.

## Repository-Aufbau

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

Konventionen:

- **Ein Buch, ein Ordner**, benannt in kebab-case und abgelegt unter `skills/`.
- Jedes Buch hat feste Unterordner `zh/` und `en/`, **jeweils ein vollständiger, eigenständiger Skill** (mit eigener `SKILL.md` und `references/`).
- Die `zh`- und `en`-Varianten eines Buchs teilen sich denselben `name` (das Feld `name` im Frontmatter), installieren Sie daher pro Buch **nur eine Sprache pro Skills-Verzeichnis**, um Namenskollisionen zu vermeiden.
- Quell-PDFs / E-Books gehören unter `docs/` und werden über `.gitignore` ausgeschlossen — **committen Sie kein urheberrechtlich geschütztes Material**.

## Skill-Katalog

| Buch | Folder | Languages | Beschreibung |
|------|--------|-----------|-------------|
| Alibaba Java-Entwicklungshandbuch (Huangshan-Ausgabe) | [`skills/alibaba-java-coding-guidelines`](skills/alibaba-java-coding-guidelines) | zh · en | Alibabas offizielle Java-Coding-Standards: Programmierung, Exception- & Logging-Handhabung, Unit-Tests, Sicherheit, MySQL, Projektstruktur, Design. Zum Schreiben und Überprüfen von Java-Code gemäß dem Standard. |

> Weitere Bücher folgen.

## Installation & Nutzung

Diese Skills werden **nicht** automatisch geladen, nur weil sie in diesem Repo liegen — es handelt sich um einen **Katalog**. Um einen Skill in Claude Code zu aktivieren, installieren Sie die **einzelne Sprache**, die Sie möchten, in ein Skills-Verzeichnis.

**Option 1: Kopieren**

```bash
# user-level (available everywhere)
cp -r skills/alibaba-java-coding-guidelines/en ~/.claude/skills/alibaba-java-coding-guidelines

# or project-level (one project only)
cp -r skills/alibaba-java-coding-guidelines/zh /path/to/your/project/.claude/skills/alibaba-java-coding-guidelines
```

**Option 2: Symlink (bleibt mit diesem Repo synchron)**

```bash
ln -s "$(pwd)/skills/alibaba-java-coding-guidelines/en" ~/.claude/skills/alibaba-java-coding-guidelines
```

Nach der Installation zieht Claude den Skill automatisch heran, wenn Sie Java-Code schreiben oder überprüfen (gemäß seiner `description`), oder Sie können ihn explizit auslösen. Um die Sprache zu wechseln, entfernen Sie die installierte Kopie und installieren Sie die andere.

## Ein neues Buch hinzufügen

1. Erstellen Sie `skills/your-book-name/` (kebab-case) unter `skills/`.
2. Fügen Sie die Unterordner `zh/` und `en/` hinzu, jeweils mit einer `SKILL.md` + `references/`.
3. `SKILL.md` benötigt Frontmatter: `name` (identisch über beide Sprachen eines Buchs hinweg) und `description` (geben Sie die Auslöseszenarien klar an; ein wenig „proaktiv“ zu sein verbessert die Auslöserate).
4. Teilen Sie den Hauptteil thematisch in `references/` auf, geben Sie der `SKILL.md` ein Inhaltsverzeichnis und ein Cheat Sheet mit hoher Nutzungsfrequenz und folgen Sie der **progressiven Offenlegung** (halten Sie `SKILL.md` schlank, laden Sie Details bei Bedarf).
5. Aktualisieren Sie die Tabelle „Skill-Katalog“ in dieser README und schreiben Sie eine buchspezifische `skills/your-book-name/README.md`.
6. Legen Sie das Quell-E-Book unter `docs/` ab und stellen Sie sicher, dass es durch `.gitignore` ausgeschlossen wird.

Sie können den offiziellen `skill-creator`-Skill verwenden, um neue Skills zu entwerfen, zu validieren und zu evaluieren.

## Lizenz & Quellenangabe

Die hier enthaltenen Skill-Inhalte sind **aus den jeweiligen Büchern adaptiert**; das Urheberrecht liegt bei den ursprünglichen Autoren/Verlagen und wird zu Lern- und Engineering-Referenzzwecken bereitgestellt. Committen Sie keine urheberrechtlich geschützten Quell-E-Books (die `.gitignore` schließt `*.pdf` bereits aus).
