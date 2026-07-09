# Test Report

> **ISO/IEC 29110 Basic Profile** — Work Product: Test Report
> Generated from BMAD artifacts: E2E test output, `npm run check`, `npm run build` results

---

## Document Control

| Field | Value |
|-------|-------|
| **Project Name** | {{project_name}} |
| **Project ID** | {{project_id}} |
| **Version** | {{test_report_version}} |
| **Date Created** | {{date_created}} |
| **Last Updated** | {{last_updated}} |
| **Prepared By** | {{author}} |
| **Tested By** | {{tester}} |
| **Build Version** | {{build_version}} |

---

## 1. Test Summary

| Metric | Count | Percentage |
|--------|-------|------------|
| Total Test Cases | {{total_tests}} | 100% |
| Passed | {{tests_passed}} | {{pct_passed}} |
| Failed | {{tests_failed}} | {{pct_failed}} |
| Blocked / Skipped | {{tests_blocked}} | {{pct_blocked}} |
| **Overall Pass Rate** | — | {{overall_pass_rate}} |

**Test Result:** {{pass_fail_status}}

---

## 2. Test Execution Environment

| Environment | Details |
|-------------|---------|
| OS | {{test_os}} |
| Node Version | {{node_version}} |
| Browser(s) | {{browsers}} |
| Database | {{test_db}} |
| Test Framework | {{test_framework}} |

---

## 3. Build Verification

| Check | Command | Status | Timestamp |
|-------|---------|--------|-----------|
| Linting | `npm run check` | {{lint_status}} | {{lint_timestamp}} |
| Build | `npm run build` | {{build_status}} | {{build_timestamp}} |
| E2E Tests | `npm run test:e2e` | {{e2e_status}} | {{e2e_timestamp}} |

---

## 4. Detailed Test Results

### 4.1 Functional Tests

| Test Suite | Test Case | Status | Duration | Notes |
|------------|-----------|--------|----------|-------|
| {{suite_name}} | {{test_case}} | {{test_status}} | {{duration}} | {{test_notes}} |

### 4.2 Non-Functional Tests

| NFR Category | Test | Result | Measurement |
|-------------|------|--------|-------------|
| {{nfr_cat}} | {{nfr_test}} | {{nfr_result}} | {{nfr_measurement}} |

---

## 5. Failed Test Details

| Test ID | Test Name | Error Summary | Severity | Linked Bug |
|---------|-----------|---------------|----------|------------|
| {{failed_test_id}} | {{failed_test_name}} | {{error_summary}} | {{failure_severity}} | {{bug_ref}} |

---

## 6. Coverage Report

| Coverage Type | Percentage |
|---------------|------------|
| Line Coverage | {{line_coverage}} |
| Branch Coverage | {{branch_coverage}} |
| Function Coverage | {{function_coverage}} |

---

## 7. Known Issues & Limitations

{{known_issues}}

---

## 8. Conclusion

{{test_conclusion}}

---

*ISO 29110 alignment: This report satisfies ISO/IEC 29110-5-1-2 Testing & Verification objectives.*
