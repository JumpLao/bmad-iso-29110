---
name: bmad-iso29110-generate
description: "Generate specific ISO 29110 compliance document from existing BMAD artifacts. Use when user says 'generate traceability matrix' or 'create risk register' or 'ISO generate [type]'. Also triggered automatically by on_complete hooks."
---

# ISO 29110 Document Generator

Generate any of the 7 ISO 29110 compliance documents from existing BMAD artifacts.

## On Activation

1. Resolve customization: `uv run {project-root}/_bmad/scripts/resolve_customization.py --skill {skill-root} --key workflow`
2. Load project config: `{project-root}/_bmad/bmm/config.yaml`
3. Determine document type from user request or hook args
4. **Ensure output directory exists:** `mkdir -p {project-root}/_bmad-output/compliance-reports`

## Auto-Generation Mode

When triggered by `on_complete` hook (automatic):
- Args passed from the hook: `args = ["traceability"]` etc.
- Skip user interaction — generate silently
- Log output: "ISO 29110: Generated {document} at {path}"
- Continue to next BMAD phase normally

## Manual Generation Mode

When invoked by user (`/bmad-iso29110-generate traceability`):
- Confirm document type with user
- Show what artifacts will be read
- Generate + report results
- Show summary table of generated content

## Document Types

### traceability
**When:** After PRD (auto), after Epics (update)
**Reads:** `docs/prd.md` (FRs), architecture (ADs), `epics.md` (stories), test files
**Writes:** `{compliance_reports}/traceability-matrix.md`
**Content:** Table mapping FR → AD → Story → Test → Status

### risk-register
**When:** After Epics (auto), at sprint start
**Reads:** `epics.md`, `sprint-status.yaml`, retrospectives
**Writes:** `{compliance_reports}/risk-register.md`
**Content:** Risks with severity, likelihood, impact, owner, mitigation

### project-plan
**When:** After Sprint Planning (auto)
**Reads:** `sprint-status.yaml`, `epics.md`
**Writes:** `{compliance_reports}/project-plan.md`
**Content:** Sprint schedule, milestones, team structure, risk budget

### test-report
**When:** After QA (auto), before closure
**Reads:** E2E test output, `npm run check`, `npm run build`, AC4 evidence
**Writes:** `{compliance_reports}/test-report.md`
**Content:** Pass/fail counts, build verification, cross-tenant evidence, failed test details

### acceptance
**When:** At closure (manual — part of `/bmad-iso29110-close`)
**Reads:** `sprint-status.yaml`, test results
**Writes:** `{compliance_reports}/acceptance-record.md`
**Content:** Acceptance criteria verification, stakeholder sign-off, deliverables checklist

### closure
**When:** At closure (manual — part of `/bmad-iso29110-close`)
**Reads:** All artifacts + metrics
**Writes:** `{compliance_reports}/closure-report.md`
**Content:** Outcome vs objectives, metrics, lessons, carry-forward, archive

### change-log
**When:** When scope changes (manual or after correct-course)
**Reads:** All `sprint-change-proposal-*.md` files
**Writes:** `{compliance_reports}/change-request-log.md`
**Content:** Collated change history with impact, priority, status

## Template Usage

Templates ship as skill assets. After install they live alongside SKILL.md in the skill directory.

```bash
# Templates are bundled in this skill's assets/ directory:
ls {skill-root}/assets/templates/

# Override hooks are in assets/overrides/:
ls {skill-root}/assets/overrides/
```

`{skill-root}` resolves to the installed skill directory (e.g., `.claude/skills/bmad-iso29110-generate/`).

## Workflow

1. Read the requested template from `{skill-root}/assets/templates/{type}-template.md`
2. Read all source artifacts (PRD, epics, sprint-status, etc.)
3. Populate template with actual data — replace `{{placeholder}}` with values
4. Write to `{project-root}/_bmad-output/compliance-reports/{type}.md`
5. If auto-mode: log "ISO 29110: Generated {document}"
6. If manual-mode: report what was generated + any gaps found