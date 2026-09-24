# Low-Level Requirements (LLR-XXXX)

**Produced by:** Agent A (Development Engineer) using Skill 1 (Requirement Writer)  
**Reviewed by:** Agent C (Verification Engineer)  
**Baselined at:** Requirements Baseline (Gate 3)

Implementation-level requirements specifying exact algorithms, data types, ranges, initial values, error handling, timing, and function signatures. Every line of source code must trace to at least one LLR. Code that does not trace to an LLR is dead code and must be removed.

Agent C writes executable unit tests (TC-LLR-XXXX) against these requirements before Agent B writes code. Agent B's sole objective is to write code that passes those tests.

---

## Naming Convention

`LLR-XXXX-YYY.yaml` where XXXX matches the parent HLR number and YYY is a sequential sub-number (e.g., `LLR-0101-001.yaml`)

---

## Format

```yaml
id: LLR-XXXX-YYY
title: [concise name]
description: >
  The software shall [exact implementation-level behavior].
rationale: [why this LLR exists — which HLR behavior it implements]
parent: HLR-XXXX
type: [functional | performance | interface | safety | derived]
dal: [A | B | C | D]
component: [architecture component that implements this LLR]
algorithm: >
  [exact algorithm, state machine, equation, or decision table]
data_types:
  - name: [variable name]
    type: [FLOAT32 | UINT8 | BOOL | etc.]
    range: [min to max]
    initial_value:
    units:
error_handling: >
  [what constitutes invalid input and what the response must be]
timing:
  rate_hz:
  max_execution_time_us:
function_signature: >
  [return_type function_name(param_type param_name, ...)]
verification_method: test
acceptance_criteria: >
  [specific measurable pass/fail criteria]
safety_impact: [yes | no]
code_references: []   # source file(s) and function(s) implementing this LLR
test_cases: []        # TC-LLR-XXXX IDs verifying this LLR
status: [draft | reviewed | approved | baselined]
```

---

## Example

```yaml
id: LLR-0101-001
title: Airspeed Difference Threshold Comparison
description: >
  The software shall compute the absolute difference between
  primary_airspeed_knots (FLOAT32) and secondary_airspeed_knots (FLOAT32)
  and set the comparison_exceeds_threshold flag (BOOL) to TRUE when the
  absolute difference strictly exceeds 10.0 knots.
rationale: Implements the threshold comparison step of HLR-0101.
parent: HLR-0101
type: functional
dal: B
component: airspeed_monitor
algorithm: >
  diff = |primary_airspeed_knots - secondary_airspeed_knots|
  comparison_exceeds_threshold = (diff > 10.0)
  Note: 10.0 exactly does NOT set the flag ("exceeds" is exclusive)
data_types:
  - name: primary_airspeed_knots
    type: FLOAT32
    range: -100.0 to 1000.0
    units: knots
  - name: secondary_airspeed_knots
    type: FLOAT32
    range: -100.0 to 1000.0
    units: knots
  - name: comparison_exceeds_threshold
    type: BOOL
    initial_value: FALSE
error_handling: >
  If either input has invalid SSM status, comparison_exceeds_threshold
  shall be set to FALSE and airspeed_data_invalid flag set to TRUE.
  See LLR-0101-004 for invalid SSM handling.
timing:
  rate_hz: 50
  max_execution_time_us: 10
function_signature: >
  void airspeed_compare(float32_t primary, float32_t secondary,
                        bool_t *exceeds_threshold)
verification_method: test
acceptance_criteria: >
  Given primary=250.0 and secondary=260.001, exceeds_threshold == TRUE.
  Given primary=250.0 and secondary=260.0, exceeds_threshold == FALSE.
safety_impact: yes
code_references: []
test_cases: []
status: draft
```
