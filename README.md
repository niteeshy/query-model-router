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

For routine work, select the smallest suitable model and a low supported reasoning effort before sending your first request. Then invoke the skill once at the start of the session. Ask it to present a recommendation and let you choose:

```text
$query-model-router For this session, assess the task before substantial work. Show the recommended model or capability tier, supported reasoning effort, one reason, and these choices: 1) switch to the recommendation, 2) keep the current model, or 3) use a cheaper model or lower effort if available, with its trade-off. Pause so I can choose. Do not claim to switch the model yourself. For simple requests, answer directly unless a model choice would materially matter. Reassess when the task’s complexity changes.
```

Continue with your normal requests. If the assistant recommends a change, choose one of the options, select the model and reasoning effort yourself when needed, then ask it to continue. The recommendation does not change the active model.

Repeat the session-start instruction in a new chat. Within a session, invoke the skill again when the task changes substantially or you want an explicit reassessment. Avoid a separate classification request for every small message: routing also consumes usage. If you do not want to pause, say so and it will show the advice while continuing.

### Check one task before starting

```text
$query-model-router Recommend a model and reasoning effort for this task without executing it: fix a typo in a heading.
```

Example recommendation: small tier, lowest supported reasoning effort, because the change is local and easy to check. The choice is yours: switch, keep the current model, or accept a cheaper trade-off where one exists.

### Reassess after the task changes

```text
$query-model-router The task now involves a failure across several services. Reassess the model and reasoning effort before continuing, and pause if I should switch.
```

### Get advice without pausing

```text
$query-model-router While helping with this task, flag material model mismatches and keep routine answers concise. Continue the authorised work without waiting for a model change.
```

Automatic discovery is enabled, but invocation on every message is not guaranteed. Explicit invocation is the more reliable way to request a recommendation. The pause in the session-start example is a user-requested checkpoint, not an automatic model switch or a spending cap. The skill never overrides the user's choice.

## Validation and limitations

See [the smoke test](evals/smoke-test.md) for reproducible cases and observations. This is an in-chat self-check, not an independent benchmark. No token, credit, latency or task-quality savings have been measured. The skill's YAML metadata was parsed and checked separately.

API prices are not subscription quotas. Repeated classification can itself waste usage; do not invoke a lengthy routing discussion for every trivial question.

## Contributing

Report the query, available models, recommendation and observed outcome. Remove private information. Prefer a demonstrated failure over speculative rules. Keep the skill small and preserve its advisory boundary.

MIT licensed. Created by Niteesh Yadav with Codex.
