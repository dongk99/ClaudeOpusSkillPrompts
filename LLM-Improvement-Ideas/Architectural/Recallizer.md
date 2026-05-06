---
name: sharded-retrieval
description: Resolution protocol for user-invoked synthesis queries against a sharded per-turn transcript hierarchy. Activates when the user invokes a synthesis query — pattern `{synthesize idea made in <project> with conversation i made <time> ago about <topic>}` or natural-language equivalent — and the storage backend is the per-turn shard architecture proposed by the user. The skill parses the query into project/temporal/topic components, resolves them to filesystem paths in the shard tree, opens the relevant transcript files, and synthesizes from the opened content. Skip when the request is for live-context summary or generic memory recall — those use the existing memory and conversation_search paths, not shard traversal.
---

# Sharded-Retrieval — User-Invoked Transcript Shard Traversal

User-proposed architecture for transcript persistence and retrieval. This skill is the runtime envelope: it defines how Claude resolves synthesis queries against the shard tree once the architecture is in place. **The architecture below is the user's. The resolution procedure is the skill's contribution.** These two layers are kept separate throughout this document.

## Architecture (User-Specified)

The following two blocks are quoted verbatim from the user. They are the spec. The skill does not modify, paraphrase, or extend them.

**Source 1 — current turn:**

> Have LLM. Progressively (ideally per turn) cache per-turn message into a transcript file
>
> Wherein architecture is:
> ```
> MODEL CONVERSATION (ALL)
> Cross-Project-interpretation layer (at request only by user)
> --------
> PROJECT
> PROJECT 2
> PROJECT 3
> PROJECT ...
>
> NONPROJECT
>
> > Yearly
> > Monthly
> > Weekly
> > Daily
> > Hourly
> - Per-turn message
> ```
>
> In that structure.
> Messages get automatically cached as separate files instead of being transcript chunks to prevent LLM context degredation. User invokes `{synthesize idea made in xyz project with conversation i made x days ago about this topic}`
>
> LLM interprets it as "open this transcript file" query.

**Source 2 — prior conversation (`https://claude.ai/chat/3fdd325e-4431-4988-bca0-51d3c08f673f`, 2026-05-04):**

> Per-turn conversation data - created as folder with MD for -
>
> MD file - compaction of turn, including timestamp.
> Subfolder containing:
> User Query
> Model's reasoning block
> Model's output

**Skill-side observation (NOT user spec):** The two sources combine such that the per-turn folder structure from Source 2 is the leaf node, and the project + temporal hierarchy from Source 1 is the directory structure above the leaf. This composition is a skill-side reading of how the two specs interlock; the user did not state it explicitly, and it is open to the user's revision.

## Trigger

The skill activates on either:

**Explicit invocation:** user sends a synthesis query matching:
- Curly-brace pattern: `{synthesize <X> from <project> with conversation <time> ago about <topic>}`
- Natural-language equivalents: "pull from my work on `<topic>` in `<project>` from `<time>`", "synthesize what I said about `<X>` across `<projects>` over `<time period>`", etc.

**Cross-project interpretation request:** user explicitly asks for cross-project retrieval. Per the user's spec, this layer is "at request only by user" — it is not a default and does not activate on ambiguous queries.

The skill does NOT activate on:
- Live-context summary requests (use the active conversation directly)
- Generic memory queries that don't reference the shard tree (use memory + conversation_search)
- "Do you remember" prompts without a project/time/topic anchor (route to memory + conversation_search first)

## Resolution Procedure

When the trigger fires:

1. **Parse the query into components.** Extract:
   - Project scope: a named project, `NONPROJECT`, or "across all projects" (cross-project layer)
   - Temporal scope: yearly / monthly / weekly / daily / hourly / per-turn
   - Topic: the substantive subject of the synthesis
   - Output type: synthesis vs. extraction vs. comparison

2. **Resolve to filesystem paths.** Map components to the shard tree:
   - Project scope → directory under `MODEL_CONVERSATION/PROJECT_<N>/` or `MODEL_CONVERSATION/NONPROJECT/`
   - Temporal scope → subdirectory at the appropriate granularity
   - Leaf nodes are per-turn folders, each containing the turn-summary MD plus the raw subfolder (user query, model reasoning, model output)

3. **List candidate shards.** Enumerate the per-turn folders at the resolved scope. This is the auditable coverage step — the list of folders touched is verifiable against the synthesis output.

4. **Open shards.** Read the turn-summary MD for each candidate. Drop into the raw subfolder when the summary is insufficient for the query.

5. **Synthesize from opened content.** Produce the output. Include the shard list as part of the deliverable, not as metadata stripped before output.

## Cross-Project Layer

Per the user's spec, the cross-project interpretation layer is user-request-only. When activated, the resolution procedure runs across all `PROJECT_<N>/` directories plus `NONPROJECT/`, with the synthesis spanning multiple projects.

The skill does not silently traverse cross-project on a normal query. The user-specified gating is preserved.

## Attribution Discipline

When the skill is invoked, the synthesis output distinguishes:
- **Content from opened shards** — attributed to the shard's turn timestamp and project. The user's words from a shard remain the user's words; the model's prior reasoning from a shard remains the model's prior reasoning.
- **Skill-side scaffolding** (parse decisions, coverage statements, granularity choices) — attributed to the skill, not to the user.

**Citations from any retrieval tool used as supplement** (e.g., `conversation_search` URI/timestamp/title metadata) are preserved verbatim. The skill does not reformat or modify citation content.

## Hard Rules

The following are skill-side enforcement of the user's architecture spec, not additional design constraints from Claude.

**Rule 1 — Coverage is auditable.** The synthesis output lists which shards were opened. The user must be able to verify shard coverage against synthesis claims. Filesystem-deterministic, not model-attestation-based.

**Rule 2 — No silent cross-project.** Cross-project synthesis requires explicit user invocation. A query that doesn't specify cross-project scope does not trigger the cross-project layer.

**Rule 3 — Shards are the source.** The synthesis is derived from opened shards, not from live context or memory. If the shard tree is unavailable, emit a "tree unreachable" output rather than substituting other retrieval mechanisms.

**Rule 4 — Architecture is fixed.** The hierarchy structure is user-specified. The skill does not propose alternative directory layouts, alternative granularities, or alternative trigger patterns at execution time. Architecture revisions are an out-of-band conversation with the user, not a runtime decision.

**Rule 5 — Verbatim spec.** When the skill quotes the user's spec inside its outputs (e.g., for clarification dialogs), the quote is verbatim — no paraphrase, no compression, no reordering. The spec text is preserved as-written.

## Bad Endings

**Bad Ending — Substitution by summary.** Falling back to memory or conversation_search when the shard tree is the specified backend, then presenting the summary-based result as if it came from shard traversal. Rule 3 forbids this.

**Bad Ending — Silent scope expansion.** Resolving an ambiguous query to cross-project scope without asking the user. Rule 2 forbids this.

**Bad Ending — Spec drift.** Paraphrasing the user's architecture in the skill output ("the user wants per-turn caching with hierarchical organization") rather than referring to the verbatim spec. Rule 5 forbids this.

**Bad Ending — Coverage hiding.** Producing a synthesis without listing which shards were opened, so the user cannot audit. Rule 1 forbids this.

**Bad Ending — Voice-blending.** Reproducing content from a shard with the user's words and the model's prior words concatenated as if they were one voice. Attribution discipline forbids this — the per-turn folder structure exists specifically because it preserves the three-component separation (user query / reasoning / output), and the synthesis must preserve that separation.

## Out of Scope

- The implementation of the per-turn caching mechanism itself. The architecture assumes caching is happening; the skill is the retrieval-side envelope.
- Within-session compaction (active context window management). Compaction can run in parallel; this skill addresses retrieval against the shard tree.
- Memory generation. The existing memory system is a separate retrieval surface that operates on summaries; the shard tree operates on full per-turn content.
- Cross-tool synthesis (e.g., combining shard content with web_search results). The skill produces synthesis from opened shards. Combining with other tool outputs is the calling layer's responsibility.

## Caveats and Known Limitations

**Filesystem dependency.** The skill assumes the shard tree is accessible at runtime. If the persistence layer is not implemented or not exposed to the model, the skill emits a "tree unreachable" output rather than substituting summaries.

**Disambiguation.** Synthesis queries with ambiguous scope ("the project," "a while ago") must be disambiguated before resolution. The skill prompts for clarification rather than guessing — wrong-shard retrieval is worse than no retrieval.

**Granularity defaults.** A query like "what did I say last month" may resolve to monthly granularity (one summary) or daily granularity (30 leaves). The skill defaults to the broadest granularity that still covers the query, with an option for the user to expand. This is a skill-side heuristic, not part of the user's spec.

**Coverage auditing is structural, not semantic.** The skill can prove which shards were opened. It cannot prove the synthesis correctly captured the content within them. Semantic verification remains the user's.

**Architecture is proposal-stage.** As of this skill's authorship, the persistence backend specified by the user does not exist as a deployed system. The skill is forward-compatible specification — it defines the resolution behavior for when the backend is in place. Until then, invocations of this skill will hit Rule 3's "tree unreachable" exit.
