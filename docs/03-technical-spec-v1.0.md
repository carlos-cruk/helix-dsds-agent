# Helix DSDS Agent for GitHub Copilot (VS Code)

**Version:** 1.1  
**Status:** Production Specification  
**Owner:** Cancer Research UK Design System Team  
**Source of Truth:** Helix Design System (Zeroheight)  
**Documentation Format:** DSDS v0.23.1  
**Target Platform:** GitHub Copilot in VS Code

---

# 1. Executive Summary

The Helix DSDS Agent is a GitHub Copilot Agent designed to generate, update, validate, audit, and synchronize Design System Documentation Specification (DSDS) component documentation using Helix as the authoritative source of truth.

The agent enables designers, developers, and design system maintainers to:

- Create DSDS documentation from Helix component pages.
- Synchronize existing DSDS files with Helix documentation.
- Generate machine-readable changelogs.
- Validate DSDS compliance.
- Audit documentation completeness.
- Execute bulk synchronization operations.

Helix remains the authoritative source. DSDS YAML files are maintained artifacts generated and updated from Helix.

---

# 2. Problem Statement

The Helix design system contains component guidance distributed across Zeroheight documentation.

Maintaining DSDS documentation manually creates several challenges:

- Duplication of documentation efforts.
- Out-of-date component documentation.
- Difficulty tracking documentation changes.
- Inconsistent structure between components.
- Limited auditability.

The Helix DSDS Agent solves these problems by introducing automated generation and synchronization workflows.

---

# 3. Objectives

## Primary Objectives

### Documentation Generation

Generate DSDS-compliant YAML documentation for individual components.

### Documentation Synchronization

Maintain parity between:

```text
Helix Documentation
        ↓
DSDS YAML
```

### Change Tracking

Record documentation changes automatically.

### Validation

Ensure documentation complies with DSDS standards.

### Coverage Auditing

Measure completeness and quality of component documentation.

---

# 4. Users

## Primary Users

### Design System Designers

Responsible for documenting design decisions.

### Design System Engineers

Responsible for implementation documentation.

### Product Designers

Consume and review component documentation.

### Design System Maintainers

Own documentation quality and synchronization.

---

# 5. Scope

## Included

### Component Documentation

Examples:

- Button
- Card
- Accordion
- Alert
- Badge
- Link
- Tabs
- Checkbox
- Radio
- Modal
- Dialog
- Breadcrumb
- Pagination

### Documentation Areas

- Overview
- Description
- Usage guidance
- Do / Don't guidance
- Accessibility
- Anatomy
- Variants
- States
- Content guidance
- Behaviour
- Related components

### Repository Operations

- Create
- Update
- Validate
- Audit
- Bulk update

---

## Excluded (Version 1)

### Design Tokens

Future Support:

```yaml
kind: token
```

### Themes

Future Support:

```yaml
kind: theme
```

### Design Patterns

Future Support:

```yaml
kind: pattern
```

---

# 6. Functional Requirements

## FR-001 Create Component Documentation

Generate DSDS documentation from Helix.

### Input

```text
/create-component button
```

### Output

```text
components/button.dsds.yaml
```

### Process

```text
Search Helix
→ Extract Documentation
→ Transform to DSDS
→ Generate YAML
```

---

## FR-002 Update Component Documentation

Update DSDS documentation using current Helix content.

### Input

```text
/update-component button
```

### Process

```text
Read Existing YAML
→ Load Helix Content
→ Compare
→ Update
→ Record Changes
```

### Output

Updated YAML document containing changelog entries.

---

## FR-003 Validate Documentation

Validate DSDS compliance.

### Input

```text
/validate-component button
```

### Output

```yaml
validation:
  yaml: pass
  schema: pass
  required_sections: pass
```

---

## FR-004 Audit Documentation

Generate completeness report.

### Input

```text
/audit-component button
```

### Output

```yaml
coverage:
  description: complete
  anatomy: complete
  accessibility: partial
```

---

## FR-005 Bulk Synchronization

Synchronize every registered component.

### Input

```text
/update-all
```

### Output

```yaml
summary:
  checked: 48
  updated: 5
  unchanged: 43
```

---

# 7. Agent Commands

## Create

```text
/create-component <name>
```

---

## Update

```text
/update-component <name>
```

---

## Validate

```text
/validate-component <name>
```

---

## Audit

```text
/audit-component <name>
```

---

## Update All

```text
/update-all
```

---

# 8. Repository Structure

```text
helix-dsds-agent/

├── .github
│   └── copilot-instructions.md
│
├── commands
│   ├── create-component.md
│   ├── update-component.md
│   ├── validate-component.md
│   ├── audit-component.md
│   └── update-all.md
│
├── prompts
│   ├── create-component.prompt.md
│   ├── update-component.prompt.md
│   ├── validate-component.prompt.md
│   └── audit-component.prompt.md
│
├── templates
│   └── component-template.dsds.yaml
│
├── scripts
│   ├── crawl_helix.py
│   ├── search_helix.py
│   ├── extract_component.py
│   ├── dsds_generator.py
│   ├── dsds_updater.py
│   ├── dsds_validator.py
│   ├── changelog_generator.py
│   └── audit.py
│
├── registry
│   └── components-index.yaml
│
├── components
│
├── reports
│
└── tests
```

---

# 9. Helix Content Model

The extraction layer normalizes documentation into a standard structure.

```python
class Component:
    name
    description
    usage
    do_guidance
    dont_guidance
    anatomy
    variants
    states
    accessibility
    content_guidance
    behaviour
    related_components
```

---

# 10. DSDS Mapping Rules

| Helix Section | DSDS Output |
|---------------|------------|
| Overview | description |
| When to use | guidelines |
| When not to use | guidelines |
| Do / Don't | guidelines |
| Anatomy | definitions |
| Variants | definitions |
| States | definitions |
| Accessibility | definitions |
| Content | guidelines |
| Behaviour | guidelines |
| Related components | metadata |

---

# 11. DSDS Output Specification

## Base Structure

```yaml
schemaVersion: "0.23.1"

name: CRUK Helix Design System

entries:
  - kind: component
```

---

## Component Structure

```yaml
entries:
  - kind: component

    id: button

    name: Button

    description: >
      Triggers an action.
```

---

## Metadata Structure

```yaml
metadata:

  status:
    status: stable

  tags:
    - action
    - interactive

  source:
    - https://helix.cancerresearchuk.org

  lastReviewed: 2026-11-06

  lastSynced: 2026-11-06
```

---

# 12. Required Sections

Every component must include:

## Guidelines

```yaml
kind: guidelines
```

---

## Anatomy

```yaml
kind: definitions
context: anatomy
```

---

## Variants

```yaml
kind: definitions
context: variants
```

---

## Accessibility

```yaml
kind: definitions
context: accessibility
```

---

## Content Guidance

```yaml
kind: guidelines
```

---

# 13. Changelog Specification

## Purpose

Track synchronization history.

## Location

```yaml
custom:
  changelog:
```

---

## Schema

```yaml
custom:

  changelog:

    - date: 2026-11-06

      source:
        - component-url

      added: []

      modified: []

      removed: []
```

---

## Example

```yaml
custom:

  changelog:

    - date: 2026-11-06

      source:
        - /components/button

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

---

# 14. Synchronization Engine

## Flow

```text
Load Component
↓
Load Existing YAML
↓
Fetch Helix Content
↓
Normalize Content
↓
Compare
↓
Generate Changes
↓
Update YAML
↓
Append Changelog
↓
Validate
↓
Save
```

---

# 15. Diff Strategy

The agent must detect meaningful documentation changes.

## Ignore

### Whitespace

Ignore:

```text
Button
 Button
```

---

### Formatting

Ignore:

```html
<strong>
<em>
```

changes.

---

### Heading Changes

Ignore:

```text
H2 → H3
```

when content remains unchanged.

---

## Detect

### New Guidance

```text
Accessibility rule added
```

### Removed Guidance

```text
Variant removed
```

### Updated Guidance

```text
Usage recommendation changed
```

---

# 16. Validation Rules

## Mandatory Fields

### schemaVersion

Required.

### name

Required.

### entries

Required.

### description

Required.

### metadata

Required.

### source

Required.

### sections

Required.

---

# 17. Audit Framework

## Coverage Categories

| Section | Weight |
|---------|---------|
| Description | 10 |
| Usage Guidance | 20 |
| Anatomy | 20 |
| Variants | 15 |
| Accessibility | 20 |
| Content Guidance | 15 |

Total:

```text
100 Points
```

---

## Example Audit Result

```yaml
audit:

  score: 92

  missing:
    - screen-reader-guidance

  recommendations:
    - expand accessibility support
```

---

# 18. Component Registry

Location:

```yaml
registry/components-index.yaml
```

Example:

```yaml
components:
  - accordion
  - alert
  - badge
  - breadcrumb
  - button
  - card
  - checkbox
  - dialog
  - modal
  - radio
  - tabs
```

Used by:

```text
/update-all
```

---

# 19. Reporting

Location:

```text
reports/
```

Example Report:

```yaml
date: 2026-11-06

summary:

  checked: 52
  updated: 5
  unchanged: 47

updatedComponents:

  - button
  - card
  - accordion
```

---

# 20. GitHub Copilot Configuration

## Repository Instructions File

```text
.github/copilot-instructions.md
```

Contents:

```markdown
Helix is the source of truth.

Always generate DSDS 0.23.1 compliant YAML.

Always include source URLs.

Never delete changelog history.

Preserve manual metadata.

Perform validation before saving.

Only record substantive documentation changes.
```

---

# 21. Prompt Specifications

## Create Component Prompt

Objective:

```text
Generate DSDS YAML from live Helix component documentation.
```

---

## Update Component Prompt

Objective:

```text
Synchronize existing DSDS documentation with Helix.
```

---

## Validate Component Prompt

Objective:

```text
Validate YAML and DSDS compliance.
```

---

## Audit Component Prompt

Objective:

```text
Measure documentation completeness and quality.
```

---

# 22. Testing Strategy

## Unit Tests

Validate:

- Parsing
- Mapping
- Generation
- Validation

---

## Integration Tests

Validate:

- Retrieval
- Extraction
- Comparison
- Synchronization

---

## Regression Tests

Validate:

- Existing YAML remains valid
- Changelog remains intact
- Metadata remains intact

---

# 23. Future Enhancements

## Phase 2

- Bulk synchronization
- Registry automation
- Reporting dashboard
- GitHub Actions integration

---

## Phase 3

- Nightly synchronization
- Automated Pull Requests
- Documentation coverage dashboards
- Release note generation

---

## Phase 4

- Token documentation
- Theme documentation
- Pattern documentation
- Figma cross-references
- Storybook integration

---

# 24. Definition of Done

The Helix DSDS Agent is considered production-ready when it can:

1. Generate DSDS YAML from Helix.
2. Update existing DSDS documents.
3. Preserve manual metadata.
4. Maintain machine-readable changelogs.
5. Validate DSDS compliance.
6. Audit documentation coverage.
7. Synchronize all registered components.
8. Produce synchronization reports.
9. Operate entirely inside GitHub Copilot and VS Code.
10. Require no manual YAML authoring for standard documentation workflows.
