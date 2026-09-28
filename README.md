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

## Use

```text
$query-model-router Recommend a model for this task: fix a typo in a heading.
```

Example recommendation: small tier, lowest supported reasoning effort, because the change is local and easy to check.

For an advisory workflow during execution:

```text
Use $query-model-router while helping with this task. Flag material model mismatches and keep routine answers concise.
```

Automatic discovery is enabled, but invocation on every message is not guaranteed. Explicit invocation is the more reliable way to request a recommendation. A routing-only request evaluates the quoted task without executing it.

## Validation and limitations

See [the smoke test](evals/smoke-test.md) for reproducible cases and observations. This is an in-chat self-check, not an independent benchmark. No token, credit, latency or task-quality savings have been measured. The skill's YAML metadata was parsed and checked separately.

API prices are not subscription quotas. Repeated classification can itself waste usage; do not invoke a lengthy routing discussion for every trivial question.

## Contributing

Report the query, available models, recommendation and observed outcome. Remove private information. Prefer a demonstrated failure over speculative rules. Keep the skill small and preserve its advisory boundary.

MIT licensed. Created by Niteesh Yadav with Codex.
