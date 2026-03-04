# Agents Guide (post-for-me-skills)

This repo is a small collection of agent “skills” (markdown documents) for Post For Me.
It currently contains no application code, package manager config, CI, or test runner.

## Repo Layout

- `skills/<skill-name>/SKILL.md`: A single skill document.
- `README.md`: Minimal repo intro.

## Build / Lint / Test

No build, lint, or test tooling is configured in this repository.

### What you can run today

- Basic hygiene
  - `git status`
  - `git diff`
  - `git diff --check` (whitespace/errors)

### Optional checks (only if you already have them installed)

These are not declared in this repo (no `package.json`, `pyproject.toml`, etc.).

- Markdown formatting
  - `prettier --check .` (if you use Prettier)
  - `markdownlint "**/*.md"` (if you use markdownlint)
- Spellcheck
  - `cspell "**/*.md"` (if you use cspell)

### Running a single test

There are no tests in this repo.

If tests are added later, prefer documenting the canonical single-test invocations here.
Common patterns (only if your future tooling matches):

- `pytest -k <pattern> -q`
- `vitest -t <name> path/to/file.test.ts`
- `jest path/to/file.test.ts -t "<name>"`
- `go test ./... -run <TestName>`

## Cursor / Copilot Rules

- Cursor: no `.cursor/rules/` and no `.cursorrules` found.
- Copilot: no `.github/copilot-instructions.md` found.

If those files are added later, keep their guidance in sync with this document.

## Skill Document Conventions

### File naming

- Skill directories are kebab-case: `skills/post-for-me-sdk/`.
- Skill file name is fixed: `SKILL.md`.

### Frontmatter

Skills use YAML frontmatter at the top of `SKILL.md`:

```yaml
---
name: <skill-id>
description: <one-line description>
---
```

- `name` should match the directory name (kebab-case) unless there is a strong reason.
- `description` is a single line; keep it action-oriented.

### Markdown style

- Use ATX headings (`#`, `##`, `###`), starting with a single `#` title.
- Prefer short sections with scannable bullet lists.
- Wrap code samples in fenced code blocks with an info string (` ```typescript `, ` ```bash `).
- Keep lines reasonably short (aim ~100 chars) but don’t aggressively hard-wrap code.
- Use backticks for literal identifiers (file names, property keys, commands, IDs).

## TypeScript Example Style (for code blocks)

This repo includes TypeScript examples inside markdown; keep them consistent.

### Formatting

- Indentation: 2 spaces.
- Strings: double quotes.
- Statements: include semicolons.
- Use trailing commas in multiline objects/arrays.
- Prefer `async/await` over `.then()` for readability.

### Imports

- Prefer explicit named imports over namespace imports when showing examples.
- Group imports in this order when relevant:
  1. Built-ins (rare in SDK examples)
  2. Third-party
  3. Local
- Avoid unused imports in snippets.

### Types

- Prefer letting TypeScript infer types when obvious.
- Add explicit types at API boundaries (function args/returns) when it clarifies intent.
- When referencing API payloads, match the SDK’s types and naming (even if keys are
  `snake_case` in request objects).

### Naming

- Variables: `camelCase`, descriptive over short.
- Constants: `camelCase` unless truly constant config, then `SCREAMING_SNAKE_CASE`.
- Functions: verbs (`createPost`, `listAccounts`), avoid ambiguous names (`handle`).
- IDs in examples: use realistic placeholders (`sa_abc123`, `sp_xyz789`).

## API/SDK Payload Guidelines (for docs)

- Match the documented request/response shapes exactly; don’t “clean up” key casing.
- Prefer complete minimal examples that are copy/paste runnable.
- When there are platform-specific options, nest under `platform_configurations` and
  show only the relevant platform keys.

## Error Handling Guidance (for examples)

- Prefer showing the “happy path” plus one minimal error example.
- When demonstrating errors, use `try/catch` and print a useful message.
- Don’t swallow errors silently; either rethrow or return a typed error object.
- If you mention retries/backoff, clarify that it is optional and context-dependent.

## Content Quality Checklist

- Does the skill explain the goal before the API calls?
- Are required fields called out (and optional fields labeled as such)?
- Are time formats explicit (e.g., ISO 8601 for timestamps)?
- Are code blocks typed (`typescript`) and consistent with existing snippets?
- Are claims verifiable (avoid undocumented features)?

## Git / PR Hygiene (lightweight)

- Keep diffs focused: one skill change per PR when possible.
- Don’t introduce secrets (API keys, tokens) in docs or examples.
- Prefer small commits with messages that explain why the docs changed.
