# AGENTS.md

## Repository

OpenSpec schema definition that brings [Superpowers](https://github.com/obra/superpowers) skills into [OpenSpec](https://github.com/Fission-AI/OpenSpec)'s spec-driven flow.

The entire schema is `openspec/schemas/openspec-superpowers/schema.yaml` and its templates.

## Schema

### Structure

- `openspec/schemas/openspec-superpowers/schema.yaml` — single source of truth; defines artifacts, templates, instructions, and dependency chain
- `openspec/schemas/openspec-superpowers/templates/` — scaffold files for each artifact type (proposal.md, spec.md, design.md, tasks.md)

### Validation

The schema should be validated using OpenSpec's CLI:

```bash
openspec schema validate openspec-superpowers
```

## Git workflow

Always use [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/).

