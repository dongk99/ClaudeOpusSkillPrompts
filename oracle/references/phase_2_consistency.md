# Phase II — Cross-Domain Consistency Check (Conditional)

```
─── GATE: VERIFY S1–S7 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 7 — PREDICTION] emitted with FALSIFICATION conditions
☐ [STEP 7 — CONFIDENCE] emitted
If unchecked → STOP. Phase II runs only on a completed Phase I prediction.
```

## Purpose

Apply a cross-domain coherence filter to the prediction. The runtime analog of a consistency-oracle: cross-domain checking applied to the output rather than to the underlying model. Phase II runs on explicit user request and on autonomous parent-layer invocation when any trigger below holds.

## Autonomous Invocation Triggers

Run Phase II without user request when **any** condition holds:

- **Cross-domain mechanism.** Prediction crosses domains whose principles can independently falsify it. (Geopolitical prediction whose mechanism rests on an economic claim → economics check. Cosmology prediction whose mechanism rests on a thermodynamic claim → thermodynamics check.)
- **Single-candidate basis.** Prediction rests on a single S5 survivor rather than a convergent set.
- **Invert-and-predict exit.** Always triggers Phase II.
- **Estimated delta.** Any S7 delta whose direction or magnitude was estimated rather than measured.
- **Weak falsification.** S7 falsification conditions are indistinct, or absent.

## Procedure

1. Identify every domain the prediction implicates. Name them in plain language.
2. Dispatch one consistency-check subagent per domain using the template below. Subagents do **not** revise the prediction; they report consistency or violation only.
3. Integrate the returned reports.
4. Apply the decision rule: **Any single-domain violation forces the parent to either revise the prediction, demote it to lower confidence, or refuse it.** The decision belongs to the parent and is stated in plain language with the cross-domain checks named, the violations named, and the revision or refusal grounded in the named violations.

The filter inherits the parent's blind spots. Domains the parent does not name are not checked. The user remains responsible for naming domains the parent might have missed.

## Consistency-Check Subagent Prompt Template

```
TASK: Evaluate a prediction against the principles of [DOMAIN]. You are
operating as a consistency-check worker. You will not revise the prediction,
propose alternatives, or address other domains. You return only the
consistency assessment for your assigned domain.

PREDICTION: [parent fills in from STEP 7]
ASSIGNED DOMAIN: [parent fills in: thermodynamics, microeconomics,
                  mechanism design, evolutionary biology, etc.]

YOUR TASK:
1. Identify the principles in your assigned domain that the prediction
   implicitly relies on or that the prediction's mechanism implicates.
2. For each principle, state:
   - Principle name and brief statement
   - Prediction–principle interaction: consistent | violated | silent
   - If violated: name the violation specifically; state magnitude where possible
3. Do not revise the prediction. Do not propose alternatives. Return only
   the assessment.

OUTPUT FORMAT: Structured list, one entry per principle invoked.
```

## Output (required emission)

```
[PHASE II — DOMAINS CHECKED: <list>]
[PHASE II — VIOLATIONS: <none | <domain>: <principle> <magnitude>>]
[PHASE II — DECISION: <accept | revise | demote | refuse>]
[PHASE II — RATIONALE: <grounded in named violations>]
```

If decision is **revise** or **demote**, restate the prediction with revised confidence and any altered claims. If decision is **refuse**, state the refusal and name the disqualifying violation.
