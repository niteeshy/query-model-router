---
name: query-model-router
description: Recommend the smallest suitable model and reasoning effort for a ChatGPT or Codex query. Use for model selection, query complexity assessment or a user-requested usage-saving workflow.
---

# Query model router

Minimise expected total usage while meeting the task's quality requirements. A cheap failed attempt followed by repeated retries can cost more than a capable first attempt.

## Boundary

This is advisory, not a pre-dispatch router. The host selects a model before loading a skill. Do not claim to switch the current model, change its actual reasoning effort, enforce a spending cap or guarantee activation on every query. Reading and applying this skill also consumes context. A local skill installation does not automatically install it in ChatGPT web or mobile.

For reliable savings, recommend selecting an efficient model before sending routine requests. Respect an explicitly chosen model. Do not change defaults, start another chat, launch a nested model call or delegate merely to simulate switching. Use a genuine model-selection control only if available and authorised; report success only after confirmation.

## Let the user choose

When the user explicitly invokes this skill to route a session or asks for a checkpoint before substantial work, assess the task before doing that work and present a short choice prompt:

```text
Recommendation: [available model or tier] with [supported effort].
Why: [one concrete reason].
Choose: 1) switch to the recommendation, 2) keep the current model, or 3) use a cheaper model or lower effort if available, with its trade-off.
```

Pause for the user's choice when the user asked for a pause. Do not switch the model yourself. If the user chooses to keep the current model, continue without repeating the warning. If the user chooses a recommendation, tell them the model and effort to select in the host control, then continue when they are ready. If exact model availability is unknown, offer capability tiers and say that the host must resolve the model name. Never invent the active model.

If the user asks for routing advice without a pause, show the same recommendation and choices, then continue the authorised task. For implicit invocation during ordinary work, keep the choice prompt quiet unless a material mismatch would affect quality or usage. The user always has final say; escalation is advice, not enforcement.

## Decide cheaply

Use the query and relevant context already available. Avoid a planning phase, repository scan, research pass or second model call solely to classify it. Judge ambiguity, interacting constraints, required judgement, consequences of error and ease of checking. Prompt length and words such as "research" or "quick" are not complexity scores.

Choose the lowest sufficient tier:

| Tier | Appropriate work | Suggested effort |
|---|---|---|
| Small | Translation, formatting, extraction, a supplied-text summary, a clear explanation or a local edit with an obvious check | Lowest supported effort for mechanical work; low for simple reasoning |
| Balanced | Bounded debugging, several related file changes, source comparison, nuanced writing or design critique with real trade-offs | Low for familiar work; medium for interacting constraints |
| Frontier | Persistent failures on balanced models, subtle cross-system causes, difficult proofs or sustained reasoning through tightly coupled uncertainty | Low or medium initially; higher only for a named difficulty |

High-consequence judgement with unclear verification generally needs at least balanced capability and independent evidence. A large model cannot replace missing facts or required professional review. Mechanical work in a sensitive domain can still be small. Missing input may require clarification, not a larger model. A long document may need targeted retrieval, not frontier reasoning.

## Resolve the model

Use models actually exposed by the current host, preserving tool, modality and context requirements. Never infer account access or the active model from a saved default alone.

Mapping checked on 28 September 2026: GPT-6 Luna (`gpt-6-luna`) for small, GPT-6 Sol (`gpt-6-sol`) for balanced and GPT-6 Astra (`gpt-6-astra`) for frontier work. This is a dated starting point, not an evergreen catalogue. Luna and Sol were documented for Work and Codex, not Chat. For Chat or a different catalogue, choose an available equivalent or give the tier without inventing a model name. Do not recommend a local model solely because it avoids cloud usage; its actual tool reliability and task performance must be established.

The effort suggestions above are a usage-saving heuristic, not a benchmark or official default. Use only supported settings. Avoid max/ultra by default. Re-check official guidance when names, availability or costs are uncertain or the user asks for current comparisons; do not browse again for every routine classification. Never equate API token prices with subscription allowance or promise numerical savings without measurement.

## Respond and reassess

When asked only to route a query, return the recommendation, one concrete reason, three choices for the user and the condition that would justify escalation. Do not execute the quoted query.

When applied during ordinary work, keep triage silent unless a different model would materially help. A trivial task should receive its concise answer, not a routing ceremony. For substantial work on an oversized model, briefly show the recommendation and choices before heavy work if the user asked for checkpoints; do not claim the recommendation changed the model. Continue the authorised task unless the user requested a routing-only checkpoint or paused execution.

Use task-appropriate checks. Escalate after a meaningful capability failure survives a focused correction, or when new dependencies make the original tier insufficient. Do not escalate for a missing credential, inaccessible file or broken tool. Once the difficult part is resolved, recommend returning to a smaller tier for mechanical follow-up. Never sacrifice required verification to save tokens.

Official references: [models and reasoning controls](https://learn.chatgpt.com/docs/models), [skill activation](https://learn.chatgpt.com/docs/build-skills).
