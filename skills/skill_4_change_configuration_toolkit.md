# Skill 4: Change & Configuration Toolkit

**Used by:** Agent D (Process & Compliance)

## Purpose

Three-mode toolkit for impact analysis, baseline management, and CR/PR lifecycle tracking. Enforces the fundamental CM invariant: no baselined artifact is modified without a formal Change Request.

## Critical Rule

> No artifact may be modified after baselining without a formal Change Request processed through this skill. **This rule has no exceptions.** Even "small" or "obvious" fixes require a CR if the artifact is baselined.

---

## Mode 1: Impact Analysis

**Use when:** Any change is proposed to any artifact.

### Procedure

**Step 1: Identify the change target**
- Which artifact is changing (ID, type)
- What is changing (specific modification)
- Why it is changing (PR number, CR number, or human direction)

**Step 2: Trace upstream**
Follow traceability links upward. For each parent artifact ask: does the parent need to change too?
- Code → LLR → HLR → SYS

**Step 3: Trace downstream**
Follow traceability links downward. Every downstream artifact must be evaluated.
- SYS → HLR → LLR → Code

**Step 4: Identify affected tests**
- Which test cases verify the changed artifact and its downstream children?
- Which tests need updated inputs/expected results?
- Which tests must be re-run for regression (even if unchanged)?
- Rule: **when in doubt, re-run.**

**Step 5: Identify affected coverage**
- Any code change requires coverage re-analysis
- New code may introduce uncovered paths

**Step 6: Identify affected baselines**
- Requirements Baseline: if any SYS, HLR, or LLR changed
- Design Baseline: if architecture or component design changed
- Code Baseline: if source code changed
- Product Baseline: if any of the above changed

**Step 7: Determine human approvals needed**
- CR approval before implementation
- Gate re-entry if applicable
- Re-baseline authorization

### Impact Analysis Report Format

```yaml
change_request: CR-[number]
change_target: [artifact ID]
change_description: >
  [what is changing and why]
upstream_impact:
  - artifact: [parent ID]
    impact: [none | must update — description]
downstream_impact:
  requirements:
    - [list of requirement IDs that must be updated]
  design:
    - [list of design elements that must be updated]
  code:
    - [list of source files that must be updated]
  tests:
    must_update:
      - [test IDs where inputs or expected results change]
    must_rerun:
      - [test IDs for regression]
  coverage:
    - [re-analysis required: yes/no]
baselines_affected:
  - [list of baselines that must be re-established]
human_approvals_needed:
  - [list of approvals required]
re_verification_plan:
  - [ordered list of re-verification activities]
```

Write to: `LAR/cm/change_requests/CR-XXXX-impact.yaml`

---

## Mode 2: Baseline Management

**Use when:** A human gate has been passed and artifacts need to be formally baselined.

### Baseline Types

| Baseline | Gate | Contents |
|----------|------|----------|
| Planning | Gate 1 | All five plans approved |
| Requirements | Gate 3 | All HLR and LLR reviewed and approved |
| Design | Gate 4 | Architecture reviewed and approved |
| Code | Gate 5 | All code reviewed, tests passing, coverage met |
| Product | Gate 6 | All verification complete, SAS ready |

### Prerequisites Before Baselining

All of the following must be true before establishing a baseline:

- [ ] Human gate approval record exists in the LAR
- [ ] All required artifacts for this baseline type exist
- [ ] All artifacts are in "reviewed" or "approved" status
- [ ] All Problem Reports are closed or dispositioned (deferred with justification)
- [ ] Agent C's traceability validation shows no blocking gaps for this gate
- [ ] Agent D's QA audit for this phase is complete with no open nonconformances

**If any prerequisite is not met, do NOT establish the baseline.**

### Baseline Record Format

```yaml
baseline_id: BL-[type]-[sequential number]
baseline_type: [planning | requirements | design | code | product]
date: [current date]
human_authorization: [reference to gate approval record]
included_artifacts:
  - artifact_id: [ID]
    version: [version number or hash]
    file_path: [path in LAR]
    status: [approved]
open_problem_reports:
  deferred:
    - pr_id: [PR-XXXX]
      justification: >
        [why this PR is acceptable to defer]
  count_closed: [number of PRs closed before baseline]
environment:
  compiler: [version]
  compiler_settings: [optimization level, warnings, flags]
  os: [version]
  target_hardware: [description]
  test_tools: [tool names and versions]
notes: >
  [any relevant context about this baseline]
```

Write to: `LAR/cm/baselines/BL-[type]-[number].yaml`

After establishing the baseline, append to `LAR/cm/cm_records.yaml`:
```yaml
event: baseline_established
baseline_id: [BL-ID]
date: [date]
artifacts_locked: [count]
```

---

## Mode 3: Change / Problem Report Tracking

**Use when:** A CR or PR is created, updated, or queried.

### Change Request Lifecycle

```
OPEN → ANALYZED → APPROVED → IMPLEMENTED → VERIFIED → CLOSED
```

#### CR Record Format

```yaml
id: CR-[next available number]
date_opened: [current date]
requested_by: [agent or human]
change_target: [artifact ID]
change_description: >
  [what needs to change and why]
priority: [critical | high | medium | low]
status: open
history:
  - date: [date]
    action: opened
    by: [who]
```

Write to: `LAR/cm/change_requests/CR-XXXX.yaml`

#### CR Steps

1. **Create** — log the CR
2. **Analyze** — run Mode 1 and attach impact analysis; update status to "analyzed"
3. **Human approval** — present CR + impact analysis; if approved update to "approved"; if rejected close as "rejected"
4. **Track implementation** — record each artifact updated; update to "implemented" when all done
5. **Track verification** — record re-review, re-test, re-coverage; update to "verified" when complete
6. **Close** — update status, record new baseline if applicable

### Problem Report Lifecycle

```
OPEN → ANALYZED → CORRECTED → VERIFIED → CLOSED
```

PRs are generated by Agent C. Agent D routes them to the responsible agent:

| Affected Artifact | Responsible Agent |
|------------------|------------------|
| Requirement | Agent A |
| Source code | Agent B |
| Test case | Agent C (self-correction) |
| Process | Agent D (self-correction) |

### Status Report Format

At any time, generate a summary and append to `LAR/cm/cm_records.yaml`:

```yaml
report_date: [date]
change_requests:
  open: [count]
  analyzed: [count]
  approved: [count]
  implemented: [count]
  verified: [count]
  closed: [count]
problem_reports:
  open: [count]
  corrected: [count]
  verified: [count]
  closed: [count]
blocking_next_gate:
  - [list of open CRs/PRs blocking gate transition]
```
