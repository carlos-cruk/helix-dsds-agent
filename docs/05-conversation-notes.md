# Helix DSDS Agent
## Conversation Notes and Design Decision History

**Version:** 1.0  
**Purpose:** Preserve all key decisions, rationale, assumptions and future considerations from the discovery and planning process for the Helix DSDS Agent project.

---

# Project Background

The Cancer Research UK design system team requires a GitHub Copilot Agent that can generate Design System Documentation Specification (DSDS) documentation from the Helix design system.

The primary objective is to reduce the manual effort required to produce and maintain DSDS-compliant documentation.

Helix should remain the authoritative source of truth.

DSDS YAML files should be treated as generated and synchronized artefacts.

---

# Initial Objective

Create an AI-powered workflow that:

1. Accepts a component name.
2. Finds the corresponding documentation within Helix.
3. Extracts relevant design system guidance.
4. Transforms guidance into DSDS-compliant YAML.
5. Stores documentation in a reusable file format.
6. Supports future updates and synchronization.

---

# Key Design Decisions

## Decision: Use DSDS as the Documentation Format

### Reasoning

The Design System Documentation Specification (DSDS) is:

- YAML-based
- Machine-readable
- Portable
- Vendor-independent
- Validatable

The team wants generated documentation to follow the DSDS specification.

### Outcome

All generated component documentation must:

- Conform to DSDS v0.23.1
- Be valid YAML
- Be structured according to DSDS entry types and sections

---

## Decision: Helix is the Source of Truth

### Reasoning

The Helix design system is actively maintained.

Guidance changes regularly.

Duplicating content manually would create maintenance overhead and documentation drift.

### Outcome

Helix is always the authoritative source.

The agent should:

- Generate documentation from Helix
- Update documentation from Helix
- Never assume local YAML files are the source of truth

---

## Decision: Use Live Data Rather Than Cached Exports

### Requirement

The design system team regularly updates Helix documentation.

### Outcome

The agent must:

- Query Helix when generating documentation
- Query Helix when updating documentation
- Use live content whenever possible

### Benefits

- Documentation remains current
- No manual exports
- Reduced maintenance effort

---

# Platform Decision

## GitHub Copilot Agent for VS Code

### Alternatives Considered

- Copilot Skill
- Microsoft 365 Copilot Skill
- Prompt-only solution
- Local scripts

### Chosen Solution

GitHub Copilot Agent inside VS Code.

### Reasoning

Provides:

- Workspace awareness
- File creation and modification capabilities
- Local repository integration
- Support for slash commands
- Better developer workflow

---

# Helix Platform Decision

## Helix Technology

Helix is built using Zeroheight.

### Implications

Zeroheight design systems typically contain:

- Structured navigation
- Consistent documentation layouts
- Static URLs
- Reusable content patterns
- Component-centric information architecture

### Benefits for the Agent

Allows predictable extraction patterns.

Enables:

- Component discovery
- Section mapping
- Consistent synchronization

---

# Core Agent Capabilities

The project evolved into a broader documentation management solution rather than a simple documentation generator.

---

## Capability 1: Create Component Documentation

### Command

```text
/create-component button
```

### Workflow

```text
Locate Helix Documentation
→ Extract Guidance
→ Transform Content
→ Generate DSDS YAML
→ Save Component File
```

### Output Example

```text
components/button.dsds.yaml
```

---

## Capability 2: Update Existing Documentation

### Command

```text
/update-component button
```

### Workflow

```text
Read Existing YAML
→ Query Helix
→ Compare
→ Update
→ Record Changes
```

### Purpose

Maintain synchronization over time.

---

## Capability 3: Validation

### Command

```text
/validate-component button
```

### Responsibilities

Check:

- YAML validity
- DSDS compliance
- Required sections
- Source references

---

## Capability 4: Audit

### Command

```text
/audit-component button
```

### Purpose

Identify missing documentation.

### Example

```yaml
coverage:
  description: complete
  anatomy: complete
  accessibility: partial
```

---

## Capability 5: Bulk Updates

### Command

```text
/update-all
```

### Purpose

Synchronize every registered component.

### Long-Term Benefit

Allows repository-wide maintenance.

---

# Changelog Discussions

A significant requirement introduced during planning was automatic change tracking.

---

## Requirement

When component guidance changes in Helix:

- The update must be recorded
- The affected documentation area must be identified
- The date must be preserved
- Historical entries must not be overwritten

---

## Initial Idea

Simple textual changelog entries.

Example:

```yaml
- date: 2026-11-06
  notes: Accessibility guidance updated
```

---

## Improved Design

Use machine-readable changelog entries.

### Example

```yaml
custom:
  changelog:
    - date: 2026-11-06

      added:
        - section: Variants
          item: Secondary Destructive

      modified:
        - section: Accessibility
          item: Keyboard navigation guidance

      removed:
        - section: Guidelines
          item: Legacy hierarchy guidance
```

### Benefits

Enables:

- Reporting
- Analytics
- Automated release notes
- Future tooling

---

# DSDS Mapping Strategy

A deterministic mapping strategy was agreed.

Consistency between components is critical.

---

## Mapping Table

| Helix Content | DSDS Output |
|---------------|------------|
| Overview | Description |
| Usage Guidance | Guidelines |
| Best Practice | Guidelines |
| Do / Don't | Guidelines |
| Anatomy | Definitions |
| Variants | Definitions |
| States | Definitions |
| Accessibility | Definitions |
| Content Guidance | Guidelines |
| Behaviour | Guidelines |
| Related Components | Metadata |

---

# Required Documentation Areas

The agent should attempt to extract:

- Description
- Purpose
- Usage guidance
- Do guidance
- Don't guidance
- Accessibility guidance
- Anatomy definitions
- Variant definitions
- State definitions
- Behaviour guidance
- Content guidance
- Related components

---

# Metadata Decisions

Every generated component should contain:

```yaml
metadata:
```

fields.

---

## Required Metadata

### Source URLs

```yaml
source:
```

Provides traceability.

---

### Last Reviewed

```yaml
lastReviewed:
```

Documents review activity.

---

### Last Synced

```yaml
lastSynced:
```

Documents synchronization activity.

---

### Status

```yaml
status:
```

Helps consumers understand maturity.

---

# Update Engine Design

A major discussion focused on avoiding noisy updates.

---

## Problem

Documentation formatting often changes without changing guidance.

---

## Solution

Ignore:

- Whitespace changes
- Formatting changes
- Heading-level changes

---

## Record Only

Meaningful content changes.

Examples:

### Record

```text
New accessibility requirement
New variant
Removed guidance
Updated content guidance
```

### Ignore

```text
Bold text
Italic text
Whitespace
Heading level changes
```

---

# Repository Structure Decision

A structured repository was proposed.

```text
helix-dsds-agent/

.github/
commands/
prompts/
templates/
scripts/
registry/
components/
reports/
tests/
```

---

# Registry Discussion

A component registry was introduced to support bulk operations.

---

## Example

```yaml
components:

  - button
  - card
  - accordion
  - modal
```

---

## Benefit

Supports:

```text
/update-all
```

without needing manual component discovery.

---

# Reporting Requirements

The update process should generate synchronization reports.

### Example

```yaml
date: 2026-11-06

summary:
  checked: 52
  updated: 4
  unchanged: 48
```

---

# Future Automation

Several future capabilities were identified.

---

## Phase 2

### Bulk Synchronization

Repository-wide updates.

### Coverage Reports

Documentation quality metrics.

### GitHub Actions

Scheduled synchronization workflows.

---

## Phase 3

### Automated Pull Requests

Agent opens update PRs.

### Nightly Synchronization

Automated refreshes.

### Release Reports

Generate change summaries automatically.

---

## Phase 4

### Tokens

Support:

```yaml
kind: token
```

### Themes

Support:

```yaml
kind: theme
```

### Patterns

Support:

```yaml
kind: pattern
```

### Design Tool Integrations

Potential future integrations:

- Figma
- Storybook
- Design Tokens workflows

---

# Copilot Instructions Guidance

Repository-level Copilot instructions should guide implementation.

---

## Recommended Behaviour

```markdown
Helix is the source of truth.

Always generate DSDS 0.23.1 compliant YAML.

Always include source URLs.

Preserve changelog history.

Validate documentation before saving.

Record only meaningful documentation changes.

Preserve manual metadata entries.
```

---

# Final Project Vision

The project evolved from:

```text
Generate YAML from a component page
```

Into:

```text
A full documentation lifecycle management system
```

Capabilities include:

- Generation
- Synchronization
- Validation
- Auditing
- Reporting
- Changelog management
- Bulk updates
- Future automation

The long-term objective is a GitHub Copilot Agent that can continuously maintain DSDS documentation in sync with Helix while minimizing manual effort and preserving complete documentation history.