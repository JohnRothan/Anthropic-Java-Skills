<p align="center">
  <img src="assets/readme.png" alt="Java Books to Claude Skills" width="720">
</p>

<p align="center">
  <a href="README.md">English</a> ·
  <a href="README.zh-CN.md">简体中文</a> ·
  <b>日本語</b> ·
  <a href="README.ko.md">한국어</a> ·
  <a href="README.es.md">Español</a> ·
  <a href="README.fr.md">Français</a> ·
  <a href="README.de.md">Deutsch</a> ·
  <a href="README.pt-BR.md">Português</a> ·
  <a href="README.ru.md">Русский</a>
</p>

# Anthropic Java Skills

> Java の書籍から抽出した **Agent Skills** のカタログで、Claude / Claude Code ですぐに利用できます。すべてのスキルは **中国語 (zh)** と **英語 (en)** の両方で提供されます。

## リポジトリ構成

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

規約:

- **1 冊の書籍につき 1 つのフォルダ** とし、kebab-case で命名して `skills/` の下に配置します。
- 各書籍には固定の `zh/` と `en/` のサブフォルダがあり、**それぞれが完結した独立したスキル** です（独自の `SKILL.md` と `references/` を持ちます）。
- 書籍の `zh` と `en` は同じ `name`（フロントマターの `name` フィールド）を共有するため、名前の衝突を避けるために、ある書籍については **1 つの skills ディレクトリにつき 1 言語のみをインストール** してください。
- ソースの PDF / 電子書籍は `docs/` の下に置き、`.gitignore` によって除外します — **著作権で保護された素材はコミットしないでください**。

## スキルカタログ

| Book | Folder | Languages | Description |
|------|--------|-----------|-------------|
| アリババ Java 開発マニュアル（黄山版） | [`skills/alibaba-java-coding-guidelines`](skills/alibaba-java-coding-guidelines) | zh · en | アリババ公式の Java コーディング規約：プログラミング、例外とロギング、ユニットテスト、セキュリティ、MySQL、プロジェクト構成、設計。規約に従って Java コードを記述・レビューするために使用します。 |

> 書籍は今後さらに追加予定です。

## インストールと使い方

これらのスキルは、このリポジトリに置かれているだけでは自動的に読み込まれ **ません** — これは **カタログ** です。Claude Code で 1 つを有効にするには、利用したい **単一の言語** を skills ディレクトリにインストールします。

**オプション 1: コピー**

```bash
# user-level (available everywhere)
cp -r skills/alibaba-java-coding-guidelines/en ~/.claude/skills/alibaba-java-coding-guidelines

# or project-level (one project only)
cp -r skills/alibaba-java-coding-guidelines/zh /path/to/your/project/.claude/skills/alibaba-java-coding-guidelines
```

**オプション 2: シンボリックリンク（このリポジトリと同期を保ちます）**

```bash
ln -s "$(pwd)/skills/alibaba-java-coding-guidelines/en" ~/.claude/skills/alibaba-java-coding-guidelines
```

インストール後、Java コードを記述またはレビューする際に、Claude は（その `description` に従って）自動的にスキルを参照します。明示的にトリガーすることもできます。言語を切り替えるには、インストール済みのコピーを削除してから、もう一方をインストールしてください。

## 新しい書籍の追加

1. `skills/` の下に `skills/your-book-name/`（kebab-case）を作成します。
2. `zh/` と `en/` のサブフォルダを追加し、それぞれに `SKILL.md` と `references/` を用意します。
3. `SKILL.md` にはフロントマターが必要です：`name`（書籍の両言語で同一にする）と `description`（トリガーとなるシナリオを明確に記述します。少し「積極的」にするとトリガー率が向上します）。
4. 本文をトピックごとに `references/` へ分割し、`SKILL.md` には索引と高頻度のチートシートを設け、**progressive disclosure（段階的開示）** に従います（`SKILL.md` は簡潔に保ち、詳細は必要に応じて読み込みます）。
5. この README の「スキルカタログ」テーブルを更新し、書籍ごとの `skills/your-book-name/README.md` を作成します。
6. ソースの電子書籍を `docs/` の下に置き、`.gitignore` によって確実に除外されるようにします。

公式の `skill-creator` スキルを使って、新しいスキルの草案作成、検証、評価を行うことができます。

## ライセンスと帰属

ここにあるスキルの内容は **それぞれの書籍を基に翻案したもの** であり、著作権は原著者／出版社に帰属し、学習およびエンジニアリングの参考用として提供されています。著作権で保護されたソースの電子書籍はコミットしないでください（`.gitignore` ですでに `*.pdf` を除外しています）。
