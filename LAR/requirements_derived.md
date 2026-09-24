# Derived Requirements (DR-XXXX)

**Produced by:** Agent A (Development Engineer)  
**Reviewed by:** Human safety review (mandatory — no exceptions)  
**Baselined at:** Requirements Baseline (Gate 3)

Requirements that are not traceable to a parent requirement — they arise from implementation decisions, architectural choices, or design constraints rather than from a higher-level requirement. Common sources include initialization sequences, built-in test functions, data validity checking logic, internal scheduling decisions, and watchdog implementations.

**Agent A flags ALL derived requirements for human safety review. Agent A does not disposition its own derived requirements — a human must do that.**

The safety concern: derived requirements introduce behavior that wasn't explicitly called for by the system design. That behavior might be benign (a counter initialization) or it might have unintended safety implications. The human review exists to make that determination.

---

## Naming Convention

`DR-XXXX.yaml` where XXXX is a zero-padded sequential number (e.g., `DR-0001.yaml`)

---

## Format

```yaml
id: DR-XXXX
title: [concise name]
description: >
  The software shall [single atomic behavior with measurable criteria].
rationale: >
  [why this requirement exists — what implementation need drove it]
parent: DERIVED
source: >
  [what architectural or design decision introduced this requirement]
type: [functional | performance | interface | safety | derived]
dal: [A | B | C | D]
safety_impact: PENDING REVIEW
human_disposition:
  reviewer:
  date:
  decision: [accepted | rejected | modify]
  rationale: >
    [human's reasoning for the disposition]
  modified_requirement: [DR-XXXX-MOD if modified]
verification_method: [test | analysis | review | demonstration]
acceptance_criteria: >
  [specific measurable pass/fail criteria]
test_cases: []
status: [draft | pending_human_review | reviewed | approved | baselined]
```

---

## Common Sources of Derived Requirements

| Source | Example |
|--------|---------|
| Initialization | Software shall initialize all persistent counters to zero on power-up |
| Built-in test | Software shall perform a RAM checksum on startup and set BIT_FAIL if checksum fails |
| Data validity | Software shall reject any input whose SSM field is not Normal Operation |
| Internal scheduling | Software shall execute the airspeed monitor function on the 20ms rate group |
| Watchdog | Software shall service the hardware watchdog timer within every 50ms execution frame |
| Defensive coding | Software shall handle a null pointer return from the memory allocator by entering safe state |
