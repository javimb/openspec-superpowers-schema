<!--
Delta spec template for a change.

This template demonstrates 4 delta section types; use only what you need:
- ADDED / MODIFIED / REMOVED / RENAMED
File name & location: openspec/changes/<change-name>/specs/<capability>/spec.md
(`<capability>` must match the openspec/specs/<capability>/ directory name)

Formatting hard rules (OpenSpec will validate):
- Requirement sentences MUST contain `SHALL` or `MUST`
- Each Requirement MUST have at least one `#### Scenario:`
- Scenario MUST use level-4 (`####`); level-3 or bullet will silently fail
-->

## ADDED Requirements

<!-- New behavior. List new Requirements this change will add to the capability. -->

### Requirement: <!-- requirement name -->
<!-- requirement description — must contain SHALL or MUST -->

#### Scenario: <!-- scenario name -->
- **WHEN** <!-- condition -->
- **THEN** <!-- expected outcome -->

---

## MODIFIED Requirements

<!--
Modify an existing Requirement. **MUST use the exact same normalized header as in openspec/specs/<capability>/spec.md** (trim then case-sensitive match), otherwise the archive's delta apply will fail because it can't find the corresponding requirement.

**MUST paste the full modified content** (not just a diff), because OpenSpec archive applies MODIFIED by full-text replacement.
-->

### Requirement: <!-- same header as existing spec -->
<!-- full modified requirement description — must contain SHALL or MUST -->

#### Scenario: <!-- scenario name (may be new or modified) -->
- **WHEN** <!-- condition -->
- **THEN** <!-- expected outcome -->

---

## REMOVED Requirements

<!--
Remove an existing Requirement. MUST include Reason and Migration notes so reviewers understand why it's being retired and how existing consumers should migrate.
-->

### Requirement: <!-- header to remove, exactly as in existing spec -->

**Reason**: <!-- why it's being retired -->

**Migration**: <!-- how existing consumers/dependents should adapt -->

---

## RENAMED Requirements

<!--
Rename a Requirement header. Format is fixed: FROM / TO using code-fence headers.
If the name changes AND the content changes, list the rename here AND write the full content under MODIFIED using the **new** header.

Archive apply order: RENAMED → REMOVED → MODIFIED → ADDED
-->

- FROM: `### Requirement: <Old Name>`
- TO: `### Requirement: <New Name>`
