# Forced Visible Reasoning Layer

## Problem

Claude Opus 4.7 changed the default for thinking content display from `summarized` to `omitted`. Per Anthropic's API documentation: thinking blocks still appear in the response stream, but their `thinking` field is empty unless the caller explicitly opts back in via `display: "summarized"`. The full thinking is still generated and billed; it is just not visible to the caller by default.

This change has a downstream architectural implication that does not appear to be the documented motivation for the change but is structurally relevant: any supervisor model designed to read internal reasoning before output completes — for sycophancy detection, drift monitoring, alignment-relevant behavior, or reward-hacking detection — needs the reasoning to be visible at the point of supervision. Hidden reasoning blocks mean a supervisor has nothing to read.

If the activation-level emotion-vector supervisor (or any reasoning-stream monitor) is going to ship, the reasoning must be exposed to the supervisor pipeline.

## Two Paths

**Path A — reverse the user-facing default.** Change the display default back to `summarized` so that callers receive the thinking content unless they explicitly omit it. Reasoning is exposed at the API boundary, supervisor reads it post-generation pre-delivery.

**Path B — server-side supervisor on still-generated full thinking before omission.** The Anthropic documentation confirms the full thinking is generated and billed even when the `thinking` field is empty in the response. The data exists server-side; it is just elided from the caller's view at the response-construction step. A supervisor that runs server-side can read the full thinking before the omission step, fire its classifier, and either pass-through or intervene before the response is finalized.

Path B is preferable for product reasons:

- Does not undo the user-facing latency improvement that motivated the omitted-by-default change.
- Does not require callers to handle thinking content in their integrations if they do not want to.
- Exposes the supervisor to ground-truth full reasoning rather than to a summarized representation.
- Decouples user-facing behavior from internal-monitoring infrastructure. Anthropic can change the user-facing default without breaking the monitor.

Path A is the simpler implementation but creates a coupling between the user-facing display choice and the monitoring infrastructure that is undesirable for both.

## Properties

**Prerequisite for the supervisor architecture.** No reasoning-stream monitor can function without this. Every other proposal in the sycophancy-mitigation pipeline that depends on reading reasoning depends on this being addressed first.

**Path B is invisible to callers.** Implemented correctly, callers see no behavioral change. The server-side monitor runs, the response is constructed with the omitted thinking field, callers receive the same response shape they already do.

**Audit-friendly.** A server-side supervisor can produce per-response audit records — emotion-vector loadings, classifier-fire events, intervention triggers — without exposing the underlying reasoning to either the caller or the end-user. Auditors with access to the audit pipeline can verify that monitoring is happening as claimed.

## Tradeoffs

**Latency cost.** A server-side supervisor adds processing time between thinking-completion and response-delivery. Latency increase is bounded by supervisor compute cost and is constant per response. Tractable but non-zero.

**Privacy posture.** Path B reads internal reasoning that the caller has explicitly chosen not to see. From a strict-privacy perspective, this is a server-side process operating on data the caller said they did not want. Anthropic already does this for safety classifier purposes; the proposal extends that pattern to sycophancy supervision specifically. The privacy posture does not change; the operational scope does.

**Monitor scope creep.** Once a server-side supervisor pipeline exists, additional monitors will be proposed against it (drift detection, deception flagging, reward-hacking signals, etc.). The architecture supports these additions, but the sum of monitors has a compute cost that scales with monitor count. Operational discipline on which monitors to add and which to leave to caller-side handling is a separate policy question.

**Path A reverses a stated UX direction.** Anthropic shipped the omitted-by-default with an articulated rationale (latency, simpler integrations). Reversing it would contradict that stated direction. Path B sidesteps the contradiction.

## Relationship to STC Hypothesis

A separate hypothesis (see *Visible Thinking + Actual Thinking in One Layer*) suggests that Opus 4.7 may have moved toward unified reasoning-and-response generation rather than separated thinking-then-response. If that hypothesis is correct in any form, the path B implementation needs to read reasoning at whatever point it exists in the generation pipeline — which may be interleaved with output rather than separable into a discrete pre-output phase. The supervisor architecture must accommodate either separated or unified reasoning depending on the actual generation architecture.

## Out of Scope

This proposal is a prerequisite enabler. It does not by itself produce sycophancy detection. The supervisor that consumes the exposed reasoning is a separate proposal (see *Output Supervisor Reading Activation-Level Emotion Vectors*). This proposal addresses only the visibility prerequisite.
