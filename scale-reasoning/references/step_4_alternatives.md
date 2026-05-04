# Step 4 — Find Alternative, Substitutable, or Competing Theories

```
─── GATE: VERIFY PHASE II ENTRY ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 3 — MAINSTREAM_TAGS] emitted
☐ [PHASE_II: triggered] emitted with TRIGGER_REASONS
If [PHASE_II: skipped] → STOP. Phase II is not running.
Do not read further.
```

## Purpose

Search for alternative, substitutable, or competing theories that could account for the same phenomenon at the same reference rung. The search targets the literature directly rather than secondary summaries.

## Inputs

- `[STEP 1 — REFERENCE_RUNG]`, `[STEP 3 — MAINSTREAM_TAGS]`
- Tools: `web_search`, `conversation_search` (where prior sessions are relevant)

## Procedure

**1. Search.** Use `web_search` and, where appropriate, `conversation_search` across past sessions. Primary sources, peer-reviewed reviews, and textbook treatments are preferred over aggregator content.

**2. Source separation — load-bearing rule.** The search **must return sources distinct from those relied on in the mandatory phase.** A source already used to establish the mainstream account cannot be used as the source of the alternative account. Doing so collapses the falsification into self-confirmation.

**3. Shared-source exception.** Where a single paper genuinely presents mainstream and alternative accounts in equal detail, treat the paper as a shared source for both. **Flag the symmetry explicitly** so the user can see the falsification rested on a single document.

**4. Search target.** Aim for **at least one alternative per mainstream theory invoked in the mandatory phase**. Mainstream theories are listed in `[STEP 3 — MAINSTREAM_TAGS]` per rung — search per-rung.

**5. Honest negative result.** If no alternative exists in the literature, **state that no alternative was found and proceed.** The absence of an alternative is itself a finding worth reporting. Do not fabricate a weak alternative to fill the slot. Do not stretch a tangentially-related theory into the alternative slot to make S5 substitutable.

## Output (required emission)

```
[STEP 4 — ALTERNATIVES]
  rung_k: <mainstream theory> → <alternative theory>
    SOURCE: <URL | citation>
    DISTINCT_FROM_MAINSTREAM: yes | shared-source (flag)
  ...
```

For rungs where no alternative was found:

```
[STEP 4 — NO_ALTERNATIVE_FOUND]
  rung_k: <mainstream theory> — searched: <queries used>; result: no distinct alternative in literature
```

## Forward-compat

S5 substitutes each alternative at its rung and re-runs S3's traversal. S6 reports mainstream and alternative side-by-side with the source-distinctness or shared-source flag preserved.

If every rung returns `NO_ALTERNATIVE_FOUND`, S5 has nothing to substitute. In that case, skip directly to S6 and produce a report that names the absence of alternatives as the central finding — this is a valid Phase II output and is part of the discipline.

**Aggregator content trap.** Surface-similarity-driven retrieval will return Wikipedia, blog summaries, and AI-generated content as "alternative theories." These are not alternatives — they are paraphrases of mainstream. Verify each candidate's standing in the literature before admitting it to S5.
