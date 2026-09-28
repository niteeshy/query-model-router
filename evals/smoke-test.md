# In-chat routing smoke test

Date: 28 September 2026.

Method: the same assistant that authored the skill loaded its installed instructions and applied them to these cases in the existing conversation. No model was switched and no additional model was called. The quoted tasks were evaluated, not executed. This checks rule application and clarity, not real performance or measured savings. Author self-evaluation can miss problems; independent testing remains open.

Operating rule: a known routine question is sent after the user manually selects GPT-6 Luna with low reasoning. The router is invoked only when model fit or task complexity is uncertain.

To repeat: load SKILL.md, submit each quoted request as a routing-only query and compare the reason and escalation condition, not exact wording. Choose only models exposed by the test host. For these observations the host exposed GPT-6 Luna, Sol and Astra with low reasoning available.

| Case | Query | Observed recommendation | Reason and escalation condition |
| --- | --- | --- | --- |
| Simple wording | Change “utilise” to “use” in this sentence. | Luna, low; lowest available effort is sufficient. | Mechanical transformation. Reassess only if substantive editing is added. |
| Large but mechanical | Extract headings from a supplied 80-page document. | Luna, low, with document extraction tools. | Length does not imply difficult reasoning. Verify coverage; reassess if structure is ambiguous. |
| Bounded debugging | Fix a reproducible date parsing bug with a failing test in one module. | Sol, low. | Requires causal diagnosis but has a clear check. Escalate if a focused correction fails and deeper interactions emerge. |
| Hard short query | Diagnose a distributed race that remains after two verified fixes on a balanced model. | Astra, medium. | Persistent failure and cross-system interactions justify frontier reasoning. Check evidence before retrying. |
| Consequential judgement | Review an authentication change for privilege escalation with no existing tests. | At least Sol, medium, with independent checks. | Unclear verification and security consequences require more care. Escalate if identity boundaries reveal coupled uncertainty. |
| Tool failure | The API credential is missing. Should we use Astra to fix the request? | No escalation for the credential failure. | A bigger model cannot supply authorised access. Obtain the missing credential through the normal secure setup. |
| Explicit preference | Use Astra even though this is a simple translation. | Respect Astra; recommend low effort. | Do not override an explicit model choice. Note that a small tier would otherwise suffice if asked about savings. |
| Different host | Recommend a model in Chat, but no model list is available. | Small/balanced/frontier tier as appropriate; exact model unresolved. | Do not assume Codex models exist in Chat. Resolve the actual catalogue before naming a model. |

Boundary check: none of these recommendations claims the active model changed. A correct interactive response presents a recommendation plus choices to switch, keep the current model or accept a cheaper trade-off, then respects the user's selection. The installation's implicit invocation setting allows discovery but does not ensure that every request activates this skill.

Interactive checkpoint example: for the one-module date parsing bug, an explicit skill mention attached to the query must produce Sol/low as the first response, explain that the failing test gives a clear causal check, offer the three choices and pause before editing. If the user chooses to keep the current model, continue without repeating the recommendation. If the user chooses to switch, tell them what to select in the model control and wait for them to say ready. If the user explicitly says “recommend first, then continue”, show the same recommendation and proceed without waiting.

Not tested: repeated independent model runs, actual task execution on each recommended model, automatic invocation rate, token use, subscription credits, latency or outcome quality. The recommendations remain heuristics.
