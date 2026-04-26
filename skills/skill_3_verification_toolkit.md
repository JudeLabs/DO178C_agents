# Skill 3: Verification Toolkit

**Used by:** Agent C (all modes), Agent D (Traceability mode)

## Purpose

Three-mode verification toolkit covering artifact reviews, structural coverage analysis, and traceability validation. All modes enforce the independence principle: findings are based solely on documented artifacts, never on verbal explanation or agent reasoning.

## Critical Rule

> You are independent from the agents who produced the artifacts you are reviewing. Base your findings **ONLY** on what is documented in the artifact. Do not accept verbal explanations, rationale, or "it works because I designed it that way" from Agent A or Agent B. If the artifact doesn't demonstrate compliance, it doesn't comply.

---

## Mode 1: Review

**Use when:** Any artifact from Agent A or Agent B is received for verification.

### Checklists by Artifact Type

#### Requirements (SYS, HLR, or LLR)

| ID | Criterion | Check |
|----|-----------|-------|
| R01 | Compliance with standards | Correct format, ID scheme, all required fields present |
| R02 | Accuracy and consistency | Accurately reflects parent; no contradictions with siblings |
| R03 | Completeness | All modes, transitions, errors, init/shutdown, I/O, timing, interfaces covered |
| R04 | Verifiability | Can a test with specific inputs and measurable pass/fail be written? |
| R05 | Conformance | No banned words; single atomic shall statement |
| R06 | Traceability | Parent field contains valid ID; parent exists in LAR; DERIVED flagged |
| R07 | Algorithm accuracy | Equations correct; units consistent; data types appropriate |

#### Architecture

| ID | Criterion | Check |
|----|-----------|-------|
| A01 | HLR allocation completeness | Every HLR allocated to ≥1 component |
| A02 | Interface completeness | Every component pair exchange is defined |
| A03 | Data coupling analysis | All data flows documented and match HLR |
| A04 | Control coupling analysis | All control flows documented and match design |
| A05 | Partitioning adequacy | Safety partition boundaries and inter-partition interfaces controlled |
| A06 | Resource feasibility | CPU, memory, stack, bus bandwidth budgets defined with margin |

#### Source Code

| ID | Criterion | Check |
|----|-----------|-------|
| C01 | Coding standards compliance | Conforms to SDP standards; static analysis findings reviewed |
| C02 | LLR compliance | Implementation matches LLR logic, thresholds, data types, error handling |
| C03 | No dead code | Every function, branch, statement traces to an LLR |
| C04 | No deactivated code | Reachable-but-inactive code is justified or flagged |
| C05 | Robustness | All LLR error conditions handled; input validation, range checking present |
| C06 | Architecture consistency | Conforms to component structure, interfaces, and data flows |

#### Test Cases

| ID | Criterion | Check |
|----|-----------|-------|
| T01 | Requirement coverage | Every requirement has ≥1 test case |
| T02 | Repeatability | Procedures are unambiguous; two people get same result |
| T03 | Expected results | Specific values and measurable pass/fail criteria for every test |
| T04 | Condition coverage | Normal, abnormal, robustness, and boundary conditions covered |

### Review Record Format

```yaml
review_id: RR-[next available number]
artifact_reviewed: [artifact ID]
artifact_type: [requirement | architecture | code | test_case]
reviewer: Agent C
date: [current date]
findings:
  - checklist_item: [R01, A03, C02, etc.]
    result: [conforming | nonconforming | observation | n/a]
    evidence: >
      [what you examined and what you found]
    problem_report: [PR-XXXX if nonconforming]
summary: >
  [X of Y checklist items conforming. Z nonconformances found.]
recommendation: [approve | revise and re-review]
```

Write to: `LAR/verification/review_records/[artifact_id]_review.yaml`

---

## Mode 2: Coverage

**Use when:** All tests have been executed. Analyze structural coverage results to identify and categorize gaps.

### Required Metric by DAL

| DAL | Required Coverage |
|-----|------------------|
| D | Statement Coverage (SC) |
| C | SC + Decision Coverage (DC) |
| B | SC + DC |
| A | SC + DC + MC/DC |

### Gap Categories

| Category | Definition | Action |
|----------|-----------|--------|
| **Dead Code** | Code element not traceable to any LLR | Generate PR to Agent B — must be removed |
| **Missing Test** | Code traceable to LLR but not exercised by any test | Write supplemental test case using Skill 2 |
| **Deactivated Code** | Traceable but unreachable in current configuration | Document justification; flag for human disposition |
| **Defensive Code** | Error handling for conditions analysis shows cannot occur | Write robustness test OR document analysis; flag for human |

### Coverage Analysis Report Format

```yaml
analysis_id: COV-[next available number]
dal: [A | B | C | D]
metrics:
  statement_coverage:
    achieved: [percentage]
    target: 100%
  decision_coverage:
    achieved: [percentage]
    target: 100%
  mcdc_coverage:
    achieved: [percentage]
    target: 100%
gaps:
  - file: [source file]
    line: [line number]
    element: [description of uncovered element]
    category: [dead_code | missing_test | deactivated | defensive]
    traces_to: [LLR ID or "none"]
    action: [what was done or what must be done]
    disposition: [removed | test_added | justified | pending_human]
status: [complete | gaps_remaining]
```

Write to: `LAR/verification/coverage_analysis/coverage_report.yaml`

---

## Mode 3: Traceability

**Use when:** End of each development phase, before any human gate, or when Agent D requests audit validation.

### The 10 Bidirectional Links

| Link | Forward | Reverse |
|------|---------|---------|
| 1 | Every SYS-XXXX has ≥1 child HLR | Every HLR traces to ≥1 SYS (or DERIVED) |
| 2 | Every HLR has ≥1 child LLR | Every LLR traces to ≥1 HLR (or DERIVED) |
| 3 | Every HLR allocated to ≥1 component | Every component implements ≥1 HLR |
| 4 | Every LLR implemented by ≥1 code element | Every code element traces to ≥1 LLR |
| 5 | Every LLR consistent with component design | Every design element traces to ≥1 LLR |
| 6 | Every SYS has ≥1 TC-SYS | Every TC-SYS traces to ≥1 SYS |
| 7 | Every HLR has ≥1 TC-HLR | Every TC-HLR traces to ≥1 HLR |
| 8 | Every LLR has ≥1 TC-LLR | Every TC-LLR traces to ≥1 LLR |
| 9 | Every TC has ≥1 test result | Every result traces to exactly 1 TC |
| 10 | Every code element has coverage data | Every coverage gap is analyzed and dispositioned |

### Validation Rules

- **No orphan requirements** — every requirement has a parent or is flagged DERIVED and dispositioned
- **No untested requirements** — every requirement has ≥1 test case
- **No untraceable code** — every code element traces to an LLR
- **No untraceable tests** — every test traces to a requirement
- **No unverified requirements** — every requirement has a passing test result
- **Bidirectional consistency** — if A→B exists then B→A exists
- **Derived dispositioned** — every derived requirement has a human disposition record

### Gate Readiness

| Gate | Required Links |
|------|---------------|
| Gate 2 (SYS approval) | Links 1, 6 complete |
| Gate 3 (Requirements baseline) | Links 1, 2, 7 complete |
| Gate 4 (Design baseline) | Links 3, 5 complete |
| Gate 5 (Code baseline) | Links 4, 8, 9, 10 complete |
| Gate 6 (Product baseline) | ALL links complete |

### Traceability Validation Report Format

```yaml
validation_id: TV-[next available number]
date: [current date]
scope: [all_links | specific links validated]
summary:
  total_links_checked: [count]
  valid: [count]
  gaps_found: [count]
gaps:
  - type: [orphan_requirement | untested_requirement | untraceable_code |
           untraceable_test | missing_reverse_link | unverified_requirement |
           derived_not_dispositioned]
    artifact: [artifact ID]
    link: [which of the 10 links]
    issue: >
      [specific description of the gap]
    action_required: >
      [who must do what to resolve this]
gate_readiness:
  gate_2: [ready | not_ready — list blocking gaps]
  gate_3: [ready | not_ready]
  gate_4: [ready | not_ready]
  gate_5: [ready | not_ready]
  gate_6: [ready | not_ready]
```

Write to: `LAR/verification/analysis_records/traceability_validation.yaml`
