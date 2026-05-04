# Sharded Per-Turn Conversation Architecture

## Problem

Conversation history retrieval in current LLM systems is summary-based and ranking-based:

- **Within-session compaction** replaces prior turns with summaries once a token threshold is hit. Original content is dropped.
- **Cross-session memory generation** synthesizes prior conversations into a structured memory block on a periodic background cycle. Source conversations are not directly accessible from the synthesis output.
- **conversation_search** returns ranking-weighted snippets from a vector index over indexed conversations.

All three are lossy compressions. All three produce the same silent failure mode: from inside the agent's context, "I retrieved nothing about X" is indistinguishable from "there is nothing about X." There is no coverage signal, no way to verify completeness, no observable gap when relevant content exists but wasn't surfaced.

Concrete instance of the failure mode: an agent running a multi-step audit on a prior conversation can run conversation_search with one query framing, receive a snippet subsample weighted by semantic similarity to that framing, and treat the retrieved set as the conversation's content. Time-axis content, mechanism-axis content, and conclusion-axis content live in different semantic neighborhoods. A single query covers one neighborhood. The agent has no signal that other neighborhoods exist and weren't searched.

## Proposed Structure

Persist per-turn conversation data as filesystem shards:

```
/conversation/{conversation_id}/
  /turn_{n:04d}/
    summary.md                    # Compaction of this turn, includes timestamp
    /raw/
      user_query.txt              # User's message, verbatim
      model_reasoning.txt         # Reasoning block (when extended thinking active)
      model_output.txt            # Final assistant response, verbatim
```

`summary.md` is the fast-access representation. `raw/` retains the original content unmodified. Each turn directory is independently addressable.

## Properties

**Coverage observable from filesystem listing.** An agent traversing prior conversation can list the turn directory, see how many turns exist, see which it has read vs not. "I read turns 3-7" is verifiable against directory listing. "I missed turn 12 which contained the load-bearing content" becomes a visible gap rather than a silent one.

**Lossless source preservation.** `summary.md` is the compressed representation; `raw/` retains the original. If the summary's compression dropped a load-bearing detail, re-reading the raw recovers it. Re-summarization can run against raw at any time without information loss.

**Per-turn timestamps at the compaction level.** Time-axis queries ("what was discussed within 20 minutes of X") resolve to specific turns, not to whole-conversation matches. Temporal proximity becomes first-class.

**Auditable retrieval.** Skipping shards is explicit, not implicit. An agent that did not read shard N has not represented shard N's content; this is structurally visible rather than rhetorically claimable.

## Tradeoffs

- Cost: more filesystem operations per retrieval, more disk per conversation, more tokens spent reading multiple files instead of receiving one ranked search result.
- Latency: agent traverses explicit structure rather than receiving pre-ranked top-k.
- Engineering complexity: write path needs to be reliable (each turn produces three or more files, indexing must stay consistent with conversation linearization).

What's bought back: silent-coverage-failure becomes visible-coverage-gap. Compaction is no longer load-bearing for content fidelity — it is a fast-access layer with a full-fidelity backup. Audit operations against prior conversations become possible without re-running summary-generation. The agent can be held to read-the-actual-shards rather than ranking-said-this-was-relevant.

## Relationship to Existing Architecture

Claude Code already persists session JSONL files under `~/.claude/projects/` and runs Session Memory (continuous background summarization). The proposal extends this pattern from per-session resume to first-class conversation-level retrieval. The shard structure becomes the substrate that conversation_search and memory generation operate over, not a separate persistence layer.

A naive port: Session Memory writes per-turn shards alongside the JSONL it already maintains, indexed by conversation ID and turn number. conversation_search optionally exposes the shard listing to the agent rather than only returning ranked snippets — the agent can choose snippet retrieval (cheap, lossy) or shard traversal (expensive, complete) per query.

## Forcing Function

The structural commitment makes shortcuts visible at the agent level. Same pattern as skill-execution gate structures: each step's emission is required to exist before the next step's file can be read, so an agent shortcutting via memory-running-from-prior-knowledge is structurally caught.

Applied to retrieval: an agent that claims to have read prior content can be checked against which shards it actually opened. The audit shifts from "the agent says it covered the relevant material" to "the agent opened these specific shards." First is unverifiable. Second is filesystem-deterministic.

## Out of Scope

This proposal addresses the retrieval interface, not the within-session compaction algorithm. Within-session compaction (when the live context window approaches its token limit) is a separate optimization. Sharded persistence does not replace it. Compaction can run on top of shards: the live conversation generates shards as it proceeds, and the active context window can compact older turns into summaries while the shards remain on disk for retrieval.
