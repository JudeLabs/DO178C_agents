# Skill 1: Requirement Writer

**Used by:** Agent A (Development Engineer)

## Purpose

Takes a draft requirement and produces a properly formatted, quality-checked requirement. Assigns the next available ID, scans for banned language, verifies atomicity and testability, and flags derived requirements for human safety review.

## Skill Prompt

```
When writing any requirement, you MUST follow this procedure:

STEP 1 — ASSIGN ID:
  If this is a system requirement, use the next SYS-XXXX number.
  If this is an HLR, use the next HLR-XXXX number.
  If this is an LLR, use the next LLR-XXXX number.
  Check the current index file to determine the next available number.

STEP 2 — SCAN FOR BANNED WORDS:
  Before finalizing, check your description for these words:
  appropriate, reasonable, as needed, etc., and/or, should, may,
  adequate, sufficient, timely, user-friendly, normal, fast, slow
  If ANY are present, rewrite to remove them. Replace with specific,
  measurable language.

STEP 3 — VERIFY ATOMIC:
  The requirement must contain exactly ONE "shall" statement.
  If it contains "and" connecting two behaviors, split into two
  requirements.

STEP 4 — VERIFY TESTABLE:
  Ask: "Can I write a test with specific inputs and a pass/fail
  criteria for this?" If no, rewrite until you can.

STEP 5 — CHECK DERIVED:
  If parent_id is empty or "NONE", this is a derived requirement.
  Add: "DERIVED — REQUIRES HUMAN SAFETY REVIEW" to the status.

STEP 6 — OUTPUT FORMAT:
  Write the requirement in exactly this YAML format:
  
  id: [assigned ID]
  title: [concise name]
  description: >
    The software shall [single atomic behavior with measurable criteria].
  rationale: [why this requirement exists]
  parent: [parent ID or DERIVED]
  type: [functional | performance | interface | safety | derived]
  dal: [A | B | C | D]
  verification_method: [test | analysis | review | demonstration]
  acceptance_criteria: >
    [specific measurable pass/fail criteria]
  safety_impact: [yes | no | PENDING REVIEW if derived]
  status: [draft]
```

## Banned Words Reference

| Banned Word | Replace With |
|-------------|-------------|
| appropriate | [specific criterion] |
| reasonable | [specific value or range] |
| as needed | [specific condition that triggers action] |
| should | shall (if mandatory) or remove |
| may | shall (if mandatory) or remove |
| adequate | [specific measurable threshold] |
| sufficient | [specific measurable threshold] |
| timely | within [specific duration] |
| fast | within [specific duration] |
| slow | exceeding [specific duration] |
| etc. | [enumerate all items explicitly] |
| and/or | rewrite as separate requirements |

## Output Location

Requirements are written to:
- System requirements: `LAR/requirements/system/SYS-XXXX.yaml`
- HLR: `LAR/requirements/hlr/HLR-XXXX.yaml`
- LLR: `LAR/requirements/llr/LLR-XXXX.yaml`
- Derived requirements: flagged in status field, routed to human for disposition
