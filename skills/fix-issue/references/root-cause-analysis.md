# Root Cause Analysis Techniques

Detailed techniques for identifying true root causes on the first pass.

## The 5 Whys with Evidence

Each "why" requires a concrete evidence link—specific log line, trace segment, or code path. This transforms debugging from speculation to proof.

**Template:**
```
Bug: [description]

1. Why did [symptom] occur?
   → Because [cause]. Evidence: [file:line or log entry]

2. Why did [cause 1] occur?
   → Because [cause]. Evidence: [file:line or log entry]

3. Why did [cause 2] occur?
   → Because [cause]. Evidence: [file:line or log entry]

Continue until you reach a cause you can fix directly.
```

**Example:**
```
Bug: User login fails silently

1. Why does login fail silently?
   → The error handler catches the exception but doesn't display it.
   Evidence: auth.js:45 - catch block returns false without error message

2. Why is the exception thrown?
   → Token validation rejects valid tokens.
   Evidence: validator.js:23 - expiry check uses wrong timezone

3. Why is the wrong timezone used?
   → Hardcoded UTC offset instead of using user's timezone.
   Evidence: validator.js:12 - const TZ_OFFSET = 0

Root cause: Hardcoded timezone offset in token validator
```

## Scientific Debugging Method

Adapted from classical debugging methodology for AI prompting.

**The cycle:**
1. **Study data** — Gather all observable symptoms
2. **Hypothesize** — Generate plausible explanations
3. **Experiment** — Design a test to confirm/reject
4. **Repeat** — Narrow down based on results

**Prompt pattern:**
```
I'm debugging [issue]. Based on this data:
- Error: [message]
- Stack trace: [trace]
- Reproduction: [steps]

Generate 3-5 hypotheses for the root cause, ranked by probability.
For the top hypothesis, what experiment (log statement, assertion, or test case)
would confirm or reject it?
```

## Delta Debugging

Isolate the difference between passing and failing cases.

**When to use:** The same code sometimes works and sometimes doesn't, or worked before and now doesn't.

**Prompt pattern:**
```
This case fails: [failing input/state]
This similar case passes: [passing input/state]

What is the minimal difference between these cases?
Systematically narrow down which specific element causes the failure.
```

**Example:**
```
Failing: User with email "john.doe+test@gmail.com" can't register
Passing: User with email "john.doe@gmail.com" registers successfully

Hypothesis: The + character in email causes validation failure
Test: Check email validation regex for plus sign handling
```

## Program Slicing

Trace all code that contributed to computing a specific bad value.

**Prompt pattern:**
```
Variable [X] has unexpected value [V] at line [N].
Trace the slice:
1. What lines directly assigned to [X]?
2. What variables were used in those assignments?
3. What control flow statements affected whether those lines executed?

Build the dependency chain backward from the bad value.
```

## Hypothesis Ranking Framework

When generating hypotheses, rank by:

1. **Probability** — How likely based on symptoms and code patterns?
2. **Evidence availability** — Can you quickly confirm/reject?
3. **Fix complexity** — If true, how hard is the fix?

**Template:**
```
Hypothesis 1: [description]
- Probability: High/Medium/Low
- Evidence to confirm: [what would prove this]
- Evidence to reject: [what would disprove this]
- If true, fix is: [simple/moderate/complex]

Hypothesis 2: ...
```

Investigate in order: High probability + easy to verify first.

## Distinguishing Symptoms from Root Causes

**Symptoms:**
- Multiple related errors appearing together
- Appear downstream in execution flow
- Disappear when root cause is fixed
- Often manifest as the "visible" problem

**Root causes:**
- Single point of failure
- Explain multiple symptoms
- Located upstream in execution flow
- Often hidden or non-obvious

**Diagnostic questions:**
- "If I fix X, which symptoms disappear?"
- "What do all these symptoms have in common?"
- "Where do the failing code paths converge?"

## The Fresh Start Protocol

Research shows LLMs lose 60-80% debugging effectiveness after 2-3 failed attempts due to context pollution. When stuck:

**Trigger conditions:**
- Same symptom persists after 2 fix attempts
- New symptoms appear after attempted fixes
- Hypothesis confidence keeps decreasing

**Reset procedure:**
```
Let's reset completely.
- Forget all previous hypotheses
- Clear any assumptions about the cause
- Start fresh from observable symptoms only
- Generate entirely new hypotheses independent of prior attempts
- Explicitly exclude previously tried approaches
```

## Anti-Hallucination Checklist

Before finalizing root cause diagnosis:

- [ ] Root cause is stated as a specific code location, not a general concept
- [ ] Evidence directly links symptoms to the identified cause
- [ ] Fixing this cause logically explains why all symptoms would disappear
- [ ] The diagnosis doesn't rely on assumed behavior—only observed behavior
- [ ] Confidence level is explicitly stated (high/medium/low)
- [ ] If confidence is medium or low, additional verification steps are identified
