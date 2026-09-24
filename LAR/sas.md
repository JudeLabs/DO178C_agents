# Certification Artifacts

**Produced by:** Agent D (Process & Compliance)  
**Reviewed by:** Human (Gate 6)  
**Purpose:** Definitive evidence package for certification authority or quality review

---

## compliance_matrix.yaml

Every applicable DO-178C objective mapped to the activity that satisfies it, the artifact that provides evidence, and the current status. Maintained continuously by Agent D; finalized at Gate 6.

```yaml
dal: [A | B | C | D]
date:
total_objectives:
satisfied:
open:
not_applicable:
objectives:
  - table: [A-1 through A-10]
    number:
    description: >
    plan_reference: [plan section]
    activity: >
      [what was done to satisfy this objective]
    evidence:
      - [LAR file path to artifact proving this objective]
    status: [satisfied | open | not_applicable]
    gap: >
      [if open: what is missing and who resolves it]
      [if n/a: justification for non-applicability]
```

---

## software_accomplishment_summary.yaml

The definitive certification document. Summarizes the entire software lifecycle and demonstrates all objectives have been met. Submitted to the certification authority (or equivalent review authority) at Gate 6.

**This document must be complete and honest. Do not overstate compliance or omit known issues.**

```yaml
document_id: SAS-001
version: draft
date:

section_1_software_overview:
  name:
  version:
  part_number:
  description: >
    [one paragraph: what the software does]
  hardware_environment:
    processor:
    memory:
    io:
  architecture_summary: >
    [component list and key design decisions]

section_2_certification_considerations:
  dal:
  dal_justification: >
  special_conditions: []
  previously_developed_software: []
  cots_components: []
  tool_qualification_summary: []

section_3_lifecycle_summary:
  development_process_summary: >
  verification_process_summary: >
  configuration_management_summary: >
  quality_assurance_summary: >
  metrics:
    sys_requirement_count:
    hlr_count:
    llr_count:
    derived_requirement_count:
    source_loc:
    test_case_count:
    statement_coverage_achieved:
    decision_coverage_achieved:
    mcdc_coverage_achieved:

section_4_deviations:
  deviations: []
  # If none: "No deviations from approved plans"

section_5_compliance_summary:
  compliance_matrix_reference: sas/compliance_matrix.yaml
  objectives_satisfied:
  objectives_total:
  open_objectives: []

section_6_open_problem_reports:
  open_prs: []
  # Ideally empty. Each open PR requires disposition justification.

status: draft
```

---

## Pre-Gate-6 Checklist

Agent D verifies all of the following before recommending Gate 6:

- [ ] Every section of the SAS is populated — no TBD, no placeholders
- [ ] Every evidence reference in the compliance matrix points to a real LAR artifact
- [ ] Compliance matrix shows all applicable objectives satisfied
- [ ] No open PRs without written disposition justification
- [ ] No open NCRs
- [ ] All five baselines established (planning, requirements, design, code, product)
- [ ] Software Conformity Review (full lifecycle audit) complete
