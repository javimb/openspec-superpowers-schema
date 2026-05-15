# openspec-superpowers-schema

Inspired by [JiangWay's superpowers-bridge](https://github.com/JiangWay/openspec-schemas).

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
7. If AGENTS.md (or equivalent) contains a workflow-routing section referencing openspec-superpowers, show me de diff between that section and the content of `openspec/schemas/openspec-superpowers/templates/AGENTS.fragment.md` from the cloned repo and follow the same ack pattern from steps 4 and 5. Otherwise, append the fragment as a new section.
8. Clean up the temp directory.
9. Show me the final state.
```
