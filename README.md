# Andesyte AI security rules

Open Semgrep rules for LLM and AI-application security, maintained by
[Andesyte](https://andesyte.com) and available for any Semgrep pipeline.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

The rules target code shapes specific to AI applications: untrusted input
entering prompts, model output reaching execution sinks, missing resource caps,
sensitive prompt logging, and tools with excessive authority.

Rules favour high-confidence findings over broad but noisy matching. The
published Andesyte CLI runs a mutation corpus and real-project false-positive
budgets before release.

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

The rules are also packaged for local, pinned use by
[`@andesyte-oss/cli`](https://www.npmjs.com/package/@andesyte-oss/cli). That
package never uses `--config auto` or downloads rules while scanning.

## Design principles

1. **High precision over recall.** Rules match observable code and data flow,
   not broad keyword heuristics.
2. **Taint mode where data flow matters.** Prompt-injection and insecure-output
   rules follow untrusted data to concrete sinks, with explicit sanitizers for
   safe message parameters, argv arrays, validation, and confirmation guards.
3. **Honest language coverage.** Python is deepest. TypeScript and JavaScript
   cover narrower framework-specific paths. Other languages are not claimed.
4. **CWE metadata.** Every rule maps findings to a CWE.

## Contributing

PRs should include:

- A `severity` (`ERROR`, `WARNING`, or `INFO`, mapped to impact metadata)
- `metadata.cwe`
- `metadata.impact: HIGH|MEDIUM|LOW`
- A vulnerable variant and a safe counterexample for the downstream mutation
  corpus before the rule is packaged in `@andesyte-oss/security-rules`

Run syntax validation with:

```bash
semgrep --validate --config rules
```

## License

MIT © Andesyte. See [LICENSE](./LICENSE).
