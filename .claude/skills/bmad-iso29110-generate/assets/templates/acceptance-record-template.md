# Acceptance Record

> **ISO/IEC 29110 Basic Profile** — Work Product: Acceptance Record
> Generated from BMAD artifacts: `sprint-status.yaml`, test results, acceptance criteria

---

## Document Control

| Field | Value |
|-------|-------|
| **Project Name** | {{project_name}} |
| **Project ID** | {{project_id}} |
| **Version** | {{acceptance_version}} |
| **Date Created** | {{date_created}} |
| **Last Updated** | {{last_updated}} |
| **Prepared By** | {{author}} |
| **Acceptance Date** | {{acceptance_date}} |

---

## 1. Acceptance Overview

**Product Delivered:** {{product_name}}
**Release Version:** {{release_version}}
**Acceptance Decision:** {{acceptance_decision}}

---

## 2. Acceptance Criteria Evaluation

| Req ID | Acceptance Criterion | Priority | Met? | Evidence |
|--------|---------------------|----------|------|----------|
| AC-001 | {{ac_001_criterion}} | P0 | {{ac_001_met}} | {{ac_001_evidence}} |
| AC-002 | {{ac_002_criterion}} | P0 | {{ac_002_met}} | {{ac_002_evidence}} |
| AC-003 | {{ac_003_criterion}} | P1 | {{ac_003_met}} | {{ac_003_evidence}} |
| AC-004 | {{ac_004_criterion}} | P1 | {{ac_004_met}} | {{ac_004_evidence}} |
| AC-005 | {{ac_005_criterion}} | P2 | {{ac_005_met}} | {{ac_005_evidence}} |

**Criteria Summary:** {{criteria_met}} of {{criteria_total}} criteria met ({{criteria_pct}}).

---

## 3. Deliverables Verification

| Deliverable | Description | Status | Accepted |
|-------------|-------------|--------|----------|
| {{deliverable_id}} | {{deliverable_desc}} | {{deliverable_status}} | {{deliverable_accepted}} |

---

## 4. Story Completion Summary

| Priority | Total Stories | Completed | Remaining | Blocked |
|----------|--------------|-----------|-----------|---------|
| P0 | {{p0_total}} | {{p0_done}} | {{p0_remaining}} | {{p0_blocked}} |
| P1 | {{p1_total}} | {{p1_done}} | {{p1_remaining}} | {{p1_blocked}} |
| P2 | {{p2_total}} | {{p2_done}} | {{p2_remaining}} | {{p2_blocked}} |

---

## 5. Open Issues / Conditions

| Issue ID | Description | Severity | Resolution Condition | Owner |
|----------|-------------|----------|---------------------|-------|
| {{issue_id}} | {{issue_desc}} | {{issue_severity}} | {{resolution_condition}} | {{issue_owner}} |

**Accepted with Conditions:** {{accepted_with_conditions}}

---

## 6. Stakeholder Sign-Off

| Role | Name | Decision | Signature | Date |
|------|------|----------|-----------|------|
| Project Manager | {{pm_name}} | {{pm_decision}} | ______________ | {{pm_date}} |
| Product Owner | {{po_name}} | {{po_decision}} | ______________ | {{po_date}} |
| Technical Lead | {{tl_name}} | {{tl_decision}} | ______________ | {{tl_date}} |
| Customer / Sponsor | {{customer_name}} | {{customer_decision}} | ______________ | {{customer_date}} |

---

## 7. Reference Documents

- Test Report: `{{test_report_path}}`
- Traceability Matrix: `{{tm_path}}`
- PRD: `docs/prd.md`

---

*ISO 29110 alignment: This record satisfies ISO/IEC 29110-5-1-2 Acceptance & Delivery objectives.*
