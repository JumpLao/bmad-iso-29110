---
name: bmad-iso29110-close
description: "ISO/IEC 29110 Basic Profile closure workflow. Verify all 14 work products, generate compliance artifacts, run audit, and archive. Use when the user says 'close project' or 'ISO closure' or 'compliance audit'."
---

# ISO 29110 Closure Workflow

Run this at project closure to verify compliance and generate final artifacts.

## On Activation

1. Resolve customization: `uv run {project-root}/_bmad/scripts/resolve_customization.py --skill {skill-root} --key workflow`
2. Load `{project-root}/_bmad/bmm/config.yaml` for project context
3. Greet in `{communication_language}`

## Pre-Flight Check

Verify all required BMAD artifacts exist:

```
✅/❌ PRD (docs/prd.md)
✅/❌ Architecture (docs/architecture.md or ARCHITECTURE-SPINE.md)
✅/❌ Epics (_bmad-output/planning-artifacts/epics.md)
✅/❌ Sprint Status (_bmad-output/implementation-artifacts/sprint-status.yaml)
✅/❌ DESIGN.md + EXPERIENCE.md
```

If any are missing: STOP and report which are needed.

## Step 1: Generate Test Report

Read E2E test results, collate into `{compliance_reports}/test-report.md`:
- Total tests, pass/fail counts
- Cross-tenant isolation evidence (AC4)
- Build verification status

## Step 2: Generate Acceptance Record

Create `{compliance_reports}/acceptance-record.md`:
- Map all P0/P1 stories to acceptance criteria
- Stakeholder sign-off table
- Open issues (accepted with conditions)

## Step 3: Generate Closure Report

Create `{compliance_reports}/closure-report.md`:
- Outcome vs objectives
- Key metrics (test pass rate, stories delivered)
- Lessons learned
- Carry-forward items

## Step 4: Compliance Audit

Check all 14 ISO 29110 work products:

| # | Work Product | BMAD Source | Present? |
|---|-------------|------------|----------|
| 1 | Statement of Work | product-brief.md | |
| 2 | SRS (Requirements) | prd.md | |
| 3 | SDD (Design) | architecture.md | |
| 4 | Epics & Stories | epics.md | |
| 5 | Sprint Status | sprint-status.yaml | |
| 6 | Code Review Records | .memlog.md | |
| 7 | E2E Tests | test files | |
| 8 | Project Plan | project-plan.md | |
| 9 | Risk Register | risk-register.md | |
| 10 | Traceability Matrix | traceability-matrix.md | |
| 11 | Test Report | test-report.md | |
| 12 | Acceptance Record | acceptance-record.md | |
| 13 | Closure Report | closure-report.md | |
| 14 | Change Request Log | change-request-log.md | |

Report any missing as 🔴 gap.

## Step 5: Archive + Tag

```bash
git add -A
git commit -m "ISO 29110 closure: compliance audit complete"
git tag iso29110-1.0.0
```

## Output

Summary table showing pass/fail per work product, plus git tag confirmation.
