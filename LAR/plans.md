# Plans

**Produced by:** Agent D (Process & Compliance)  
**Reviewed by:** Human (Gate 1)  
**Baselined at:** Planning Baseline (Gate 1)

The five plans required by DO-178C. All plans must be established **before** the activities they govern. No development or verification begins until Gate 1 is passed with all five plans approved.

---

## PSAC — Plan for Software Aspects of Certification

**File:** `PSAC.yaml`

The master plan. Describes the software being developed, the hardware environment, the certification approach, how compliance will be demonstrated, and the schedule for milestone reviews. This is the document that tells the certification authority what you are going to do and how you will prove you did it.

```yaml
# PSAC.yaml
document_id: PSAC-001
title: Plan for Software Aspects of Certification
version: draft
date:
software_overview:
  name:
  description:
  dal:
  hardware_environment:
certification_approach:
  compliance_strategy:
  deviations:
lifecycle_overview:
  phases: []
  human_gates: []
tool_environment: []
schedule: []
status: draft
```

---

## SDP — Software Development Plan

**File:** `SDP.yaml`

How development will happen. Requirements methods, design methods, coding standards (language, complexity limits, banned constructs), integration approach, development environment.

```yaml
# SDP.yaml
document_id: SDP-001
title: Software Development Plan
version: draft
date:
development_methods:
  requirements_method:
  design_method:
  integration_approach:
coding_standards:
  language:
  standard:
  banned_constructs: []
  complexity_limits:
development_environment:
  compiler:
  compiler_version:
  compiler_flags:
  os:
  target_hardware:
status: draft
```

---

## SVP — Software Verification Plan

**File:** `SVP.yaml`

How verification will happen. Review procedures, test strategy at each level, structural coverage approach (which metric at which DAL), independence requirements, tool qualification needs, regression testing strategy.

```yaml
# SVP.yaml
document_id: SVP-001
title: Software Verification Plan
version: draft
date:
verification_methods:
  reviews: []
  analysis: []
  testing: []
coverage_requirements:
  dal:
  statement_coverage: required
  decision_coverage:
  mcdc_coverage:
independence_requirements:
  development_verification_separation:
regression_strategy:
tool_qualification: []
status: draft
```

---

## SCMP — Software Configuration Management Plan

**File:** `SCMP.yaml`

How artifacts are controlled. Configuration identification scheme, baseline management approach, change control procedures, problem reporting process, environment control, archive procedures.

```yaml
# SCMP.yaml
document_id: SCMP-001
title: Software Configuration Management Plan
version: draft
date:
identification_scheme:
  requirements_prefix: SYS / HLR / LLR
  test_case_prefix: TC-SYS / TC-HLR / TC-LLR
  change_request_prefix: CR
  problem_report_prefix: PR
baseline_types:
  - planning
  - requirements
  - design
  - code
  - product
change_control_procedure:
problem_reporting_procedure:
archive_procedure:
status: draft
```

---

## SQAP — Software Quality Assurance Plan

**File:** `SQAP.yaml`

How compliance is monitored. QA authority and independence, review/audit schedule, process compliance monitoring, nonconformance handling, transition criteria between phases.

```yaml
# SQAP.yaml
document_id: SQAP-001
title: Software Quality Assurance Plan
version: draft
date:
qa_authority: Agent D
independence: Agent D is part of the developer side; DO-178C independence is provided by the human reviewer at the commit gate
audit_schedule: []
transition_criteria:
  gate_1: []
  gate_2: []
  gate_3: []
  gate_4: []
  gate_5: []
  gate_6: []
nonconformance_handling:
status: draft
```
