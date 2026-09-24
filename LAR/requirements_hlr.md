# High-Level Requirements (HLR-XXXX)

**Produced by:** Agent A (Development Engineer) using Skill 1 (Requirement Writer)  
**Reviewed by:** Agent C (Verification Engineer)  
**Baselined at:** Requirements Baseline (Gate 3)

Software-level requirements decomposed from system requirements. Each HLR specifies functional behavior, performance, interfaces, or safety constraints at the software level. HLR must trace upward to a SYS requirement (or be flagged DERIVED) and downward to at least one LLR and one architecture component.

Agent C writes HLR-level integration test cases (TC-HLR-XXXX) against these requirements **before Agent B writes any code**. The test-writing process is the primary mechanism for discovering gaps in requirements — when Agent C cannot determine expected behavior for a condition, it issues a Problem Report rather than assuming behavior.

---

## Naming Convention

`HLR-XXXX.yaml` where XXXX is a zero-padded sequential number (e.g., `HLR-0101.yaml`)

---

## Format

```yaml
id: HLR-XXXX
title: [concise name]
description: >
  The software shall [single atomic behavior with measurable criteria].
rationale: [why this requirement exists]
parent: [SYS-XXXX or DERIVED]
type: [functional | performance | interface | safety | derived]
dal: [A | B | C | D]
verification_method: [test | analysis | review | demonstration]
acceptance_criteria: >
  [specific measurable pass/fail criteria]
safety_impact: [yes | no | PENDING REVIEW if derived]
allocated_to: []   # architecture component(s) implementing this HLR
child_llr: []      # LLR-XXXX IDs decomposed from this HLR
test_cases: []     # TC-HLR-XXXX IDs verifying this HLR
status: [draft | reviewed | approved | baselined]
```

---

## Example

```yaml
id: HLR-0101
title: Airspeed Sensor Failure Detection Logic
description: >
  The software shall compare primary and secondary airspeed values and
  declare a sensor failure when the absolute difference exceeds 10.0 knots
  for 4 consecutive samples at the 50 Hz execution rate.
rationale: >
  Implements SYS-0001 detection requirement at the software level.
  4-sample persistence prevents false trips from transient noise.
parent: SYS-0001
type: safety
dal: B
verification_method: test
acceptance_criteria: >
  airspeed_failed flag is set to TRUE on the 4th consecutive sample
  where |primary_airspeed - secondary_airspeed| > 10.0 knots.
  Flag is not set if the condition clears before 4 consecutive samples.
safety_impact: yes
allocated_to: []
child_llr: []
test_cases: []
status: draft
```
