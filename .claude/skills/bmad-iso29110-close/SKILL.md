---
name: bmad-iso29110-close
description: "ISO/IEC 29110 Basic Profile closure workflow. Verify all 14 work products, generate compliance artifacts, run audit, remediate gaps, and archive. Use when the user says 'close project' or 'ISO closure' or 'compliance audit'."
---

# ISO 29110 Closure Workflow

Run this at project closure to verify compliance, remediate gaps, and generate final artifacts.

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

If any are missing: note them — the remediation loop (Step 6) will attempt to generate missing BMAD support files.

## Step 1: Generate Missing Phase Documents

Before the audit, check if any phase-generated documents are missing and generate them:

1. **Project Plan** — if `_bmad-output/compliance-reports/project-plan.md` does not exist:
   - Read `sprint-status.yaml` and `epics.md`
   - Generate using `bmad-iso29110-generate project-plan`
   - Log: "ISO 29110: Project plan was not generated during sprint planning — generating now."

2. **Change Request Log** — if `_bmad-output/compliance-reports/change-request-log.md` does not exist:
   - Check for `sprint-change-proposal-*.md` files
   - If proposals exist: generate using `bmad-iso29110-generate change-log`
   - If no proposals: create a stub stating "No scope changes were made during this project."
   - Log: "ISO 29110: Change request log was not generated during correct-course — generating now."

## Step 2: Generate Test Report

Read E2E test results, collate into `{compliance_reports}/test-report.md`:
- Total tests, pass/fail counts
- Cross-tenant isolation evidence (AC4)
- Build verification status

## Step 3: Generate Acceptance Record

Create `{compliance_reports}/acceptance-record.md`:
- Map all P0/P1 stories to acceptance criteria
- Stakeholder sign-off table
- Open issues (accepted with conditions)

## Step 4: Generate Closure Report

Create `{compliance_reports}/closure-report.md`:
- Outcome vs objectives
- Key metrics (test pass rate, stories delivered)
- Lessons learned
- Carry-forward items

## Step 5: Compliance Audit (First Pass)

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

Also check for **quality gaps** (work product exists but content is incomplete):
- Scan acceptance-record for "Partially" criteria
- Scan sprint-status for stories still "planned" when code exists
- Scan closure-report for "carry-forward" or "conditional" items
- Check for missing companion artifacts: README.md, DESIGN.md, EXPERIENCE.md

Report all issues as 🔴 gap or 🟡 quality issue.

**If all 14 pass with no quality gaps → skip to Step 7 (Archive).**

## Step 6: Remediation Loop

When the audit finds gaps or quality issues, classify and resolve them before closing.

### 6a: Classify Each Gap

For every 🔴 gap and 🟡 quality issue, classify as AUTO-FIXABLE or HUMAN-NEEDED:

| Classification | Criteria | Examples |
|---------------|----------|----------|
| **AUTO-FIXABLE** | Missing artifact can be generated from existing project data | README, DESIGN.md, EXPERIENCE.md stub, sprint-status update, traceability re-generation |
| **HUMAN-NEEDED** | Requires a business/product decision or external sign-off | Stakeholder acceptance, unresolved OQs, genuinely unimplemented features |

Present the classification table to the user:

```
Gap Classification:
┌──────────────────────────────────────┬──────────────┬────────────────────────────────┐
│ Gap                                  │ Type         │ Remediation                    │
├──────────────────────────────────────┼──────────────┼────────────────────────────────┤
│ Missing README.md                    │ AUTO-FIXABLE │ Generate from PRD + source     │
│ sprint-status stories still planned  │ AUTO-FIXABLE │ Update to done (code verified) │
│ DESIGN.md missing                    │ AUTO-FIXABLE │ Derive from architecture.md    │
│ Acceptance AC-007 "Partially"        │ AUTO-FIXABLE │ Fix root cause (missing doc)   │
│ Stakeholder sign-off pending         │ HUMAN-NEEDED │ Ask user to approve/defer      │
│ OQ: interrupted-session policy       │ HUMAN-NEEDED │ Ask user to decide/defer       │
└──────────────────────────────────────┴──────────────┴────────────────────────────────┘
```

### 6b: Auto-Fix AUTO-FIXABLE Gaps

Execute fixes for each AUTO-FIXABLE gap. Common fixes:

1. **Missing README.md** → Generate from PRD + architecture + source code:
   - Project overview (from product-brief.md)
   - Installation (Python version, no deps)
   - Usage (commands from CLI --help output)
   - Configuration (default paths and settings)

2. **sprint-status stories still "planned"** when implementation exists → Update each story:
   - Read source files and test results
   - If code + tests exist for a story → set `status: done`
   - If code exists but no tests → set `status: in-progress`
   - Regenerate acceptance-record and closure-report to reflect updated statuses

3. **Missing DESIGN.md** → Derive from architecture.md:
   - Copy architecture decisions, component diagram, tech stack
   - Add DESIGN.md reference to pre-flight check

4. **Missing EXPERIENCE.md** → Create appropriate stub:
   - For CLI/API projects: "This project has no graphical UI. User experience is defined by CLI command behavior documented in PRD and README."
   - For web/app projects: flag as HUMAN-NEEDED (UX must be authored)

5. **Acceptance criteria "Partially"** → Fix the root cause:
   - If partial because of missing README → generate README, then re-evaluate as "Yes"
   - If partial because story not implemented → stays HUMAN-NEEDED

Log each fix: "🔧 Remediated: {gap} → {action taken}"

### 6c: Re-Generate Affected Reports

After auto-fixes, regenerate any reports whose inputs changed:
- If sprint-status changed → regenerate acceptance-record, closure-report
- If README created → update acceptance criteria evaluation
- If traceability gaps fixed → regenerate traceability-matrix

### 6d: Re-Audit

Run the 14-work-product audit again. Report the delta:
```
Re-Audit Results:
Before: X/14 pass, Y gaps
After:  Z/14 pass, W gaps (remaining are HUMAN-NEEDED)
Delta:  +N auto-fixed
```

### 6e: Handle HUMAN-NEEDED Gaps

For each remaining HUMAN-NEEDED gap, present options to the user:

```
⚠️ HUMAN-NEEDED — {gap description}
  1. Resolve now (provide answer/decision)
  2. Accept with condition (carry-forward to next sprint)
  3. Block closure (stop here)
```

- If user resolves → fix and re-audit
- If user accepts with condition → record in closure-report and acceptance-record as "Accepted with Condition"
- If user blocks → STOP, do not archive

**All HUMAN-NEEDED gaps must be either resolved or accepted before proceeding to Step 7.**

## Step 7: Archive + Tag

Only reached when all 14 work products pass AND all quality gaps are resolved or accepted.

```bash
git add -A
git commit -m "ISO 29110 closure: compliance audit complete (14/14 pass, N remediated)"
git tag iso29110-1.0.0
```

## Output

Final summary:

```
ISO 29110 Closure — Final Report
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Work Products: 14/14 ✅
Quality Gaps:  0 remaining (N auto-fixed, M accepted with condition)

Remediation Summary:
  🔧 Auto-fixed: {list}
  📋 Accepted with condition: {list}
  ✅ Human-resolved: {list}

Git: tag iso29110-1.0.0 created
Closure status: COMPLETE
```
