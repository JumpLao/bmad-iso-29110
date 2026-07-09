# BMAD ISO 29110 Extension

Add ISO/IEC 29110 Basic Profile compliance to any BMAD-METHOD project.

## What it does

- **Process Documentation**: Generates project plan, risk register, traceability matrix
- **Compliance Artifacts**: Test reports, acceptance records, closure reports, change logs
- **Audit Ready**: Verifies all 14 ISO 29110 work products exist and are compliant
- **BMAD Integration**: Works seamlessly with existing BMAD artifacts (PRD, epics, stories)

## Installation

Requires an existing BMAD installation first:

```bash
# In your project directory (with BMAD already installed)
npx bmad-method install \
  --directory . \
  --modules iso-29110 \
  --yes
```

This installs:
- Two skills: `/bmad-iso29110-close`, `/bmad-iso29110-generate`
- 7 document templates in `_bmad-output/iso-29110-templates/`
- Extended PM/architect agents with ISO awareness

## Usage

### Generate Documents

```bash
# Generate any ISO document
/bmad-iso29110-generate traceability
/bmad-iso29110-generate risk-register
/bmad-iso29110-generate project-plan
# etc.
```

### Project Closure

```bash
# Run full ISO 29110 compliance audit and closure
/bmad-iso29110-close
```

## How it Works

1. **Reads BMAD artifacts**: PRD, epics, sprint-status, architecture
2. **Fills templates**: Uses {{placeholder}} syntax
3. **Generates docs**: In `_bmad-output/compliance-reports/`
4. **Checks compliance**: Verifies all 14 work products exist
5. **Archives**: Git tag for audit trail

## ISO 29110 Work Products

| # | Work Product | BMAD Source |
|---|-------------|------------|
| 1 | Statement of Work | product-brief.md |
| 2 | SRS (Requirements) | prd.md |
| 3 | SDD (Design) | architecture.md |
| 4 | Epics & Stories | epics.md |
| 5 | Sprint Status | sprint-status.yaml |
| 6 | Code Review Records | .memlog.md |
| 7 | E2E Tests | test files |
| 8 | Project Plan | project-plan.md |
| 9 | Risk Register | risk-register.md |
| 10 | Traceability Matrix | traceability-matrix.md |
| 11 | Test Report | test-report.md |
| 12 | Acceptance Record | acceptance-record.md |
| 13 | Closure Report | closure-report.md |
| 14 | Change Request Log | change-request-log.md |

---

**Note**: This is a BMAD-METHOD extension, not a Hermes skill. Install via `npx bmad-method install --modules iso-29110`.