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
  <b>Português</b> ·
  <a href="README.ru.md">Русский</a>
</p>

# Anthropic Java Skills

> Um catálogo de **Agent Skills** destilado de livros de Java, pronto para uso com o Claude / Claude Code. Cada skill é disponibilizada tanto em **chinês (zh)** quanto em **inglês (en)**.

## Estrutura do repositório

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

Convenções:

- **Um livro, uma pasta**, nomeada em kebab-case, colocada em `skills/`.
- Cada livro tem subpastas fixas `zh/` e `en/`, **cada uma uma skill completa e independente** (com seu próprio `SKILL.md` e `references/`).
- O `zh` e o `en` de um livro compartilham o mesmo `name` (o campo `name` no frontmatter), portanto **instale apenas um idioma por diretório de skills** para um determinado livro, a fim de evitar colisões de nomes.
- PDFs / e-books de origem vão para `docs/` e são excluídos via `.gitignore` — **não faça commit de material protegido por direitos autorais**.

## Catálogo de skills

| Livro | Pasta | Idiomas | Descrição |
|------|--------|-----------|-------------|
| Manual de Desenvolvimento Java da Alibaba (Edição Huangshan) | [`skills/alibaba-java-coding-guidelines`](skills/alibaba-java-coding-guidelines) | zh · en | Os padrões oficiais de codificação Java da Alibaba: programação, exceções e logging, testes unitários, segurança, MySQL, estrutura de projeto, design. Para escrever e revisar código Java conforme o padrão. |

> Mais livros em breve.

## Como instalar e usar

Estas skills **não** são carregadas automaticamente apenas por estarem neste repositório — ele é um **catálogo**. Para habilitar uma no
Claude Code, instale o **único idioma** desejado em um diretório de skills.

**Opção 1: Copiar**

```bash
# user-level (available everywhere)
cp -r skills/alibaba-java-coding-guidelines/en ~/.claude/skills/alibaba-java-coding-guidelines

# or project-level (one project only)
cp -r skills/alibaba-java-coding-guidelines/zh /path/to/your/project/.claude/skills/alibaba-java-coding-guidelines
```

**Opção 2: Symlink (mantém-se sincronizado com este repositório)**

```bash
ln -s "$(pwd)/skills/alibaba-java-coding-guidelines/en" ~/.claude/skills/alibaba-java-coding-guidelines
```

Após a instalação, o Claude consulta a skill automaticamente quando você escreve ou revisa código Java (conforme sua
`description`), ou você pode acioná-la explicitamente. Para trocar de idioma, remova a cópia instalada e
instale a outra.

## Adicionando um novo livro

1. Crie `skills/your-book-name/` (kebab-case) em `skills/`.
2. Adicione as subpastas `zh/` e `en/`, cada uma com um `SKILL.md` + `references/`.
3. O `SKILL.md` precisa de frontmatter: `name` (idêntico em ambos os idiomas de um livro) e `description` (declare claramente os cenários de acionamento; ser um pouco "proativo" melhora a taxa de acionamento).
4. Divida o corpo por tópico em `references/`, dê ao `SKILL.md` um índice e uma folha de consulta rápida de alta frequência, e siga a **divulgação progressiva** (mantenha o `SKILL.md` enxuto, carregue os detalhes sob demanda).
5. Atualize a tabela "Catálogo de skills" neste README e escreva um `skills/your-book-name/README.md` por livro.
6. Coloque o e-book de origem em `docs/` e certifique-se de que ele esteja excluído pelo `.gitignore`.

Você pode usar a skill oficial `skill-creator` para esboçar, validar e avaliar novas skills.

## Licença e atribuição

O conteúdo das skills aqui é **adaptado dos respectivos livros**; os direitos autorais pertencem aos
autores/editoras originais e são fornecidos para fins de aprendizado e referência de engenharia. Não faça commit de e-books de origem
protegidos por direitos autorais (o `.gitignore` já exclui `*.pdf`).
