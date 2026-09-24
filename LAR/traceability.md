# Traceability

**Maintained by:** Agent C (Verification Engineer)  
**Audited by:** Agent D (Process & Compliance)  
**Validated at:** Every phase gate

Nine traceability files covering all ten bidirectional links. Agent C validates these at every phase transition. Agent D audits the validation before authorizing each human gate. Any gap in any link blocks the gate.

---

## Files in this directory

| File | Links Covered |
|------|--------------|
| `sys_req_to_hlr.md` | Link 1: SYS ↔ HLR |
| `hlr_to_llr.md` | Link 2: HLR ↔ LLR |
| `hlr_to_architecture.md` | Link 3: HLR ↔ Architecture Components |
| `llr_to_code.md` | Link 4: LLR ↔ Source Code |
| `llr_to_design.md` | Link 5: LLR ↔ Component Design |
| `sys_req_to_test.md` | Link 6: SYS ↔ System Test Cases |
| `hlr_to_test_cases.md` | Link 7: HLR ↔ HLR Test Cases |
| `llr_to_test_cases.md` | Link 8: LLR ↔ LLR Test Cases |
| `test_cases_to_results.md` | Link 9: Test Cases ↔ Test Results |

Link 10 (Source Code ↔ Structural Coverage) is maintained in `verification/coverage_analysis/`.

---

## Format (all files)

```markdown
# [Link Name]
# Last validated: [date] by Agent C (see TV-XXXX)

| Left Artifact | Right Artifact | Forward ✓ | Reverse ✓ |
|---------------|---------------|-----------|-----------|
| SYS-0001      | HLR-0101      | ✓         | ✓         |
| SYS-0001      | HLR-0102      | ✓         | ✓         |
| HLR-0103      | [ORPHAN]      | ✗         | n/a       |
```

---

## Validation Rules

- **No orphan requirements** — every requirement has a parent or is flagged DERIVED and dispositioned by a human
- **No untested requirements** — every requirement has at least one test case
- **No untraceable code** — every code element traces to an LLR
- **No untraceable tests** — every test traces to a requirement
- **No unverified requirements** — every requirement has a passing test result
- **Bidirectional consistency** — if A→B exists then B→A exists
- **Derived dispositioned** — every derived requirement has a human disposition record

---

## Gate Requirements

| Gate | Links That Must Be Complete |
|------|-----------------------------|
| Gate 2 — SYS approval | Links 1, 6 |
| Gate 3 — Requirements baseline | Links 1, 2, 7 |
| Gate 4 — Design baseline | Links 3, 5 |
| Gate 5 — Code baseline | Links 4, 8, 9, 10 |
| Gate 6 — Product baseline | All links |
