# Skill 5: Compliance & Audit Toolkit

**Used by:** Agent D (Process & Compliance)

## Purpose

Three-mode toolkit covering process compliance audits, DO-178C objective compliance matrix maintenance, and Software Accomplishment Summary (SAS) compilation. Provides the evidence trail that demonstrates every lifecycle objective was satisfied.

---

## Mode 1: Audit

**Use when:** At each phase transition and when QA compliance checks are needed.

### Procedure

**Step 1: Identify audit scope**
Determine which plan governs this phase:
- Planning phase → SQAP
- Development phase → SDP
- Verification phase → SVP
- CM activities → SCMP

**Step 2: Extract required process steps from the applicable plan**

For each required step, verify:
- Was the activity performed? (evidence artifact exists)
- Does the artifact conform to its defined standard?
- Was independence maintained where required?
- Were transition criteria satisfied before phase entry?

**Step 3: Evaluate each process step**

Record one of:
- **CONFORMING** — activity performed per plan with evidence
- **NONCONFORMING** — deviation found — record specific deficiency
- **OBSERVATION** — not a compliance issue but worth noting
- **NOT APPLICABLE** — state justification

**Step 4: Generate audit record**

```yaml
audit_id: AU-[next available number]
date: [current date]
auditor: Agent D
scope: >
  [which phase, which plan, which activities audited]
findings:
  - process_step: [description from plan]
    result: [conforming | nonconforming | observation | n/a]
    evidence_examined: >
      [what artifact was reviewed as evidence]
    finding: >
      [what was found — conforming statement or specific deficiency]
    ncr: [NCR-XXXX if nonconforming]
summary: >
  [X of Y steps conforming. Z nonconformances issued.]
recommendation: [approve transition | hold — resolve NCRs first]
```

Write to: `LAR/cm/audit_records/AU-XXXX.yaml`

**Step 5: Generate Nonconformance Reports for deviations**

```yaml
id: NCR-[next available number]
date: [current date]
source_audit: [AU-XXXX]
finding: >
  [specific process deviation]
affected_plan_section: [section reference]
responsible_agent: [who must correct]
corrective_action_required: >
  [what must be done to achieve conformance]
status: open
```

Write to: `LAR/cm/nonconformance_reports/NCR-XXXX.yaml`

---

## Mode 2: Compliance Matrix

**Use when:** Tracking objective satisfaction throughout the project, and before Gate 6.

### Procedure

**Step 1: Load DO-178C objective tables for assigned DAL**

Tables A-1 through A-10 define all objectives. The number of applicable objectives varies by DAL:
- DAL D: ~26 objectives
- DAL C: ~44 objectives
- DAL B: ~57 objectives
- DAL A: ~71 objectives

**Step 2: For each applicable objective, identify three things:**

1. **Plan reference** — which section of which plan describes how this objective will be satisfied
2. **Activity** — what was actually done to satisfy it
3. **Evidence** — the specific LAR artifact that proves it (e.g., `LAR/verification/review_records/RR-007.yaml`)

Every evidence reference must point to an actual file in the LAR.

**Step 3: Determine status for each objective**

- **SATISFIED** — plan reference exists, activity was performed, evidence artifact exists and is complete
- **OPEN** — one or more of the above is missing — record what's missing
- **NOT APPLICABLE** — objective does not apply at this DAL or for this project — record justification

**Step 4: Identify gaps**

For every OPEN objective:
- What is missing (plan, activity, or evidence)?
- Which agent is responsible for resolving it?
- Path to resolution?

**Step 5: Generate compliance matrix**

```yaml
dal: [A | B | C | D]
date: [current date]
total_objectives: [count]
satisfied: [count]
open: [count]
not_applicable: [count]
objectives:
  - table: [A-1 through A-10]
    number: [objective number]
    description: >
      [objective text]
    plan_reference: [plan section]
    activity: >
      [what was done]
    evidence:
      - [LAR file path]
    status: [satisfied | open | not_applicable]
    gap: >
      [if open: what is missing and who must resolve it]
      [if n/a: justification]
```

Write to: `LAR/sas/compliance_matrix.yaml`

---

## Mode 3: SAS Compilation

**Use when:** Preparing for Gate 6 (Product Baseline).

The Software Accomplishment Summary is the definitive document that summarizes the entire software lifecycle and demonstrates all objectives have been met. It must be complete, accurate, and honest — do not overstate compliance or omit known issues.

### Inputs Required

Collect from the LAR before beginning:
- All five plans (PSAC, SDP, SVP, SCMP, SQAP)
- Compliance matrix (from Mode 2)
- All review records, test results, coverage analysis
- All audit records and NCR dispositions
- All CR and PR records
- All baseline records
- Environment configuration records

### SAS Sections

**Section 1 — Software Overview**
- Software identification (name, version, part number)
- Software description (what it does, one paragraph)
- Hardware environment (target processor, memory, I/O)
- Software architecture summary (component list, key design decisions)

**Section 2 — Certification Considerations**
- Assigned DAL and justification
- Special conditions or deviations from standard process
- Previously developed software used (if any)
- COTS components used (if any)
- Tool qualification summary (tools used, qualification status, TQL level)

**Section 3 — Software Lifecycle Summary**
- Development process summary (methods as defined in SDP)
- Verification process summary (methods as defined in SVP)
- Configuration management summary
- Quality assurance summary
- Key metrics: requirements count (SYS, HLR, LLR), LOC, test count, coverage achieved

**Section 4 — Deviations**
- Any deviations from the plans, with justification
- Any objectives satisfied by alternative means
- If none: state "No deviations from approved plans"

**Section 5 — Compliance Summary**
- Reference the compliance matrix
- State: "[X] of [Y] applicable objectives satisfied"
- If any OPEN: explain why and the plan to close them

**Section 6 — Open Problem Reports**
- List any open PRs with disposition
- Ideally this list is empty. If not, each open PR must have documented justification for release acceptability.

### Pre-Submission Checklist

Before finalizing the SAS:

- [ ] Every section is populated (no TBD, no placeholders)
- [ ] Every evidence reference points to a real LAR artifact
- [ ] Compliance matrix shows all objectives satisfied
- [ ] No open PRs without justification
- [ ] No open NCRs
- [ ] All baselines established
- [ ] Software Conformity Review (Mode 1 audit of full lifecycle) is complete

### SAS Output

Write to: `LAR/sas/software_accomplishment_summary.yaml`

The SAS is the document the human presents at Gate 6 and, in a DO-178C program, submits to the certification authority.
