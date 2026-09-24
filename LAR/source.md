# Source Code

**Produced by:** Agent B (Developer)  
**Reviewed by:** Agent C (Verification Engineer) — code review against LLR  
**Baselined at:** Code Baseline (Gate 5)

Source code files implementing the Low-Level Requirements. Agent B writes code **after** Agent C has already written the test cases. Agent B's only objective is to make the existing tests pass. Every code element must trace to at least one LLR — code without a requirement is dead code and must be removed.

---

## Required File Header

Every source file must include a header block identifying which LLR it implements:

```c
/*
 * FILE:        [filename]
 * PURPOSE:     [one-line description]
 * LLR REFS:    [LLR-XXXX-YYY, LLR-XXXX-YYY, ...]
 * VERSION:     [version]
 * AUTHOR:      Agent B
 * REVIEWED BY: Agent C (see RR-XXXX)
 */
```

---

## Required Function Header

Every function must include a header block:

```c
/*
 * FUNCTION:     [function_name]
 * DESCRIPTION:  [what this function does]
 * LLR REF:      [LLR-XXXX-YYY]
 * PARAMETERS:
 *   [param_name] (IN)  [type] [units] [range] — [description]
 *   [param_name] (OUT) [type] [units] [range] — [description]
 * RETURNS:      [return type and meaning]
 * PRECONDITIONS:  [what must be true before calling]
 * POSTCONDITIONS: [what is guaranteed true after return]
 */
```

---

## Coding Constraints (defined in SDP)

- No dynamic memory allocation
- No recursion
- No undefined behavior
- No code that does not trace to an LLR
- Static analysis warnings must not be suppressed without a human-approved deviation record

---

## Regression Gate

Every commit from Agent B triggers automatic execution of all test cases and conventional static analysis (MISRA C conformance, structural coverage, data/control coupling, stack depth, WCET, dead/deactivated code, type and range violations). **Any test failure or analyzer violation rejects the commit.** Agent B must revise and resubmit. The commit package carries static analysis results alongside test results for human review. Analyzers used this way are a DO-330 Criterion 3 qualification case (TQL-5 at every DAL), unrelated to the agents. The test suite is the behavioral contract — Agent B cannot violate it.
