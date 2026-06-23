<p align="center">
  <img src="https://cdn.jsdelivr.net/gh/JohnRothan/Anthropic-Java-Skills@main/assets/readme.webp" alt="Java Books to Claude Skills" width="720">
</p>

<p align="center">
  <a href="README.md">English</a> ·
  <a href="README.zh-CN.md">简体中文</a> ·
  <a href="README.ja.md">日本語</a> ·
  <a href="README.ko.md">한국어</a> ·
  <b>Español</b> ·
  <a href="README.fr.md">Français</a> ·
  <a href="README.de.md">Deutsch</a> ·
  <a href="README.pt-BR.md">Português</a> ·
  <a href="README.ru.md">Русский</a>
</p>

# Anthropic Java Skills

> Un catálogo de **Agent Skills** destilados a partir de libros de Java, listos para usar con Claude / Claude Code. Cada skill se distribuye tanto en **chino (zh)** como en **inglés (en)**.

## Estructura del repositorio

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

Convenciones:

- **Un libro, una carpeta**, nombrada en kebab-case, ubicada en `skills/`.
- Cada libro tiene subcarpetas fijas `zh/` y `en/`, **cada una un skill completo e independiente** (con su propio `SKILL.md` y `references/`).
- El `zh` y el `en` de un libro comparten el mismo `name` (el campo `name` del frontmatter), por lo que conviene **instalar solo un idioma por directorio de skills** para un libro dado, a fin de evitar colisiones de nombres.
- Los PDF / libros electrónicos de origen van en `docs/` y se excluyen mediante `.gitignore` — **no incluyas en el repositorio material con derechos de autor**.

## Catálogo de skills

| Libro | Carpeta | Idiomas | Descripción |
|------|--------|-----------|-------------|
| Manual de Desarrollo Java de Alibaba (Edición Huangshan) | [`skills/alibaba-java-coding-guidelines`](skills/alibaba-java-coding-guidelines) | zh · en | Estándares oficiales de codificación Java de Alibaba: programación, excepciones y registro (logging), pruebas unitarias, seguridad, MySQL, estructura del proyecto, diseño. Para escribir y revisar código Java conforme al estándar. |

> Más libros próximamente.

## Cómo instalar y usar

Estos skills **no** se cargan automáticamente solo por estar en este repositorio: es un **catálogo**. Para habilitar uno en
Claude Code, instala el **único idioma** que desees en un directorio de skills.

**Opción 1: Copiar**

```bash
# user-level (available everywhere)
cp -r skills/alibaba-java-coding-guidelines/en ~/.claude/skills/alibaba-java-coding-guidelines

# or project-level (one project only)
cp -r skills/alibaba-java-coding-guidelines/zh /path/to/your/project/.claude/skills/alibaba-java-coding-guidelines
```

**Opción 2: Enlace simbólico (se mantiene sincronizado con este repositorio)**

```bash
ln -s "$(pwd)/skills/alibaba-java-coding-guidelines/en" ~/.claude/skills/alibaba-java-coding-guidelines
```

Tras la instalación, Claude consulta el skill automáticamente cuando escribes o revisas código Java (según su
`description`), o puedes activarlo de forma explícita. Para cambiar de idioma, elimina la copia instalada e
instala la otra.

## Añadir un nuevo libro

1. Crea `skills/your-book-name/` (kebab-case) dentro de `skills/`.
2. Añade las subcarpetas `zh/` y `en/`, cada una con un `SKILL.md` + `references/`.
3. `SKILL.md` necesita frontmatter: `name` (idéntico en ambos idiomas de un libro) y `description` (indica claramente los escenarios de activación; ser un poco "proactivo" mejora la tasa de activación).
4. Divide el cuerpo por temas en `references/`, dale a `SKILL.md` un índice y una hoja de referencia rápida de alta frecuencia, y sigue la **divulgación progresiva** (mantén `SKILL.md` ligero, carga los detalles bajo demanda).
5. Actualiza la tabla del "Catálogo de skills" en este README y escribe un README por libro en `skills/your-book-name/README.md`.
6. Coloca el libro electrónico de origen en `docs/` y asegúrate de que esté excluido por `.gitignore`.

Puedes usar el skill oficial `skill-creator` para redactar, validar y evaluar nuevos skills.

## Licencia y atribución

El contenido de los skills aquí está **adaptado de los libros respectivos**; los derechos de autor pertenecen a los
autores/editores originales y se proporciona como referencia para el aprendizaje y la ingeniería. No incluyas en el repositorio
libros electrónicos de origen con derechos de autor (el `.gitignore` ya excluye `*.pdf`).
