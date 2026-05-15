# openspec-superpowers-schema

An [OpenSpec](https://github.com/Fission-AI/OpenSpec) schema that leverages artifact governance (the **what**) and [Superpowers](https://github.com/obra/superpowers) execution skills (the **how**) in a single workflow. It also adds an evidence-first `retrospective` artifact filling a gap Superpowers does not natively cover.

Inspired by [JiangWay's superpowers-bridge](https://github.com/JiangWay/openspec-schemas). This is a simplified fork: English-only, generalized for any subagent-capable platform (not just Claude Code), and routed through AGENTS.md instead of CLAUDE.md.

## What problem does this solve?

OpenSpec governs **what to do** (artifact lifecycle: proposal / specs / tasks / verify / sync & archive, etc.). Superpowers governs **how to do it** (execution discipline: brainstorming, writing-plans, TDD, code review). Each is solid on its own; interleaving them in real development surfaces three structural problems:

1. **Output duplication** — brainstorming writes design output to `docs/superpowers/specs/`; OpenSpec re-authors `proposal.md` / `design.md` in the change directory, with overlapping content.
2. **Task fragmentation** — OpenSpec's `tasks.md` (coarse checkboxes) and Superpowers' `plan.md` (TDD micro-steps) describe the same work in different formats, locations, and progress trackers.
3. **Manual orchestration** — the user has to decide on every step which skill to invoke; the two systems do not connect on their own.

This schema mitigates those issues by orchestrating the explicit calls of Superpowers' skills where needed, including guardrails to avoid orphan artifacts.

### Why a custom schema rather than modifying existing skills?

Editing the `opsx` skill files directly is invasive and fragile because skills are overwritten on OpenSpec upgrades.

A custom schema uses OpenSpec's **native project-level schema mechanism**: the CLI validates structure, `openspec schemas` lists it automatically, each change picks its schema independently (`--schema spec-driven` or `--schema openspec-superpowers`), and no existing SKILL.md or command file is modified.

## Installation

### Requirements

- Subagent-capable platform (Claude Code, OpenCode, Codex, etc.). This schema does not support runtimes without subagent support because the alternative executor (executing-plans) loses TDD and code-review transitive activation, defeating Superpowers' value. If your platform lacks subagent support, use OpenSpec's default spec-driven schema instead.
- [Superpowers](https://github.com/obra/superpowers) plugin installed.
- [OpenSpec](https://github.com/Fission-AI/OpenSpec) installed and configured in your project with all commands (not only core) available: `openspec init` and `openspec config profile`

### Install with one-shot prompt

```
Install the openspec-superpowers schema for OpenSpec into this project:

1. Verify that the project has an `openspec/` directory (if missing, ask me to configure OpenSpec in this project  with `openspec init` and to enable all commands with `openspec config profile`).
2. Verify that the following Superpowers skills are available: brainstorming, writing-plans, using-git-worktrees, subagent-driven-development, finishing-a-development-branch.
3. Clone https://github.com/javimb/openspec-superpowers-schema to a temp directory.
4. Copy the `openspec/schemas/openspec-superpowers/` from the cloned repository to `openspec/schemas/openspec-superpowers/` in the current project.
5. Run `openspec schema validate openspec-superpowers` to verify.
6. Run `openspec schemas` and confirm `openspec-superpowers` is listed.
7. Edit `openspec/config.yaml` to use `schema: openspec-superpowers`
8. If AGENTS.md (or equivalent) exists, append `openspec/schemas/openspec-superpowers/templates/AGENTS.fragment.md` as a new section. Otherwise, ask me to create it based on the fragment.
9. Clean up the temp directory.
10. Show me the final state.
```

### Upgrade

If your project already has `openspec/schemas/openspec-superpowers/` and you want to upgrade to the latest version of the schema, use the following one-shot prompt:

```
Upgrade the openspec-superpowers schema for OpenSpec in this project:

1. Verify `openspec/schemas/openspec-superpowers/` already exists (this is an upgrade, not fresh install). If missing, abort and tell me to use the install instructions instead.
2. Verify that the following Superpowers skills are available: brainstorming, writing-plans, using-git-worktrees, subagent-driven-development, finishing-a-development-branch.
3. Clone https://github.com/javimb/openspec-superpowers-schema to a temp directory.
4. Show me the diff between `openspec/schemas/openspec-superpowers/` from this project and `openspec/schemas/openspec-superpowers/` from the cloned repository (use `diff -ruN`). Wait for my ack before overwriting.
5. After my ack, overwrite `openspec/schemas/openspec-superpowers/` in this project with `openspec/schemas/openspec-superpowers/` from the cloned repository.
6. Run `openspec schema validate openspec-superpowers` to verify.
7. If AGENTS.md (or equivalent) contains a workflow-routing section referencing openspec-superpowers, show me the diff between that section and the content of `openspec/schemas/openspec-superpowers/templates/AGENTS.fragment.md` from the cloned repo and follow the same ack pattern from steps 4 and 5. Otherwise, append the fragment as a new section.
8. Clean up the temp directory.
9. Show me the final state.
```

## Workflow

```text
brainstorm ──→ proposal ──→ specs ──→ tasks ──→ plan ──→ apply ──→ verify ──→ retrospective ──→ PR
           │               ↑
           └─→ design ─────┘
```

### Usage

#### Step-by-step flow (recommended)

```bash
/opsx:new my-feature # → scaffolds new change
/opsx:continue       # → brainstorm (interactive dialogue)
/opsx:continue       # → proposal
/opsx:continue       # → design
/opsx:continue       # → specs
/opsx:continue       # → tasks
/opsx:continue       # → plan
/opsx:apply          # → implementation: worktree + subagent-driven-development (TDD + code-review)
/opsx:verify         # → evidence-based verification steps
/opsx:continue       # → retrospective
/opsx:archive        # → syncs specs, archives change and creates PR
```

#### Quick flow

```bash
/opsx:ff my-feature  # → one-shot: scaffold + brainstorm + proposal + design + specs + tasks + plan
/opsx:apply          # → implementation: worktree + subagent-driven-development (TDD + code-review)
/opsx:verify         # → evidence-based verification steps
/opsx:continue       # → retrospective
/opsx:archive        # syncs specs, archives change and creates PR
```

### Differences from OpenSpec's default `spec-driven` schema:


|             | spec-driven                                | openspec-superpowers                                                                                                                   |
| ----------- | ------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| Entry point | Generates proposal.md                      | Generates **brainstorm.md** (invoking Superpowers' brainstorming skill)                                                                |
| Plan        | Generates tasks.md (coarse)                | Generates tasks.md + **plan.md** (TDD micro-steps)                                                                                     |
| Apply       | Standard task-by-task in a single context. | Using Superpowers' **worktree + subagent-driven-development** (with TDD + code-review transitive + fresh context for each task) skills |
| Post-apply  | None                                       | Generates **verify.md** + **retrospective.md**, creates PR                                                                             |


### Superpowers touchpoints


| Superpowers skill                            | Where it's invoked                  |
| -------------------------------------------- | ----------------------------------- |
| `superpowers:brainstorming`                  | `brainstorm.md` artifact generation |
| `superpowers:writing-plans`                  | `plan.md` artifact generation       |
| `superpowers:using-git-worktrees`            | Apply's set up step                 |
| `superpowers:subagent-driven-development`    | Apply's implementation step         |
| `superpowers:test-driven-development`        | Apply's implementation step         |
| `superpowers:requesting-code-review`         | Apply's implementation step         |
| `superpowers:finishing-a-development-branch` | After archive step is completed     |


### Output redirection

Superpowers skills have default output paths (for example, brainstorming writes to `docs/superpowers/specs/`). This schema's artifact instructions **override** that behavior by injecting context that redirects output into the change directory:

- brainstorming → `openspec/changes/<name>/brainstorm.md` + `openspec/changes/<name>/design.md`
- writing-plans → `openspec/changes/<name>/plan.md`

## Entry gates

This schema's instructions only fire when invoked through `/opsx:*` commands. If you trigger Superpowers skills via narrative — for example, by saying "let's discuss the architecture" — the default behavior bypasses the schema. Brainstorming will still write to `docs/superpowers/specs/`, defeating the integration's redirection.

This section covers two topics:

1. When you don't need to enter the schema at all (just open a PR)
2. When verbal brainstorming should be promoted to an `opsx` change

### When NOT to enter the schema (direct PR)

Not every change needs an OpenSpec change. Some scenarios should skip `opsx` entirely:


| Scenario                                                  | Needs an opsx change?                                             | What to do                      |
| --------------------------------------------------------- | ----------------------------------------------------------------- | ------------------------------- |
| New feature / new capability                              | ✅ Yes                                                             | `/opsx:new <name>`              |
| Breaking change                                           | ✅ Yes                                                             | `/opsx:new <name>`              |
| Architecture change                                       | ✅ Yes                                                             | `/opsx:new <name>`              |
| Build tooling tweak (linter rule, coverage threshold)     | ⚠️ It depends on the level of detail in your tooling specs.       | `/opsx:new <name>` or direct PR |
| Documentation update / typo fix                           | ⚠️ It depends on the level of detail in your documentation specs. | `/opsx:new <name>` or direct PR |
| Test backfill / coverage                                  | ⚠️ It depends on the level of detail in your tests specs.         | `/opsx:new <name>` or direct PR |
| Bug fix (restoring intended behavior, no contract change) | ❌ No                                                              | Direct PR                       |
| Non-breaking dependency upgrade                           | ❌ No                                                              | Direct PR                       |
| Config value tweak (no structural change)                 | ❌ No                                                              | Direct PR                       |


### When verbal brainstorming should be promoted to a change

If `superpowers:brainstorming` was triggered via narrative ("let's brainstorm the architecture") in a project that uses this schema, the brainstorming output **MUST NOT** land in `docs/superpowers/specs/` — that bypasses the schema's output redirection and creates orphan artifacts.

The correct flow: keep brainstorming verbally until all 5 conditions below hold, then promote to `/opsx:propose` or `/opsx:new` so the agreed design lands in `openspec/changes/<name>/brainstorm.md`.

1. **Scope locked** — one sentence describes what's in / out, and the scope doesn't keep growing each turn
2. **Major design forks resolved** — alternatives have been weighed and one chosen; remaining unknowns are **explicit TBDs** (with owner and impact-scope statement), not "haven't thought about it yet"
3. **Cross-system dependencies mapped** — for each dependency: ready / mockable / genuinely unknown — pick one
4. **Acceptance criteria stateable** — concrete pass conditions (e.g., `./mvnw clean verify` passes + specific deliverables)
5. **Conversation converging** — the last 1-2 turns are confirmations, not new "what about..." forks

If any condition is missing, keep brainstorming. When all five are met:

- The model **should proactively suggest** "this looks ready for `/opsx:propose` — want to open a change?".
- The user **may also explicitly say** "open this as an opsx change".
- Either way, **promotion requires a deliberate human ack** — never automatic.

