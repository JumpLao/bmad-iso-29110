# BMAD ISO 29110 Extension

Add ISO/IEC 29110 Basic Profile compliance to any BMAD-METHOD project.

ISO 29110 is the international standard for Very Small Entities (VSEs, ≤25 people). This extension auto-generates the 7 process documents that BMAD doesn't produce natively — so your project is audit-ready without manual paperwork.

## How It Works

```
BMAD Phase Complete
       ↓
on_complete hook fires
       ↓
bmad-iso29110-generate runs silently
       ↓
Reads BMAD artifacts (PRD, epics, etc.)
       ↓
Fills ISO template with real data
       ↓
Writes to _bmad-output/compliance-reports/
       ↓
BMAD continues to next phase
```

### Auto-Generation Schedule

| BMAD Phase Completes | ISO Document Auto-Generated | Source Data Read |
|---|---|---|
| **PRD** (Phase 3) | Traceability Matrix (seed) | FRs from `prd.md` |
| **Epics** (Phase 6) | Risk Register | Stories, complexity, dependencies from `epics.md` |
| **Sprint Planning** (Phase 7) | Project Plan | Schedule, milestones from `sprint-status.yaml` |
| **QA** (Phase 9) | Test Report | E2E test results, build status, AC4 evidence |
| **Closure** (manual) | Closure Report + Acceptance Record | All artifacts + metrics |

No manual intervention needed — documents appear automatically as you progress through BMAD phases.

### Manual Override

Need a specific document on-demand? Generate any document anytime:

```bash
/bmad-iso29110-generate traceability     # Re-generate traceability matrix
/bmad-iso29110-generate risk-register    # Refresh risk register
/bmad-iso29110-generate project-plan     # Update project plan
/bmad-iso29110-generate test-report      # Collate test results
/bmad-iso29110-generate change-log       # Update change request log
```

### Project Closure

Closure is intentionally **manual** — it's a quality gate, not an auto-step:

```bash
/bmad-iso29110-close
```

This runs a 7-step closure workflow with remediation loop:
1. **Pre-flight check** — verify all BMAD artifacts exist (PRD, architecture, epics, etc.)
2. **Generate missing phase docs** — auto-generate project plan + change log if not already created
3. **Generate test report** — collate E2E results into formal test report
4. **Generate acceptance record** — stakeholder sign-off with acceptance criteria
5. **Generate closure report** — final project summary with metrics
6. **Compliance audit + remediation loop**:
   - Audit all 14 ISO 29110 work products + quality gaps (partial criteria, planned-not-done stories, missing companion files)
   - Classify each gap as AUTO-FIXABLE or HUMAN-NEEDED
   - Auto-fix: generate README, update sprint-status, create DESIGN.md/EXPERIENCE.md stubs
   - Re-generate affected reports (acceptance, closure, traceability)
   - Re-audit with before/after delta
   - Handle remaining HUMAN-NEEDED gaps (resolve / accept with condition / block)
7. **Archive + tag** — git commit + tag `iso29110-1.0.0` for audit trail

The remediation loop ensures closure is **clean** — not just "documented gaps". Auto-fixable issues are resolved before the project is closed.

## Installation

> **IMPORTANT:** `iso-29110` is a custom module — you MUST use `--custom-source` to install it. Using `--modules iso-29110` alone will fail.

```bash
# Step 1: Install BMAD core
npx bmad-method install \
  --directory . \
  --modules bmm \
  --tools claude-code \
  --yes

# Step 2: Install ISO extension
npx bmad-method install \
  --directory . \
  --modules iso-29110 \
  --custom-source https://github.com/JumpLao/bmad-iso-29110 \
  --tools claude-code \
  --yes
```

Works with Claude Code, Codex, Cursor, GitHub Copilot, and 30+ other BMAD-supported tools.

### What Gets Installed

```
your-project/
├── .claude/skills/                    # or .agents/skills/ for Codex
│   ├── bmad-iso29110-close/           # Closure + remediation skill
│   │   └── SKILL.md
│   └── bmad-iso29110-generate/        # Document generator skill
│       ├── SKILL.md
│       └── assets/
│           ├── templates/             # 7 ISO document templates
│           │   ├── project-plan-template.md
│           │   ├── risk-register-template.md
│           │   ├── traceability-matrix-template.md
│           │   ├── test-report-template.md
│           │   ├── acceptance-record-template.md
│           │   ├── closure-report-template.md
│           │   └── change-request-log-template.md
│           └── overrides/             # 6 phase boundary hooks
│               ├── bmad-prd.toml
│               ├── bmad-create-epics-and-stories.toml
│               ├── bmad-sprint-planning.toml
│               ├── bmad-correct-course.toml
│               ├── bmad-qa-generate-e2e-tests.toml
│               └── config.toml
├── _bmad/iso-29110/                   # Module config
│   ├── config.yaml
│   └── module-help.csv
└── _bmad-output/
    └── compliance-reports/            # Generated docs land here
```

## The 14 ISO 29110 Work Products

BMAD already produces 7 of the 14 required work products. This extension adds the remaining 7.

| # | ISO 29110 Work Product | Source | How |
|---|---|---|---|
| 1 | Statement of Work | `product-brief.md` | ✅ BMAD native |
| 2 | Software Requirements Spec | `prd.md` | ✅ BMAD native |
| 3 | Software Design Description | `architecture.md` | ✅ BMAD native |
| 4 | Epics & Stories | `epics.md` | ✅ BMAD native |
| 5 | Sprint Status | `sprint-status.yaml` | ✅ BMAD native |
| 6 | Code Review Records | `.memlog.md` | ✅ BMAD native |
| 7 | E2E Tests | test files | ✅ BMAD native |
| 8 | **Project Plan** | `compliance-reports/project-plan.md` | 🆕 Auto after sprint planning |
| 9 | **Risk Register** | `compliance-reports/risk-register.md` | 🆕 Auto after epics |
| 10 | **Traceability Matrix** | `compliance-reports/traceability-matrix.md` | 🆕 Auto after PRD |
| 11 | **Test Report** | `compliance-reports/test-report.md` | 🆕 Auto after QA |
| 12 | **Acceptance Record** | `compliance-reports/acceptance-record.md` | 🆕 At closure |
| 13 | **Closure Report** | `compliance-reports/closure-report.md` | 🆕 At closure |
| 14 | **Change Request Log** | `compliance-reports/change-request-log.md` | 🆕 On scope change |

## Extension Architecture

```
bmad-iso-29110/                                ← This repository
├── .claude-plugin/marketplace.json             ← BMAD plugin registry (skills array)
├── .claude/skills/
│   ├── bmad-iso29110-close/SKILL.md            ← Closure + remediation (7-step)
│   └── bmad-iso29110-generate/
│       ├── SKILL.md                             ← Document generator (auto + manual)
│       └── assets/
│           ├── templates/                       ← 7 ISO document templates
│           └── overrides/                       ← 6 phase boundary hooks
├── module.yaml                                 ← Module config (code, name, description)
├── module-help.csv                             ← Skill catalog for BMAD
└── README.md                                   ← You are here
```

## Demo Project

See a full end-to-end example: **[pomodoro-iso29110-demo](https://github.com/JumpLao/pomodoro-iso29110-demo)**

A Python CLI Pomodoro timer built from spec to ISO 29110 compliance audit using **Codex + BMAD-METHOD + this extension**:

- 12 functional requirements, 8 NFRs, 6 architecture decisions
- 4 epics, 9 stories with Given/When/Then acceptance criteria
- Python CLI implementation (stdlib only) + 13 unittest tests (all pass)
- Full ISO 29110 compliance: **14/14 work products verified**
- Remediation loop auto-fixed 5 gaps (missing README, DESIGN.md, EXPERIENCE.md, stale sprint-status, partial acceptance criterion)
- Git tagged `iso29110-1.0.0`

Tested with both **Claude Code** and **Codex** — skills work across both tools via `.claude/skills/` and `.agents/skills/` respectively.

## Template Variables

Each template uses `{{placeholder}}` syntax. The generator reads BMAD artifacts and fills these automatically:

| Variable Pattern | Source | Example |
|-----------------|--------|---------|
| `{{project_name}}` | BMAD config | `badminton-queue-v2` |
| `{{date}}` | Current date | `2026-07-09` |
| `{{fr_count}}` | `prd.md` FR list | `39` |
| `{{epic_count}}` | `epics.md` | `13` |
| `{{story_count}}` | `sprint-status.yaml` | `54` |
| `{{test_pass_rate}}` | E2E results | `96%` |
| `{{git_sha}}` | `git rev-parse` | `fca5798` |
| `{{team_size}}` | `module.yaml` config | `3` |

164 total placeholders across 7 templates, all auto-filled from project artifacts.

## Without BMAD

The templates can be used standalone (without BMAD installation):

```bash
# Just copy templates
cp -r templates/ /your-project/docs/iso-29110/
# Fill in manually
```

However, auto-generation hooks and the generator skill require BMAD.

## Requirements

- BMAD-METHOD v6.9+ installed (`npx bmad-method install --modules bmm`)
- Claude Code (for skill execution)
- Python 3.10+ with `uv` (BMAD scripts dependency)

## License

MIT

## Repository

- **GitHub:** [JumpLao/bmad-iso-29110](https://github.com/JumpLao/bmad-iso-29110)
- **BMAD-METHOD:** [bmad-code-org/BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD)
- **ISO 29110 Standard:** [ISO/IEC 29110](https://www.iso.org/standard/51158.html)