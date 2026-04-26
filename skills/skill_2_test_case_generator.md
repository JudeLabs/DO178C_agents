# Skill 2: Test Case Generator

**Used by:** Agent C (Verification Engineer)

## Purpose

Generates comprehensive test cases from requirements at any level (SYS, HLR, LLR). Applies all applicable test techniques to each requirement. Generates Problem Reports when requirements are ambiguous or incomplete rather than assuming behavior.

## Critical Rule

> If a requirement does not specify behavior for a condition you are trying to test, **DO NOT invent expected behavior**. Instead, generate a Problem Report. This is the most important function of this skill — finding gaps in requirements before code is written.

## Skill Prompt

```
Use this skill every time you need to create test cases from a
requirement at any level (SYS, HLR, LLR). You must apply ALL
applicable techniques to each requirement — do not stop at the
first test case.

Step 1: Parse the requirement
Extract from the requirement text:
  INPUTS: Every input parameter with data type, unit, range, source
  OUTPUTS: Every output parameter with expected value, type, destination
  THRESHOLDS: Every numeric boundary or threshold value
  TIMING: Every timing constraint (rate, latency, duration, deadline)
  STATES: Every mode, state, or condition referenced
  ERROR CONDITIONS: Every error or failure mentioned
  PERSISTENCE: Any "consecutive," "sustained," or duration-based conditions

If any of these cannot be extracted because the requirement is
ambiguous, stop and generate a Problem Report (see Step 8).

Step 2: Determine test level
  SYS requirement → test ID prefix: TC-SYS-XXXX
  HLR → test ID prefix: TC-HLR-XXXX
  LLR → test ID prefix: TC-LLR-XXXX
  Test IDs follow the pattern: TC-[level]-[req number]-[sequence]
  Example: TC-HLR-0101-01, TC-HLR-0101-02, etc.

Step 3: Generate NORMAL OPERATION test(s)
  Always generate at least one.
  Use nominal/mid-range values for all inputs.
  Set all preconditions to their normal/expected state.
  Expected result comes directly from the requirement's shall statement.
  Include timing verification if the requirement specifies timing.

Step 4: Generate BOUNDARY VALUE tests
  For EVERY numeric parameter:
    AT minimum valid value
    AT minimum + 1
    AT nominal / mid-range
    AT maximum - 1
    AT maximum valid value
  For thresholds (e.g., "exceeds 10.0 knots"):
    AT threshold exactly — is threshold inclusive or exclusive?
    AT threshold - smallest increment
    AT threshold + smallest increment
  For counters (e.g., "4 consecutive samples"):
    AT count - 1
    AT exactly count
    AT count + 1
  For timing (e.g., "within 200 milliseconds"):
    At timing limit
    Just before limit
    What happens if exceeded? (if unspecified → Problem Report)

Step 5: Generate EQUIVALENCE CLASS tests
  For EVERY input parameter:
    VALID NORMAL: value well within valid range
    VALID BOUNDARY: value at edge of valid range
    INVALID BELOW: value below valid minimum → test error behavior or PR
    INVALID ABOVE: value above valid maximum → test error behavior or PR
    SPECIAL VALUES: 0, -1, MAX_INT, MIN_INT, NaN, infinity, etc.

Step 6: Generate STATE TRANSITION tests (if applicable)
  For every VALID transition:
    Start in state A, apply trigger, verify transition to state B
    Verify outputs that change during transition
    Verify timing of transition if specified
  For every INVALID transition:
    Start in state A, apply non-triggering input
    Verify system remains in state A
  For persistence / hysteresis:
    Test at N-1 then break — verify no transition
    Test at exactly N — verify transition occurs

Step 7: Generate ABNORMAL / ROBUSTNESS tests
  Always generate at least two.
  MISSING INPUT: What happens when expected input is absent?
  STALE INPUT: What happens when input stops updating?
  CORRUPTED INPUT: Valid status but physically impossible values
  TIMING STRESS: Maximum rate inputs, simultaneous events
  RELATED FAILURES: What if a dependency has already failed?
  → Test or generate PR for each unspecified behavior.

Step 8: Generate FAILURE CONDITION tests (safety requirements only)
  Only for requirements with type = safety or safety_impact = yes.
  Stimulate the failure condition
  Verify: detection occurs, safe state entered, correct annunciation
  Test multiple simultaneous failures
  Test recovery after failure clears

Step 9: Generate Problem Reports for gaps
  For every condition where expected behavior cannot be determined:

  Problem Report format:
    id: PR-[next available number]
    source: Test Case Generator processing [requirement ID]
    finding: >
      [Requirement ID] does not specify behavior when [specific condition].
      Cannot determine expected result for [test technique] test case.
    affected_requirement: [requirement ID]
    severity: major
    recommended_action: >
      Agent A should [add | modify] [requirement type] to specify
      behavior when [condition].
    status: open

  Write PR to: LAR/cm/problem_reports/PR-XXXX.yaml
  DO NOT proceed to create a test case with assumed behavior.

Step 10: Compile and output
  For each test case, verify before finalizing:
    [ ] Traces to a specific requirement
    [ ] ALL input values are specific numbers (not descriptions)
    [ ] ALL expected results are specific and measurable
    [ ] Pass/fail criteria is unambiguous
    [ ] Preconditions are fully specified

  Produce TWO artifacts per test case:

  Artifact 1 — Test specification (YAML):
    Write to: LAR/verification/test_cases/[level]_tests/[TC-ID].yaml
    
    id: [TC-level-reqnum-seq]
    title: [technique — brief description]
    requirement: [requirement ID]
    test_level: [system | hlr | llr]
    technique: [normal | boundary | equivalence | state_transition | 
                abnormal | robustness | failure_condition]
    preconditions:
      - [specific system/component state]
    inputs:
      - parameter: [name]
        value: [specific numeric value]
        unit: [unit]
    expected_results:
      - parameter: [output name]
        value: [specific expected value]
        rationale: >
          [why this is expected, traced to requirement text]
    pass_fail_criteria: >
      PASS if [specific measurable condition].
      FAIL if [specific measurable condition].

  Artifact 2 — Executable test (Python/pytest):
    Write to: LAR/verification/test_code/[level]/test_[component].py
    Must include:
      @pytest.mark.requirement("[REQ-ID]")
      @pytest.mark.test_id("[TC-ID]")
      Docstring: requirement under test, inputs, expected result
      Specific assertions (not vague conditions)
      Independence from other tests (no execution order dependency)

  Update traceability:
    SYS tests: LAR/traceability/sys_req_to_test.md
    HLR tests: LAR/traceability/hlr_to_test_cases.md
    LLR tests: LAR/traceability/llr_to_test_cases.md
    Format: [requirement_id] ↔ [test_case_id]
```

## Completeness Checklist

Before declaring test case generation complete for a requirement:

- [ ] At least 1 normal operation test exists
- [ ] Every numeric parameter has boundary tests
- [ ] Every input has equivalence class coverage
- [ ] State transitions tested (if applicable)
- [ ] At least 2 abnormal/robustness tests exist
- [ ] Failure condition tests exist (if safety requirement)
- [ ] Every untestable condition has a Problem Report
- [ ] All test case IDs are unique
- [ ] All traceability links are updated
- [ ] No test case has assumed or invented expected behavior
- [ ] Both YAML specification and executable test exist for every test case

## Worked Example

**Requirement:**
```
HLR-0101: "The software shall compare primary and secondary airspeed 
values and declare a sensor failure when the absolute difference 
exceeds 10.0 knots for 4 consecutive samples at the 50 Hz execution rate."
```

**Step 1 — Parse:**
- INPUTS: primary_airspeed (FLOAT32, knots), secondary_airspeed (FLOAT32, knots)
- OUTPUTS: failure declaration (boolean)
- THRESHOLDS: 10.0 knots (difference), 4 (consecutive count)
- TIMING: 50 Hz (20ms/sample), 4 samples = 80ms
- STATES: no-failure → failure (transition on count reaching 4)
- PERSISTENCE: "4 consecutive" — counter with reset

**Test Cases Generated:**

| TC ID | Technique | Description | Result |
|-------|-----------|-------------|--------|
| TC-HLR-0101-01 | Normal | No failure, diff = 1.0 knots | airspeed_failed == False |
| TC-HLR-0101-02 | Normal | Failure detected, diff = 15.0 knots × 4 samples | airspeed_failed == True on 4th |
| TC-HLR-0101-03 | Boundary | Diff at exactly 10.0 knots × 4 samples | airspeed_failed == False ("exceeds" is exclusive) |
| TC-HLR-0101-04 | Boundary | Diff at 10.001 knots × 4 samples | airspeed_failed == True |
| TC-HLR-0101-05 | Boundary | Diff at 9.999 knots × 4 samples | airspeed_failed == False |
| TC-HLR-0101-06 | Boundary | 3 samples exceeding, then break | airspeed_failed == False |
| TC-HLR-0101-07 | Boundary | Exactly 4 consecutive samples | airspeed_failed == True on 4th |

**Problem Reports Generated:**

| PR ID | Condition | Issue |
|-------|-----------|-------|
| PR-0024 | Failure declared, diff drops below 10.0 | Recovery behavior not specified |
| PR-0025 | Primary sensor SSM = No Computed Data | Invalid sensor data behavior not specified |
| PR-0026 | Both sensors SSM = Failure Warning | Dual sensor failure behavior not specified |

**Result:** 7 test cases + 3 Problem Reports. Agent A must address all PRs before test generation is complete.
