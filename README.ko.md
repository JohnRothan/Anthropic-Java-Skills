<p align="center">
  <img src="assets/readme.png" alt="Java Books to Claude Skills" width="720">
</p>

<p align="center">
  <a href="README.md">English</a> ·
  <a href="README.zh-CN.md">简体中文</a> ·
  <a href="README.ja.md">日本語</a> ·
  <b>한국어</b> ·
  <a href="README.es.md">Español</a> ·
  <a href="README.fr.md">Français</a> ·
  <a href="README.de.md">Deutsch</a> ·
  <a href="README.pt-BR.md">Português</a> ·
  <a href="README.ru.md">Русский</a>
</p>

# Anthropic Java Skills

> Java 서적에서 정제한 **Agent Skills** 카탈로그로, Claude / Claude Code에서 바로 사용할 수 있습니다. 모든 스킬은 **중국어(zh)** 와 **영어(en)** 두 가지로 제공됩니다.

## 저장소 구성

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

규칙:

- **한 권의 책에 하나의 폴더**, kebab-case로 이름을 지어 `skills/` 아래에 둡니다.
- 각 책은 고정된 `zh/`와 `en/` 하위 폴더를 가지며, **각각이 완전한 독립 스킬**입니다(고유의 `SKILL.md`와 `references/`를 포함).
- 한 책의 `zh`와 `en`은 동일한 `name`(frontmatter의 `name` 필드)을 공유하므로, 이름 충돌을 피하기 위해 특정 책에 대해서는 **하나의 스킬 디렉터리에 한 가지 언어만 설치**하세요.
- 원본 PDF / 전자책은 `docs/` 아래에 두고 `.gitignore`로 제외합니다 — **저작권이 있는 자료를 커밋하지 마세요**.

## 스킬 카탈로그

| 책 | 폴더 | 언어 | 설명 |
|------|--------|-----------|-------------|
| 알리바바 Java 개발 매뉴얼 (황산판) | [`skills/alibaba-java-coding-guidelines`](skills/alibaba-java-coding-guidelines) | zh · en | 알리바바의 공식 Java 코딩 표준: 프로그래밍, 예외 및 로깅, 단위 테스트, 보안, MySQL, 프로젝트 구조, 설계. 해당 표준에 따라 Java 코드를 작성하고 리뷰하는 데 사용합니다. |

> 더 많은 책이 추가될 예정입니다.

## 설치 및 사용 방법

이 스킬들은 이 저장소에 존재한다는 이유만으로 **자동으로 로드되지 않습니다** — 이것은 **카탈로그**입니다. Claude Code에서 스킬을 활성화하려면 원하는 **단일 언어**를 스킬 디렉터리에 설치하세요.

**옵션 1: 복사**

```bash
# user-level (available everywhere)
cp -r skills/alibaba-java-coding-guidelines/en ~/.claude/skills/alibaba-java-coding-guidelines

# or project-level (one project only)
cp -r skills/alibaba-java-coding-guidelines/zh /path/to/your/project/.claude/skills/alibaba-java-coding-guidelines
```

**옵션 2: 심볼릭 링크 (이 저장소와 동기화 유지)**

```bash
ln -s "$(pwd)/skills/alibaba-java-coding-guidelines/en" ~/.claude/skills/alibaba-java-coding-guidelines
```

설치 후에는 Java 코드를 작성하거나 리뷰할 때 Claude가 (해당 스킬의 `description`에 따라) 자동으로 스킬을 참조하며, 명시적으로 트리거할 수도 있습니다. 언어를 전환하려면 설치된 사본을 제거하고 다른 언어를 설치하세요.

## 새 책 추가하기

1. `skills/` 아래에 `skills/your-book-name/`(kebab-case)을 만듭니다.
2. `zh/`와 `en/` 하위 폴더를 추가하고, 각각에 `SKILL.md` + `references/`를 넣습니다.
3. `SKILL.md`에는 frontmatter가 필요합니다: `name`(한 책의 두 언어에서 동일) 및 `description`(트리거 시나리오를 명확하게 기술하며, 약간 "적극적(proactive)"으로 작성하면 트리거 비율이 높아집니다).
4. 본문을 주제별로 나누어 `references/`에 넣고, `SKILL.md`에 색인과 고빈도 치트 시트를 두며, **점진적 공개(progressive disclosure)** 원칙을 따릅니다(`SKILL.md`는 간결하게 유지하고, 세부 내용은 필요할 때 로드).
5. 이 README의 "스킬 카탈로그" 표를 업데이트하고 책별 `skills/your-book-name/README.md`를 작성합니다.
6. 원본 전자책을 `docs/` 아래에 두고 `.gitignore`로 제외되는지 확인합니다.

새 스킬을 초안 작성, 검증, 평가하는 데 공식 `skill-creator` 스킬을 사용할 수 있습니다.

## 라이선스 및 출처 표기

여기의 스킬 콘텐츠는 **각 서적에서 각색한 것**이며, 저작권은 원저자/출판사에 있고 학습 및 엔지니어링 참고용으로 제공됩니다. 저작권이 있는 원본 전자책을 커밋하지 마세요(`.gitignore`가 이미 `*.pdf`를 제외하고 있습니다).
