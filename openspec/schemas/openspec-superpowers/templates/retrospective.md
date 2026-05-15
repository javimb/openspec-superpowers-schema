# Retrospective: <change-name>

> Written: <YYYY-MM-DD> (after verify passed)
> Commit range: `<base-sha>..<head-sha>`
> Worktree: <path or "merged to main">

---

## 0. Evidence

> Quantitative baseline data — subsequent Wins / Misses bullets reference these, avoiding repeated [evidence: ...] per line.
> For cold-write scenarios (retro written well after the cycle ends), using only `git log` + `tasks.md` + commit messages should still allow reconstructing this section.

- **Commit range**: `<base-sha>..<head-sha>` (<n> commits)
- **Diff size**: <+X / -Y lines across N files>
- **Tasks done**: <x>/<y> (`grep -cE '^\s*- \[x\]' tasks.md` → x; regex tolerates sub-task indentation)
- **Active hours**: <estimate>
- **Subagent dispatches**: <count or "n/a">
- **New external dependencies**: <list, with license + version, or "none">
- **Bugs encountered post-merge**: <count, one-line each, or "none">
- **OpenSpec validate state at archive**: <pass / fail / not-run>
- **Test coverage signal**: <e.g. jacoco %, pytest count, vitest count, or "n/a">

Commit chain (chronological):

```
<base-sha> <one-line summary>
...
<head-sha> <archive commit one-line>
```

---

## 1. Wins

- [evidence: <commit/file/test>] <description>

## 2. Misses

- 🔴 [blocking | evidence: ...] <description>
- 🟡 [painful  | evidence: ...] <description>
- 📌 [nit      | evidence: ...] <description>

## 3. Plan deviations

| Plan task | What changed | Why |
|-----------|--------------|-----|
| 1.2       | ...          | ... |

## 4. Skill / workflow compliance

| Skill                                            | Used |
|--------------------------------------------------|------|
| superpowers:brainstorming                        |      |
| superpowers:writing-plans                        |      |
| superpowers:using-git-worktrees                  |      |
| superpowers:subagent-driven-development          |      |
| (transitive) superpowers:test-driven-development |      |
| (transitive) superpowers:requesting-code-review  |      |
| superpowers:finishing-a-development-branch       |      |

> **Default expectation**: all ✓. Every skill is part of the schema design; skipping one is an exceptional situation. Any ✗ must include a reason and prevention plan in the `### Deliberately Skipped Skills` subsection below.

### Deliberately Skipped Skills

> Skipping a skill is a designed escape hatch, not the normal path. Each ✗ must answer three questions; an empty section (all green) is the expected state.

- **`<skill name>`**
  - **What was skipped**: <specifically, was the entire skill skipped or just a sub-step>
  - **Why this cycle**: <concrete cycle condition — vague reasons like "not needed" / "too small" / "no time" / "blocked by external dep" / "skill output looked wrong" are not acceptable; state the actual trigger (specific commit / log line / observed behavior)>
  - **How to prevent recurrence**: How to avoid skipping under similar conditions next cycle? Choose one:
    - `schema graph fix` — specify which section of schema.yaml to change
    - `skill description tightening` — specify which skill's frontmatter / instruction to change
    - `AGENTS.md trigger` — specify which judgment rule to add to AGENTS.md.fragment
    - `scope-judgment rule` — specify how this cycle's scope should have been judged
    - `one-off — schema boundary case, no prevention possible` — must explicitly explain why it's a boundary (vague reservations not accepted)

> **Relationship to §6 Promote candidates**: if multiple cycles share the same `How to prevent` answer for the same skill → that pattern should be promoted to §6, directly triggering a schema / skill PR; it must not accumulate as "normal".

## 5. Surprises

- <assumption that turned out wrong>

## 6. Promote candidates → long-term learning

Each candidate uses a `- [ ]` checklist:

- Title: severity emoji (🔴/🟡/📌) + one-sentence learning
- `→ **Promote to** <destination>` (memory / AGENTS.md / schema / skill / one-off)
- Two-line body (matching the superpowers feedback memory body schema):
  - `> **Why**: <reason; often a past incident or strong preference>`
  - `> **How to apply**: <when/where this guidance kicks in>`

Unchecked `- [ ]` items mean the candidate has not yet been promoted — they can be carried into the next cycle's retro for re-evaluation, or kept as cross-cycle observation points.

> **Carry-forward mechanism**: when writing the next cycle's retro, you can
> `grep -A 5 '^- \[ \]' openspec/changes/archive/*/retrospective.md` to extract
> past unchecked candidates, then judge each one: carry-forward into this cycle's §6,
> promote in-place, or mark stale and stop tracking.

Example:

- [ ] 🔴 **<short rule>** → **Promote to memory** (type: feedback)
  > **Why**: <past incident or strong preference that motivated this rule>
  > **How to apply**: <which file / cycle phase / decision moment this kicks in>

- [ ] 🟡 **<another candidate>** → **Promote to project AGENTS.md** (`<path/to/AGENTS.md>` section)
  > **Why**: ...
  > **How to apply**: ...

- [ ] 📌 **<third candidate>** → **One-off** (record only, no promotion)
  > **Why**: <why it doesn't generalize>