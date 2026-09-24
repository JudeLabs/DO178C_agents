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
- Was independence (the human reviewer at the commit gate) exercised where required?
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

Write to: `LAR/AU-XXXX.yaml`

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

Write to: `LAR/NCR-XXXX.yaml`

---

## Mode 2: Compliance Matrix

**Use when:** Tracking objective satisfaction throughout the project, and before Gate 6.

> **Note on the standard:** DO-178C (RTCA/EUROCAE ED-12C) must be purchased for use on actual certification programs. Tables A-1 through A-10 in the standard define all lifecycle objectives. The representative objective set below covers the most commonly encountered objectives at DAL C/D and is sufficient to operate this skill for learning, prototyping, or framework evaluation. For a real certification program, replace this set with the full tables from the purchased standard.

### Representative Objective Set

The following objectives are drawn from Tables A-1 through A-10. The **Independence** column indicates whether the activity must be performed by someone independent of the developer (the human reviewer at the commit gate; Agents A–D are collectively the developer, so Agent C does not satisfy it). Independence is required at DAL A/B for most objectives; at DAL C/D it applies to selected objectives as noted.

#### Table A-1: Software Planning Process

| Obj | Description | DAL D | DAL C | DAL B | DAL A | Ind? | Responsible |
|-----|-------------|-------|-------|-------|-------|------|-------------|
| A1-1 | Software development and verification processes are defined | ✓ | ✓ | ✓ | ✓ | No | Agent D |
| A1-2 | Software lifecycle is defined | ✓ | ✓ | ✓ | ✓ | No | Agent D |
| A1-3 | Software lifecycle environment is defined | ✓ | ✓ | ✓ | ✓ | No | Agent D |
| A1-4 | Additional considerations are addressed | ✓ | ✓ | ✓ | ✓ | No | Agent D |
| A1-5 | Plans are consistent with each other | ✓ | ✓ | ✓ | ✓ | No | Agent D |

#### Table A-2: Software Development — Requirements

| Obj | Description | DAL D | DAL C | DAL B | DAL A | Ind? | Responsible |
|-----|-------------|-------|-------|-------|-------|------|-------------|
| A2-1 | High-level requirements are developed | ✓ | ✓ | ✓ | ✓ | No | Agent A |
| A2-2 | Derived HLR are defined and provided to system safety process | ✓ | ✓ | ✓ | ✓ | No | Agent A |
| A2-3 | Software architecture is developed | — | ✓ | ✓ | ✓ | No | Agent A |
| A2-4 | Low-level requirements are developed | — | ✓ | ✓ | ✓ | No | Agent A |
| A2-5 | Derived LLR are defined and provided to system safety process | — | ✓ | ✓ | ✓ | No | Agent A |
| A2-6 | Source code is developed | ✓ | ✓ | ✓ | ✓ | No | Agent B |
| A2-7 | Executable object code is produced | ✓ | ✓ | ✓ | ✓ | No | Agent B |

#### Table A-3: Verification of HLR

| Obj | Description | DAL D | DAL C | DAL B | DAL A | Ind? | Responsible |
|-----|-------------|-------|-------|-------|-------|------|-------------|
| A3-1 | HLR comply with system requirements | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A3-2 | HLR are accurate and consistent | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A3-3 | HLR are compatible with target hardware | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A3-4 | HLR are verifiable | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A3-5 | HLR conform to standards | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A3-6 | HLR are traceable to system requirements | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A3-7 | Algorithms are accurate | — | ✓ | ✓ | ✓ | Yes | Agent C |

#### Table A-4: Verification of Software Architecture

| Obj | Description | DAL D | DAL C | DAL B | DAL A | Ind? | Responsible |
|-----|-------------|-------|-------|-------|-------|------|-------------|
| A4-1 | Architecture is compatible with HLR | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A4-2 | Architecture is consistent | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A4-3 | Architecture is compatible with target hardware | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A4-4 | Partitioning integrity is confirmed | — | — | ✓ | ✓ | Yes | Agent C |

#### Table A-5: Verification of LLR

| Obj | Description | DAL D | DAL C | DAL B | DAL A | Ind? | Responsible |
|-----|-------------|-------|-------|-------|-------|------|-------------|
| A5-1 | LLR comply with HLR | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A5-2 | LLR are accurate and consistent | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A5-3 | LLR are compatible with target hardware | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A5-4 | LLR are verifiable | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A5-5 | LLR conform to standards | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A5-6 | LLR are traceable to HLR | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A5-7 | Algorithms are accurate | — | ✓ | ✓ | ✓ | Yes | Agent C |

#### Table A-6: Verification of Source Code

| Obj | Description | DAL D | DAL C | DAL B | DAL A | Ind? | Responsible |
|-----|-------------|-------|-------|-------|-------|------|-------------|
| A6-1 | Source code complies with LLR | ✓ | ✓ | ✓ | ✓ | Yes | Agent C |
| A6-2 | Source code complies with software architecture | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A6-3 | Source code is verifiable | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A6-4 | Source code conforms to standards | ✓ | ✓ | ✓ | ✓ | Yes | Agent C |
| A6-5 | Source code is traceable to LLR | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A6-6 | Source code is accurate and consistent | — | — | ✓ | ✓ | Yes | Agent C |

#### Table A-7: Testing of Outputs of Integration

| Obj | Description | DAL D | DAL C | DAL B | DAL A | Ind? | Responsible |
|-----|-------------|-------|-------|-------|-------|------|-------------|
| A7-1 | Executable object code complies with HLR (normal range) | ✓ | ✓ | ✓ | ✓ | Yes | Agent C |
| A7-2 | Executable object code is robust with respect to HLR | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A7-3 | Executable object code complies with LLR (normal range) | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A7-4 | Executable object code is robust with respect to LLR | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A7-5 | Executable object code is compatible with target hardware | — | ✓ | ✓ | ✓ | Yes | Agent C |

#### Table A-8: Structural Coverage

| Obj | Description | DAL D | DAL C | DAL B | DAL A | Ind? | Responsible |
|-----|-------------|-------|-------|-------|-------|------|-------------|
| A8-1 | Statement coverage is achieved | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A8-2 | Decision coverage is achieved | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A8-3 | Modified condition/decision coverage (MC/DC) is achieved | — | — | — | ✓ | Yes | Agent C |

#### Table A-9: Verification of Verification Results

| Obj | Description | DAL D | DAL C | DAL B | DAL A | Ind? | Responsible |
|-----|-------------|-------|-------|-------|-------|------|-------------|
| A9-1 | Test coverage of HLR is achieved | ✓ | ✓ | ✓ | ✓ | Yes | Agent C |
| A9-2 | Test coverage of LLR is achieved | — | ✓ | ✓ | ✓ | Yes | Agent C |
| A9-3 | Test coverage of software structure is achieved | — | ✓ | ✓ | ✓ | Yes | Agent C |

#### Table A-10: Software Configuration Management

| Obj | Description | DAL D | DAL C | DAL B | DAL A | Ind? | Responsible |
|-----|-------------|-------|-------|-------|-------|------|-------------|
| A10-1 | Configuration items are identified | ✓ | ✓ | ✓ | ✓ | No | Agent D |
| A10-2 | Baselines and traceability are established | ✓ | ✓ | ✓ | ✓ | No | Agent D |
| A10-3 | Problem reporting, change control, and review are established | ✓ | ✓ | ✓ | ✓ | No | Agent D |
| A10-4 | Release of configuration items is controlled | ✓ | ✓ | ✓ | ✓ | No | Agent D |
| A10-5 | Archive, retrieval, and protection are established | ✓ | ✓ | ✓ | ✓ | No | Agent D |
| A10-6 | Software load control is established | ✓ | ✓ | ✓ | ✓ | No | Agent D |
| A10-7 | Software lifecycle environment control is established | ✓ | ✓ | ✓ | ✓ | No | Agent D |

---

### Procedure

**Step 1: Load objective set for assigned DAL**

Use the representative objective tables above. Mark each row as applicable or not applicable for the project. The column for the assigned DAL indicates whether the objective is required (✓) or not required (—) at that level.

**Step 2: For each applicable objective, identify three things:**

1. **Plan reference** — which section of which plan describes how this objective will be satisfied
2. **Activity** — what was actually done to satisfy it
3. **Evidence** — the specific LAR artifact that proves it (e.g., `LAR/RR-0007.yaml`)

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

Write to: `LAR/compliance_matrix.yaml`

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

Write to: `LAR/SAS.yaml`

The SAS is the document the human presents at Gate 6 and, in a DO-178C program, submits to the certification authority.
