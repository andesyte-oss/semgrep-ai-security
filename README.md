# semgrep-ai-security

> Open Semgrep ruleset for LLM and AI-application security. Maintained by Foundation Machines for [Sebastion AI](https://foundationmachines.ai) and freely usable in your own Semgrep pipeline.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

The OWASP LLM Top 10 (2026) and the AI Incident Database keep growing, but the Semgrep ecosystem has *no* widely-adopted free ruleset that covers AI-app-specific bugs: prompt-injection sinks, unsanitised tool-calls, RAG-poisoning vectors, secret leakage in system prompts, agent-loop bombs. This repo fills that gap.

Rules are designed to be **diff-friendly** — they prefer high-confidence matches over recall — so they're safe to run as an inline PR reviewer on real codebases.

## Coverage

| Rule pack | OWASP LLM Top 10 | Status |
|---|---|---|
| `prompt-injection.yml` | LLM01 | ✅ |
| `insecure-output-handling.yml` | LLM02 | ✅ |
| `training-data-poisoning.yml` | LLM03 | 🚧 |
| `model-dos.yml` | LLM04 | ✅ |
| `supply-chain.yml` | LLM05 | 🚧 |
| `sensitive-info-disclosure.yml` | LLM06 | ✅ |
| `insecure-plugin-design.yml` | LLM07 | ✅ |
| `excessive-agency.yml` | LLM08 | ✅ |
| `overreliance.yml` | LLM09 | 🚧 |
| `model-theft.yml` | LLM10 | 🚧 |

## Use it

```bash
semgrep --config https://raw.githubusercontent.com/andesyte-oss/semgrep-ai-security/main/ai-security.yml ./
```

Or pin to a specific commit for stability:

```bash
semgrep --config https://raw.githubusercontent.com/andesyte-oss/semgrep-ai-security/<sha>/ai-security.yml ./
```

Inside [Sebastion AI](https://foundationmachines.ai), this ruleset is enabled automatically. Disable it per repo with:

```yaml
# .sebastion.yml
disable_scanners:
  - semgrep
```

## Design principles

1. **High precision over recall.** Every rule is gated on observable code shape, not heuristics. We'd rather miss a finding than ship a false positive that gets the bot muted.
2. **No taint mode** in v1. Semgrep OSS taint mode has too many false positives on Python/TS LLM frameworks. We may add v2 rules using deep-semgrep once it stabilises.
3. **Language coverage**: Python first (langchain / llamaindex / openai / anthropic / litellm). TypeScript second (Vercel AI SDK, OpenAI Node, Anthropic Node, AI SDK Core). Adding more on demand.
4. **CWE-tagged**: every rule maps to a CWE in `metadata.cwe` so findings render with a proper CWE link in PR comments.

## Contributing

PRs welcome. Each rule must include:

- A `severity` (`ERROR` / `WARNING` / `INFO` mapped to our impact metadata)
- `metadata.cwe` (e.g. `CWE-77: Command Injection`)
- `metadata.impact: HIGH|MEDIUM|LOW`
- A minimal positive and negative test case in `tests/<rule-id>/`

Run tests with `semgrep --test`.

## License

MIT © Foundation Machines Ltd. See [LICENSE](./LICENSE).
