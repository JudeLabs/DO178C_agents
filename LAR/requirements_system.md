# System Requirements (SYS-XXXX)

**Produced by:** Agent A (Development Engineer) using Skill 1 (Requirement Writer)  
**Reviewed by:** Agent C (Verification Engineer), then Human (Gate 2)  
**Baselined at:** Requirements Baseline (Gate 3)

Top-level requirements derived from the system concept and safety assessment. Each requirement defines what the software must do at the operational level — modes, functions, inputs, outputs, timing, and failure behavior. Every SYS requirement must be traced to at least one HLR (Link 1) and at least one system-level test case (Link 6).

---

## Naming Convention

`SYS-XXXX.yaml` where XXXX is a zero-padded sequential number (e.g., `SYS-0001.yaml`)

---

## Format

```yaml
id: SYS-XXXX
title: [concise name]
description: >
  The system shall [single atomic behavior with measurable criteria].
rationale: [why this requirement exists — operational or safety driver]
parent: [ARP4754A system requirement ID, or DERIVED]
type: [functional | performance | interface | safety | derived]
dal: [A | B | C | D]
failure_condition: [description of failure condition if safety requirement]
failure_condition_class: [catastrophic | hazardous | major | minor | no_effect]
verification_method: [test | analysis | review | demonstration]
acceptance_criteria: >
  [specific measurable pass/fail criteria]
safety_impact: [yes | no | PENDING REVIEW if derived]
allocated_to: []  # HLR IDs that implement this requirement
test_cases: []    # TC-SYS IDs that verify this requirement
status: [draft | reviewed | approved | baselined]
```

---

## Example

```yaml
id: SYS-0001
title: Airspeed Sensor Failure Detection
description: >
  The system shall detect a failure of the airspeed measurement function
  within 200 milliseconds of the failure occurring and annunciate the
  failure condition to the flight crew.
rationale: >
  Loss of accurate airspeed indication is a hazardous failure condition.
  Crew awareness within 200ms allows timely corrective action.
parent: ARP4754A-SYS-0042
type: safety
dal: B
failure_condition: Loss of airspeed measurement function
failure_condition_class: hazardous
verification_method: test
acceptance_criteria: >
  When airspeed sensor failure is induced, failure flag is set and
  crew alert is generated within 200 milliseconds.
safety_impact: yes
allocated_to: []
test_cases: []
status: draft
```

---

## Also in this directory

- `safety_assessment.yaml` — FHA results, failure condition classifications, probability targets
- `dal_allocation.yaml` — mapping of each software function to its assigned DAL
- `system_architecture.yaml` — hardware/software partitioning, external interfaces, system boundaries
