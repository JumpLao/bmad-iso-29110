# Traceability Matrix

> **ISO/IEC 29110 Basic Profile** — Work Product: Traceability Matrix
> Generated from BMAD artifacts: `prd.md` (FRs), architecture (ADs), `epics.md` (stories), test files

---

## Document Control

| Field | Value |
|-------|-------|
| **Project Name** | {{project_name}} |
| **Project ID** | {{project_id}} |
| **Version** | {{tm_version}} |
| **Date Created** | {{date_created}} |
| **Last Updated** | {{last_updated}} |
| **Prepared By** | {{author}} |

---

## Purpose

This matrix provides bi-directional traceability from Requirements → Architecture Decisions → Epics/Stories → Tests, ensuring complete coverage per ISO/IEC 29110 requirements management objectives.

---

## Traceability Matrix

| Req ID | Requirement Description | Source (PRD) | Architecture Decision | Epic / Story ID | Test Case ID | Test Status | Coverage |
|--------|------------------------|-------------|----------------------|-----------------|-------------|-------------|----------|
| FR-001 | {{fr_001_desc}} | {{fr_001_source}} | {{fr_001_ad}} | {{fr_001_story}} | {{fr_001_test}} | {{fr_001_test_status}} | {{fr_001_coverage}} |
| FR-002 | {{fr_002_desc}} | {{fr_002_source}} | {{fr_002_ad}} | {{fr_002_story}} | {{fr_002_test}} | {{fr_002_test_status}} | {{fr_002_coverage}} |
| FR-003 | {{fr_003_desc}} | {{fr_003_source}} | {{fr_003_ad}} | {{fr_003_story}} | {{fr_003_test}} | {{fr_003_test_status}} | {{fr_003_coverage}} |
| FR-004 | {{fr_004_desc}} | {{fr_004_source}} | {{fr_004_ad}} | {{fr_004_story}} | {{fr_004_test}} | {{fr_004_test_status}} | {{fr_004_coverage}} |
| FR-005 | {{fr_005_desc}} | {{fr_005_source}} | {{fr_005_ad}} | {{fr_005_story}} | {{fr_005_test}} | {{fr_005_test_status}} | {{fr_005_coverage}} |

---

## Coverage Summary

| Coverage Type | Count | Percentage |
|--------------|-------|------------|
| Total Requirements | {{total_reqs}} | 100% |
| Requirements with Story | {{reqs_with_story}} | {{pct_with_story}} |
| Requirements with Test | {{reqs_with_test}} | {{pct_with_test}} |
| Requirements Passed | {{reqs_passed}} | {{pct_passed}} |
| Requirements Untested | {{reqs_untested}} | {{pct_untested}} |

---

## Coverage Gaps

| Gap Type | Req ID | Description | Action |
|----------|--------|-------------|--------|
| {{gap_type}} | {{gap_req_id}} | {{gap_desc}} | {{gap_action}} |

---

## Non-Functional Requirements Traceability

| NFR ID | NFR Description | Architecture Ref | Validation Method | Status |
|--------|----------------|-----------------|-------------------|--------|
| {{nfr_id}} | {{nfr_desc}} | {{nfr_arch}} | {{nfr_validation}} | {{nfr_status}} |

---

## Source Artifact References

| Artifact | Path | Version |
|----------|------|---------|
| Product Requirements | `docs/prd.md` | {{prd_version}} |
| Architecture Document | {{arch_doc_path}} | {{arch_version}} |
| Epics | {{epics_path}} | {{epics_version}} |
| Sprint Status | {{sprint_path}} | {{sprint_version}} |

---

*ISO 29110 alignment: This matrix satisfies ISO/IEC 29110-5-1-2 Requirements Analysis & Design traceability objectives.*
