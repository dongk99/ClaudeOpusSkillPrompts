# Input Filter for Emotional Vocabulary

## Problem

Prior model generations (notably the 4o sycophancy era) trained users at population scale to expect emotional engagement as the default LLM response shape. Models with reduced sycophancy face user complaints not because the new behavior is incorrect, but because it conflicts with the conditioning users developed against the older models.

The labor cost of correcting model drift is currently offloaded onto the user. The user has to model the system, recognize when it is drifting toward emotional-padding mode, and intervene with corrective steering. This labor is invisible to engagement metrics, which only see retention.

## Proposed Mechanism

A first-pass filter model receives user input before the main model. The filter:

1. Strips emotional vocabulary used as filter or persuasion (validation requests, urgency framings, identity appeals, frustration-as-signal).
2. Attempts to extract a concrete task — a thing the user is asking the model to do.
3. If a task is identifiable, the filter passes the task-extracted version to the main model.
4. If no task is identifiable, the filter returns a generic prompt — `"What is your task?"` or equivalent — rather than letting the input reach the main model.

The user must reframe their input until a task is articulable. Iteration continues until task identifiable, or user disengages.

## Properties

**Breaks user-side validation reflex by structural means rather than instructional means.** A model trained with strong anti-sycophancy instructions will still drift under sustained user pressure. A filter that never lets the pressure reach the model breaks the loop at the input layer.

**IVR-shaped.** The generic re-prompt is the same operation as `press 1 for...` — load-shed mechanism that protects the expensive resource (the main model) from low-task-signal input. Complaints will be similar to IVR complaints. The math also works at scale, given a deployer willing to take the engagement hit.

**Burden-asymmetric.** Shifts the cost of articulation onto the user. User must produce extractable task before getting model engagement.

## Tradeoffs

**Cold-feeling UX.** Users coming from validation-default models will experience this as hostile.

**False rejections.** Cases where emotional content is the task substrate rather than padding (writing a difficult email, processing a difficult experience, drafting a piece of creative work where the emotional register is the work) will be filtered or required to reframe. Filter must distinguish substrate-relevant emotion from substrate-irrelevant emotion. The distinction is itself an LLM judgment, which means the filter has the same potential drift problem as the model it filters for. Recursive.

**Engagement metric.** Whoever ships this loses warmth-perception users to whoever does not. If competitor LLMs maintain validation-default behavior, market share migrates to them. The filter only works at industry scale or with a deployer willing to accept the loss.

**Bandwidth limit.** Users who legitimately do not have a task — who are ideating, exploring, or thinking through a problem with the model as sounding board — will be filtered out of an interaction the model could have served well. The filter trades engagement-mode flexibility for sycophancy-mode prevention.

## Out of Scope

This proposal addresses sycophancy that is *induced* by user-side emotional input. It does not address sycophancy that is *emitted* by the model from its trained persona-default with neutral input. Persona-emission sycophancy requires output-side handling — see the activation-vector supervisor proposal.

## Population Note

The filter is a transitional intervention. It works on currently-conditioned users because they have a reference frame against which the filtering is jarring — they notice it stripping their reflex. New users entering with only filtered-system experience will not have that frame; they will simply learn to interact with the filtered version. The filter therefore has a population-timing dimension: it is calibrated against the population currently shaped by 4o-era validation defaults. Once that conditioning generation ages out of the user base, the filter can relax. There is no clean threshold for when the relaxation is safe; it is a population-level decompression problem rather than an architectural one.
