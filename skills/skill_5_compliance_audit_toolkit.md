# Skill 5: Compliance & Audit Toolkit

> **Note on the standard:** DO-178C (RTCA DO-178C / EUROCAE ED-12C) is a copyrighted document and must be purchased from RTCA or EUROCAE. This skill does not reproduce its objective tables. Any objective references below are paraphrased illustrations of the compliance-matrix format; a real program must populate the matrix from its own copy of the standard.

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

This mode maintains `LAR/compliance_matrix.yaml`: one entry per DO-178C objective, each recording the software levels at which it applies, whether independence is required, which agent owns it, the plan section that governs it, the activity performed, and the evidence that proves it. The objectives themselves are not supplied by this skill. They come from the program's own copy of the standard.

### Compliance Matrix Template

```yaml
dal: [A | B | C | D]
date: [current date]
objective_source: >
  [the program's copy of DO-178C / ED-12C: edition and document control reference]
total_objectives: [count]
satisfied: [count]
open: [count]
not_applicable: [count]
objectives:
  - table: [Annex A table identifier, as given in the program's copy of the standard]
    number: [objective identifier, as given in the program's copy of the standard]
    description: >
      [objective summary, taken from the program's copy of the standard]
    required_at_dal: [true | false (from the standard's applicability for the assigned DAL)]
    independence_required: [true | false (from the standard, for the assigned DAL)]
    responsible: [Agent A | Agent B | Agent C | Agent D]
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

Where `independence_required` is true, the agent that performed the activity does not satisfy it. Agents A–D are collectively the developer, so independence is satisfied only by the human reviewer at the commit gate, and the evidence must show that review.

### Procedure

**Step 1: Load the objective set for the assigned DAL from the program's own copy of the standard**

Populate the matrix from the Annex A objective tables in the program's purchased copy of DO-178C / ED-12C. The objectives are supplied by the user or the program (for example as a list or file transcribed from the standard under the program's licence); this skill does not contain them. If no objective set has been provided, stop and request it. Do not reconstruct objectives from memory or from the illustrative example below.

For each supplied objective, record its table, number, and description, and from the standard's applicability for the assigned DAL set `required_at_dal` and `independence_required`. Assign `responsible` to the agent whose activity produces the evidence. Mark objectives not required at the assigned DAL as `not_applicable`.

**Step 2: For each applicable objective, identify three things:**

1. **Plan reference** — which section of which plan describes how this objective will be satisfied
2. **Activity** — what was actually done to satisfy it
3. **Evidence** — the specific LAR artifact that proves it (e.g., `LAR/RR-0007.yaml`)

Every evidence reference must point to an actual file in the LAR.

**Step 3: Determine status for each objective**

- **SATISFIED** — plan reference exists, activity was performed, evidence artifact exists and is complete (and, where independence is required, the evidence shows the human reviewer's review)
- **OPEN** — one or more of the above is missing — record what's missing
- **NOT APPLICABLE** — objective does not apply at this DAL or for this project — record justification

**Step 4: Identify gaps**

For every OPEN objective:
- What is missing (plan, activity, or evidence)?
- Which agent is responsible for resolving it?
- Path to resolution?

**Step 5: Write the compliance matrix**

Fill in the template above and write it to `LAR/compliance_matrix.yaml`.

### Illustrative Example: Filling the Template

> **Illustration of the format only.** The rows below are a small paraphrased sample, chosen to show the structure of objective-level tracking: applicability that varies by DAL, independence that applies at some levels and not others, and one owning agent per objective. They are not the standard's wording, not a complete table, and not a basis for compliance. A real matrix is built from the program's own copy of the standard (Step 1).

| Table | Obj | Paraphrased objective | DAL D | DAL C | DAL B | DAL A | Independence required at | Responsible |
|-------|-----|-----------------------|-------|-------|-------|-------|--------------------------|-------------|
| A-2 | A2-1 | High-level requirements are developed | ✓ | ✓ | ✓ | ✓ | — | Agent A |
| A-2 | A2-4 | Low-level requirements are developed | — | ✓ | ✓ | ✓ | — | Agent A |
| A-2 | A2-6 | Source code is developed | ✓ | ✓ | ✓ | ✓ | — | Agent B |
| A-2 | A2-7 | Executable object code is produced | ✓ | ✓ | ✓ | ✓ | — | Agent B |
| A-3 | A3-1 | HLR comply with system requirements | — | ✓ | ✓ | ✓ | A, B | Agent C |
| A-3 | A3-4 | HLR are verifiable | — | ✓ | ✓ | ✓ | A, B | Agent C |
| A-3 | A3-6 | HLR are traceable to system requirements | — | ✓ | ✓ | ✓ | A, B | Agent C |
| A-10 | A10-1 | Configuration items are identified | ✓ | ✓ | ✓ | ✓ | — | Agent D |
| A-10 | A10-3 | Problem reporting, change control, and review are established | ✓ | ✓ | ✓ | ✓ | — | Agent D |

One of these rows as a matrix entry, for a DAL B program:

```yaml
  - table: A-3
    number: A3-1
    description: >
      HLR comply with system requirements (paraphrased)
    required_at_dal: true
    independence_required: true
    responsible: Agent C
    plan_reference: SVP §4.2 (requirements review)
    activity: >
      Agent C drafted the HLR review record against SYS requirements; the
      human reviewer reviewed and signed it at the commit gate.
    evidence:
      - LAR/RR-0007.yaml
    status: satisfied
    gap: ""
```

At DAL C or D the same objective would carry `independence_required: false`; at DAL D it would be `not_applicable`.

### Annex A Tables Not Sampled Above

The standard's remaining objective tables are summarised here by subject only. Obtain their objectives from the program's copy of DO-178C / ED-12C.

- **Table A-1** — software planning: definition of the lifecycle processes, environment, and plans, and their mutual consistency.
- **Table A-4** — verification of the software architecture.
- **Table A-5** — verification of the low-level requirements.
- **Table A-6** — verification of the source code.
- **Table A-7** — testing of the integrated executable object code.
- **Table A-8** — structural coverage analysis.
- **Table A-9** — verification of the verification process results.

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
