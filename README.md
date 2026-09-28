# Query Model Router

A lightweight Codex skill that recommends the smallest suitable model and reasoning effort for a query.

**Advisory only.** It cannot intercept a request before model selection, switch the model answering the current turn or enforce a usage cap. Start routine work on an efficient model to obtain the intended savings.

## How it chooses

| Tier | Typical work |
| --- | --- |
| Small | Clear questions, extraction, formatting, summaries and focused edits |
| Balanced | Debugging, related file changes, source comparison and nuanced judgement |
| Frontier | Difficult proofs, persistent reasoning failures and tightly coupled uncertainty |

It considers ambiguity, dependencies, consequences and verification, rather than prompt length. Missing files and broken tools do not justify a bigger model. Reasoning effort is also kept proportionate.

The skill includes a dated model mapping and uses the host's available models. It does not assume every ChatGPT surface offers the same catalogue.

## Install in Codex

Ask Codex with the skill installer available:

```text
Use $skill-installer to install the skill at https://github.com/niteeshy/query-model-router
```

Or install manually on macOS/Linux, stopping if the destination already exists:

```sh
skill_dir="${CODEX_HOME:-$HOME/.codex}/skills/query-model-router"
if [ -e "$skill_dir" ]; then
  echo "Skill already exists; review it before updating."
else
  mkdir -p "$(dirname "$skill_dir")"
  git clone https://github.com/niteeshy/query-model-router.git "$skill_dir"
fi
```

If the skill does not appear, start a new Codex session. This local installation does not install it in ChatGPT web or mobile.

## Use during a session

For routine questions, select GPT-6 Luna with low reasoning before sending the question. Do this manually in the model control. If Luna is unavailable, select the smallest available equivalent with low supported effort.

Use the skill when you are uncertain whether the task is routine, when the task is ambiguous or consequential or when it may need deeper reasoning. Invoke it before the task and let it recommend first:

```text
$query-model-router Assess this task before answering or acting. Show the recommended model or capability tier, supported reasoning effort, one reason, and these choices: 1) switch to the recommendation, 2) keep the current model, or 3) use a cheaper model or lower effort if available, with its trade-off. Pause so I can choose. I will change the model myself before asking you to continue.
```

Continue with your normal requests. If the assistant recommends a change, choose one of the options, select the model and reasoning effort yourself when needed, then ask it to continue. The recommendation does not change the active model.

An explicit skill mention always recommends first, even when attached after the query. Avoid invoking it for every small message because routing also consumes usage. If you do not want to pause, say “recommend first, then continue without waiting for my choice”. Automatic routing is outside this skill because model selection must happen before the main request runs.

### Check one task before starting

```text
$query-model-router Recommend a model and reasoning effort for this task without executing it: fix a typo in a heading.
```

Example recommendation: Luna with low reasoning, because the change is local and easy to check. The choice is yours: switch, keep the current model, or accept a cheaper trade-off where one exists.

### Reassess after the task changes

```text
$query-model-router The task now involves a failure across several services. Reassess the model and reasoning effort before continuing, and pause if I should switch.
```

### Get advice without pausing

```text
$query-model-router Recommend first, then continue without waiting for my choice. Keep routine answers concise and flag material model mismatches while helping with this task.
```

Automatic discovery is enabled, but invocation on every message is not guaranteed. Explicit invocation is the reliable way to request a recommendation first. The default pause is a user-choice checkpoint, not an automatic model switch or a spending cap. The skill never overrides the user's choice.

## Validation and limitations

See [the smoke test](evals/smoke-test.md) for reproducible cases and observations. This is an in-chat self-check, not an independent benchmark. No token, credit, latency or task-quality savings have been measured. The skill's YAML metadata was parsed and checked separately.

API prices are not subscription quotas. Repeated classification can itself waste usage; do not invoke a lengthy routing discussion for every trivial question.

## Contributing

Report the query, available models, recommendation and observed outcome. Remove private information. Prefer a demonstrated failure over speculative rules. Keep the skill small and preserve its advisory boundary.

MIT licensed. Created by Niteesh Yadav with Codex.
