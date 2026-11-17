# ADR 0001: Adopt Conventional Commits

- **Status:** Accepted
- **Date:** 2026-11-15
- **Deciders:** Hunar Roop Kahlon
- **Technical Story:** Establish a consistent commit message convention for DiHelios.

## Context and Problem Statement

The project needs a clear, consistent commit message format to make history readable, support merge-only integration, and enable future automation (e.g. changelogs, validation) without ambiguity. Decisions around commit structure, types, scopes, breaking changes, and issue references must be explicit.

## Decision

Adopt the **Conventional Commits** specification with the project-specific rules below. Every commit intended for the shared history MUST follow this convention.

### 1. Header format

- **Structure:** `<type>(<scope>)<bang>: <subject>`
  - `<type>`: required; one of the allowed types.
  - `(<scope>)`: optional; included only when a scope is provided.
  - `<bang>`: optional `!` to signal a breaking change.
  - `<subject>`: required; a single-line description.
- **Examples:** `feat(api): add session export`, `fix: handle empty input`, `refactor!: drop legacy config`

### 2. Allowed types

| Type | Use for |
| --- | --- |
| `feat` | New user-facing feature or capability |
| `fix` | Bug fix (user-facing or correctness) |
| `docs` | Documentation only |
| `style` | Formatting/whitespace/style-only changes (no logic/meaning change) |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `perf` | Performance improvement |
| `test` | Adding or correcting tests |
| `build` | Build system or external dependencies (e.g. tooling, packaging) |
| `ci` | CI configuration/scripts |
| `chore` | Other maintenance tasks that don’t fit above (no src/test behavior change) |
| `revert` | Reverts a previous commit |

### 3. Scope

- **Optional.** Omitted when not useful.
- **Form:** short, lowercase identifier for the affected area when used; prefer `kebab-case` (e.g. `api`, `ui`, `auth`, `docs`, `deps`, `ci`).
- **Vocabulary:** free-form for now (no fixed allowed-scope list yet); a canonical list may be introduced later.
- **Multiplicity:** single scope per commit. Support for multiple scopes is deferred until a concrete need arises.

### 4. Subject and body

- **Subject:**
  - Imperative, present-tense mood (e.g. “add”, “fix”, “remove”).
  - No trailing period.
  - Concise; recommended maximum of 72 characters for the full header line when practical.
  - Lowercase first word is acceptable unless a proper noun/acronym requires capitalization (not strictly enforced).
- **Body (optional):**
  - Separated from the header by a blank line.
  - Explain **what and why**, not low-level how, when non-obvious.
  - Wrap lines at ~72–100 characters for readability.
  - Bullet points may be used where helpful.

### 5. Footers, breaking changes, and issue references

- **Footers:** optional; separated from the body (or header if no body) by a blank line. Use git trailer style (`Token: value` or `Token #value`) where appropriate.
- **Breaking changes:** indicate via either or both:
  - `!` in the header (e.g. `feat!: ...` or `feat(api)!: ...`).
  - A footer `BREAKING CHANGE: <description>` describing impact/migration (recommended when details are needed).
- **Issue references (GitHub):**
  - **Non-closing reference:** `Refs #123` (or `Refs owner/repo#123` for cross-repo).
  - **Closing reference:** repeat the closing keyword for each issue, e.g. `Closes #1, Closes #2`.
    - A single keyword with a comma-separated list (e.g. `Closes #1, #2`) is **invalid**.
  - Standard keywords: `Refs` for linking, `Closes` for closing (capitalized by convention).
- **Other trailers:** optional as needed (e.g. `Co-authored-by: Name <email>`, `Signed-off-by:`).

### 6. Enforcement and alignment

- **Commit-level discipline:** Every commit intended for the shared history MUST follow this convention (not PR titles alone), to keep history readable under a merge-only strategy.
- **Merge-only integration:** Pull requests are merged via **merge commits only** (no squash, no rebase merges). Do not rely on squash-generated titles. Exact merge-commit titling conventions may be defined in a subsequent decision.
- **Stacked pull requests:** Each PR in a stack corresponds to one or more conventional commits; commit messages are the source of truth for change intent. Stacking mechanics/tooling and local VCS workflow are individual choices and out of scope of this ADR.
- **Automated enforcement:** Optional; local hooks or CI checks (e.g. commitlint) may be introduced later. No enforcement tooling is mandated by this decision.

## Consequences

### Positive

- Consistent, scannable commit history across contributors.
- Clear signaling of breaking changes and issue linkage.
- Compatible with merge-only history and future automation (changelog/release tooling, linting).

### Negative / Risks

- Requires contributor discipline until/unless automated checks are added.

### Neutral / Deferred

- Canonical scope list, multiple scopes per commit, and automated enforcement are deferred to future decisions.
