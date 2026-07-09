---
name: bmad-iso29110-generate
description: "Generate specific ISO 29110 compliance document from existing BMAD artifacts. Use when user says 'generate traceability matrix' or 'create risk register' or 'ISO generate [type]'."
---

# ISO 29110 Document Generator

Generate any of the 7 ISO 29110 compliance documents from existing BMAD artifacts.

## On Activation

1. Resolve customization: `uv run {project-root}/_bmad/scripts/resolve_customization.py --skill {skill-root} --key workflow`
2. Load project config
3. Determine document type from user request

## Document Types

### traceability
Read `docs/prd.md` (FRs) + architecture (ADs) + `epics.md` (stories) + test files.
Generate `{compliance_reports}/traceability-matrix.md` mapping FR → AD → Story → Test.

### risk-register
Read `epics.md` + `sprint-status.yaml` + retrospectives.
Generate `{compliance_reports}/risk-register.md` with identified risks, severity, mitigation.

### project-plan
Read `sprint-status.yaml` + `epics.md`.
Generate `{compliance_reports}/project-plan.md` with sprint schedule, milestones, team structure.

### test-report
Read E2E test output + `npm run check` + `npm run build` results.
Generate `{compliance_reports}/test-report.md` with pass/fail summary.

### acceptance
Read `sprint-status.yaml` (all P0/P1 done) + test results.
Generate `{compliance_reports}/acceptance-record.md` with sign-off table.

### closure
Read all artifacts + metrics.
Generate `{compliance_reports}/closure-report.md` — final project summary.

### change-log
Read all `sprint-change-proposal-*.md` files.
Generate `{compliance_reports}/change-request-log.md` — collated change history.

## Template Usage

Each document uses the corresponding template from `{iso_templates}/`:

```bash
# Templates installed at:
ls {iso_templates}/
# project-plan-template.md
# risk-register-template.md
# traceability-matrix-template.md
# test-report-template.md
# acceptance-record-template.md
# closure-report-template.md
# change-request-log-template.md
```

Read the template, fill in values from project artifacts, write to `{compliance_reports}/`.

## Workflow

1. Read the requested template from `{iso_templates}/`
2. Read all source artifacts (PRD, epics, sprint-status, etc.)
3. Populate template with actual data
4. Write to `{compliance_reports}/`
5. Report what was generated and any gaps found
