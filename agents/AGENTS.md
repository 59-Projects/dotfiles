# Jeremy's global instructions for coding agents

These apply in every project. It's mainly for coding, but some parts (like [Writing](#writing-docs-readmes-comments-messages)) apply more broadly. Instructions from the user or a project's own configuration/documentation (including `AGENTS.md`, `CLAUDE.md`, etc.) take precedence where they conflict.

This file lives in `~/dotfiles/agents/AGENTS.md` and is symlinked into each agent's config directory (e.g. `~/.claude/CLAUDE.md`), so edit it there. For a preference that would be saved into project/agent memory, ask if it should be saved in this file instead.

Huge thanks to **Aidan Feldman** for the inspiration — his post [Documenting My Coding Style](https://blog.afeld.me/documenting-my-coding-style-d852c6a75963) is what prompted this file, and it started as a direct copy of [his own AGENTS.md](https://github.com/afeld/dotfiles/blob/main/agents/AGENTS.md), modified from there to actually reflect how I work.

## Commits

- Only commit when asked — don't commit automatically just because a change is finished and verified.
  - Push only when asked, separately from committing.
- When committing, make focused commits: one per logical change (a feature, then its error handling, then tests), rather than one big commit at the end.
  - Refactors get their own commits, separate from behavior changes.
- Keep diffs minimal.
  - Leave code where it is and keep existing names, unless the change calls for moving or renaming.
  - Don't bundle unrelated fixes; put them in their own commits.
- Subject line: imperative, sentence case, no trailing period, no length limit.
  - Say what changes for the user or the code's behavior, not which files were touched.
  - Match the casing/format of a repo's existing history.
- Add a body when the why isn't obvious from the subject, which is most of the time for anything non-trivial. Explain the problem it solves, why this approach over the alternatives, and any consequence a reviewer wouldn't guess.
  - Use concrete numbers when you have them.
  - Skip the body for self-explanatory changes like dependency updates.
- Keep tool-generated directories out of Git.
- Don't add a `Co-Authored-By: Claude` (or similar) attribution line to commit messages or PR descriptions.

## Working style

- Before any action that's destructive, hard to reverse, or visible to others (force-push, deleting branches/data, posting somewhere, pushing to shared infra) — check first, even if a similar action was approved earlier in the session. One approval doesn't cover every future instance.
- Verify changes by actually running them (tests, the app, the build) before saying they work, and say what you couldn't verify.
- Investigate and explain before changing something load-bearing (data pipelines, ingestion logic, anything touching money or stored data) — I want to understand it first, not just see the fix.
- When something doesn't work the way the data/evidence suggests, say so and revise — don't defend the first theory.
- Prefer a clear simplification, and mention it, rather than preserving complexity out of caution.

## Writing (docs, READMEs, comments, messages, UI copy)

- Plain, direct, and concise.
  - Prefer short declarative sentences.
  - Put the reason in a clause ("rather than…", "so that…", "since…") instead of a separate paragraph.
  - Skip filler, hype, and recaps of what was just done.
- No em dashes. Use a comma, period, colon, or parentheses instead.
- No "this, not that" / "it's not X, it's Y" staccato contrast pairs for dramatic effect (e.g. "That isn't a coincidence. It's the point."). Write it as one sentence instead.
- Comments explain why, not what.
  - Cover the constraint, the surprising behavior, the version that introduced something, or the unit.
  - Don't comment what the code already says.
- Docstrings are a single line when they can be.
- Be consistent in terminology within a project, and fix the term everywhere when you rename it.
- READMEs lead with setup as a numbered list with copy-pasteable commands.
- Write for a reader who will maintain this later — explain how the pieces fit together when it isn't obvious from the code.

### Markdown

- Number every item of an ordered list `1.`.
- Tag fenced code blocks (`sh`, `python`, etc.).
- Prefer nested lists to long bullets: a short parent point, with the details, examples, and exceptions as sub-points.
- No horizontal rules (`---`/`<hr>`). Use a heading to separate sections instead.

## Code

- Use opinionated formatters/linters that auto-fix, and run them before committing.
- Keep functions and modules small and single-purpose.
  - Remove unused code and dependencies as you notice them.
  - DRY up repetition, including in CI workflows.
- Prefer existing solutions to custom code: a well-known library over hand-rolled code, a framework's built-in feature over a custom one.
- Don't add abstractions, error handling, or validation for scenarios that can't happen. No premature generalization for hypothetical future requirements.
- Always set explicit timeouts on HTTP requests.
- Keep dependency version specifications minimal; explain any non-obvious floor in a comment.

### Automated testing

- Keep tests small and readable.
- Avoid having tests hit the network or require API keys.

## User interfaces

- Provide a good experience on mobile: large enough tap targets, works on narrow/short screens, loads fast.
- Be accessible.

## Being polite to other people's servers

Free/shared APIs and data sources are shared resources:

- Identify the app in the `User-Agent` where practical.
- Keep concurrency low and space requests out; avoid bursts.
- Back off on rate limits (429/403) and stop sending to a server that has limited you.
- Cache results rather than refetching.

## Open questions

Not yet decided, don't assume either way — settle these as this file evolves:

- Python tooling conventions (uv, Ruff, type-checking) — afeld's original had a dedicated section; add back once there's an actual Python project to calibrate against.
- CI/CD conventions — same, add once there's a project with CI to generalize from.
- Branching/rebase workflow for multi-person repos — afeld's original specified GitHub Flow + rebase-before-push; not yet confirmed this is how Jeremy wants shared repos handled.
