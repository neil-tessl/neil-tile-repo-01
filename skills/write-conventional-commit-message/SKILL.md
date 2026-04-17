---
name: write-conventional-commit-message
description: |
  Use this when the user asks you to write, suggest, or improve a git commit message based on a diff or staged changes. Triggered by phrases like "write a commit message", "commit this", "what should the commit message be", or after the user runs git diff and asks for a message.
---

# Write Conventional Commit Message

Write commit messages that follow the [Conventional Commits](https://www.conventionalcommits.org/) specification. A good message lets a future reader understand *why* a change happened without reading the diff.

## Structure

```
<type>(<scope>): <subject>

<body>

<footer>
```

- **Subject line** is mandatory. Body and footer are optional but usually present for non-trivial changes.
- **Blank line** between subject, body, and footer.

## Type

Pick exactly one:

| Type | Use when |
|---|---|
| `feat` | Adding new user-facing functionality |
| `fix` | Fixing a bug |
| `chore` | Maintenance that doesn't change behavior (deps, tooling) |
| `docs` | Documentation-only change |
| `refactor` | Restructuring code without changing behavior |
| `test` | Adding or fixing tests |
| `perf` | Performance improvement |
| `build` | Build system or external dependency changes |
| `ci` | CI/CD configuration |
| `style` | Formatting, whitespace — no code change |
| `revert` | Reverting a previous commit |

If a change fits multiple types, pick the one a reader would find most load-bearing. `feat` and `fix` take priority over `refactor` when both apply.

## Scope

Optional parenthetical that narrows where the change lives. Good scopes: `auth`, `api`, `cli`, `db`, `ui`, or a module/package name (`parser`, `billing`). Skip the scope if the change is cross-cutting or if a scope would be noise.

## Subject

- **Imperative mood**, present tense: "add", "fix", "remove" — not "added" or "adds".
- **Under 50 characters** when possible, hard cap 72.
- **No trailing period.**
- **Lowercase** after the type prefix unless the first word is a proper noun.
- Describe **what** changed, briefly. The body is for why.

## Body

- Wrap at **72 characters** per line.
- Explain **why** the change was made and what problem it solves. Reference incidents, tickets, or constraints.
- Don't restate what the diff already shows. If a reader can see it from the diff, don't put it here.
- Use bullet points for lists of related changes.

## Footer

- `BREAKING CHANGE: <description>` for any breaking change. Required when the change is breaking, even if the type is `feat` or `fix`.
- `Closes #123`, `Refs #456` for issue tracking.
- `Co-authored-by: Name <email>` for pair/mob work.

## Rules of thumb

- One logical change per commit. If the diff spans unrelated changes, suggest splitting it before writing a single message.
- If you can't summarize the change in under 50 characters, the commit is probably doing too much.
- Don't mention file paths in the subject — they belong in the body if at all.
- Never write "misc fixes", "updates", "changes", or "wip" as a subject.

## Examples

### Good

```
fix(auth): extend JWT expiry to match session length

Users were being logged out mid-session because the JWT TTL (15m)
was shorter than the session cookie TTL (24h). Align JWT TTL with
session TTL and add a background refresh 2m before expiry.

Closes #412
```

```
feat(cli): add --dry-run flag to publish command

Lets users preview what would be published without touching the
registry. Useful during PR review and for CI gates.
```

```
refactor(parser): extract token normalization into its own module
```

### Bad

```
fixed bug                              ← not imperative, no type, no detail
feat: Updated the thing.               ← vague, trailing period, "updated" not "update"
fix(auth): fixed the JWT expiry bug because users were being logged out mid-session
                                       ← runs past 72 chars, restates itself
chore: wip                             ← never commit wip messages
```

## When to ask for clarification

Ask the user before writing the message if:
- The diff contains clearly unrelated changes (suggest splitting).
- The change is breaking but the user hasn't flagged it — confirm before adding `BREAKING CHANGE`.
- A linked ticket is referenced in the branch name but not in the diff — ask whether to include `Closes #N`.

Otherwise, just write the message.
