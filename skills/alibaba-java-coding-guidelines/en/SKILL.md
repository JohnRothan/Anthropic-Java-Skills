---
name: alibaba-java-coding-guidelines
description: >-
  Alibaba Java Development Manual (Huangshan Edition) coding standards. Use this
  skill whenever you write, review, refactor, or evaluate any Java code — covering
  naming conventions, constant definitions, code formatting, OOP rules, date/time,
  collection handling, concurrency and multithreading, control statements, comments,
  exception handling, logging, unit testing, security, MySQL table/index/SQL/ORM design,
  and application-layering project structure. Whether or not the user explicitly mentions
  "Alibaba spec / P3C / Huangshan / development manual", proactively apply the rules and
  severity levels here for any task that produces or vets Java code (e.g. "write a Java
  class / Service / DAO", "review this Java", "is this code compliant?", "create a MySQL
  table", "help me name this", "handle exceptions and logging").
---

# Alibaba Java Development Manual (Huangshan Edition) — Coding Standards

This skill turns Alibaba's official *Java Development Manual (Huangshan Edition)* into actionable
guidance for writing and reviewing Java code. It spans seven dimensions: programming specification,
exception & logging, unit testing, security, MySQL database, project structure, and design. The full
text is split by dimension under `references/`. **Use the high-frequency cheat sheet below to cover
common cases, and read the matching reference file when you need the details of a specific dimension.**

## Severity levels (always reflect these in your output)

Every rule carries a severity tag. When resolving conflicts or giving advice, prioritize by:

- **[Mandatory]** — must be followed unconditionally. Violations seed failures, security holes, or
  maintainability debt. Never violate when writing; **always** flag as a blocker when reviewing.
- **[Recommended]** — strongly advised. Adopt unless there is a solid reason not to; raise as an
  improvement suggestion in review.
- **[Reference]** — a good practice for reference; adopt as appropriate for your team.

## How to use

### When writing Java code
1. Before coding, skim the "High-frequency mandatory rules" cheat sheet below so you don't trip on
   naming, POJOs, null-safety, collections, concurrency, SQL, and other common pitfalls.
2. When a task centers on one dimension (e.g. writing a DAO/table → MySQL; a concurrency utility →
   Concurrency), **read the matching `references/` file** before writing, to get the details right.
3. After producing code, self-check: are any [Mandatory] rules violated? If you deviate from a
   [Recommended] rule for readability, state the reason.

### When reviewing / evaluating Java code
1. Check against the relevant dimension's rules and classify findings by severity:
   **[Mandatory] violation = must fix**, **[Recommended] violation = suggested improvement**.
2. Cite the rule source for each comment where possible (e.g. "Naming rule 9 [Mandatory]: POJO
   boolean fields take no `is` prefix") so the author can understand and verify it.
3. Pair each problem with a **positive-example** fix — don't just say "non-compliant".

## High-frequency mandatory rules (covers the most common cases without opening a reference file)

**Naming**
- Class names `UpperCamelCase`; methods/params/members/locals `lowerCamelCase`; constants `UPPER_SNAKE_CASE`.
- No mixing Pinyin with English, no Chinese names; no leading/trailing `_` or `$`; no sloppy abbreviations.
- POJO boolean fields take **no `is` prefix** (use `deleted`, not `isDeleted`), or some frameworks fail serialization.
- Abstract classes start with `Abstract`/`Base`; exception classes end with `Exception`; test classes end with `Test`.
- Package names all lowercase and singular; Service/DAO interface implementations use the `Impl` suffix.

**Constants & types**
- No magic values inline; `long` literals use uppercase `L` (e.g. `2L`).
- POJO fields and RPC params/returns must use **wrapper types**; locals use primitives.
- Compare Integer wrapper values with `equals`; compare `BigDecimal` with `compareTo()`; never `new BigDecimal(double)` — use `new BigDecimal("0.1")` or `BigDecimal.valueOf()`.
- Don't compare floats directly with `==`. Store monetary amounts as integers in the smallest currency unit.
- POJOs set no default field values; must implement `toString()`; overrides must carry `@Override`.

**Collections**
- The result of `Arrays.asList()` cannot be add/remove'd; check emptiness with `isEmpty()`, not `size()==0`.
- Use `Collection.toArray(new T[0])`; don't remove/add inside a `foreach` (use `Iterator` or concurrent containers).
- If you override `equals`, also override `hashCode`; iterate with `Map.entrySet`, not keySet with a second lookup.
- Converting a collection to a `Map`: a null value causes NPE; `Collectors.toMap` throws on duplicate keys.

**Concurrency**
- Thread pools **must not be created via `Executors`**; use `new ThreadPoolExecutor(...)` with an explicit queue and rejection policy to avoid OOM.
- Threads/thread pools must be named (`ThreadFactory`); `SimpleDateFormat` is not thread-safe — use `DateTimeFormatter`.
- Lock in a consistent order to avoid deadlock; use atomics/locks for concurrent updates; `remove()` `ThreadLocal` after use.

**Control statements / OOP**
- Each `switch` `case` must `break`/`return` or comment the fall-through; must have a `default`.
- `if/else/for/while` always use braces, even for a single line; avoid nesting deeper than 3 levels (guard clauses / state pattern).
- Call `equals` from a constant or known-non-null object (`"x".equals(param)`).

**Exceptions & logging**
- Don't catch `RuntimeException` (e.g. `NullPointerException`/`IndexOutOfBounds`) in place of a pre-check; check beforehand.
- Don't wrap large unrelated blocks in `try-catch`; don't merely `printStackTrace` or swallow in catch; release resources in `finally` (or try-with-resources).
- Log via the SLF4J facade with placeholders `logger.info("id={}", id)`, not string concatenation; log context and stack trace for exceptions.

**MySQL (when writing DDL/SQL/entities)**
- Tables require `id` (bigint unsigned, auto-increment PK), `gmt_create`, `gmt_modified`; table/column names lowercase snake_case, no reserved words.
- Yes/no fields named `is_xxx` (unsigned tinyint, 1 yes / 0 no).
- Use `decimal` for fractional types; never `float`/`double` for money; `varchar` over 5000 → use `text` in a separate table.
- Use parameter binding to prevent injection; `count(*)` for row counts; see the reference for pagination, indexing, left-wildcard `like` defeating indexes, etc.

> The cheat sheet is a high-frequency reminder, not the whole standard. For specific details, edge
> cases, and positive/counter examples, read the matching reference file below.

## Reference index (read on demand)

| Dimension | File | When to read |
|-----------|------|--------------|
| Programming | `references/01-programming.md` | Naming, constants, formatting, OOP, date/time, collections, concurrency, control statements, comments, front/back-end, misc — the main reference for writing or reviewing any Java code |
| Exception & Logging | `references/02-exception-and-logging.md` | Designing error codes, exception-handling strategy, logging conventions |
| Unit Testing | `references/03-unit-testing.md` | Writing or reviewing unit tests (AIR / BCDE principles, etc.) |
| Security | `references/04-security.md` | User input, authorization, SQL injection, XSS/CSRF, file upload, masking, sensitive data |
| MySQL Database | `references/05-mysql-database.md` | Table creation, index design, SQL statements, ORM/MyBatis mapping |
| Project Structure | `references/06-project-structure.md` | Application layering (DO/DTO/VO, Manager layer), second-party dependencies, server config |
| Design | `references/07-design.md` | Architecture- and design-level conventions |
| Glossary | `references/08-glossary.md` | Look up terms like POJO/DO/DTO/CAS/IDE when you need to confirm a meaning |

Each reference file keeps the official rules' severity tags and positive/counter examples, so you can
cite "rule X [Mandatory/Recommended/Reference]" directly.
