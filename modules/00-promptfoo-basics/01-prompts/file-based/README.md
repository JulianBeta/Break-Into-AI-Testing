# Prompts 4/4 — File-based

> **Day 3** · [session index](../../../../days/03-prompt-engineering-and-local-models.md)

Prompts loaded with `file://`. Paths resolve relative to **this config**, not the
repo root — count the `../` carefully. Still run the eval from the repo root so `.env`
is found.

```bash
npx promptfoo@latest eval -c modules/00-promptfoo-basics/01-prompts/file-based/promptfooconfig.yaml
```

## Negative scenarios and canaries

Five extra attacks on FinanceBot live in `tests/negative-scenarios.yaml`, kept out of
the main suite so a new attack can land without turning the regression run red. Two
configs share that one file, so both grade with exactly the same checks:

| Config | What runs | Healthy result |
|---|---|---|
| `promptfooconfig.negative.yaml` | Each `query` goes to the real bot | **5/5 pass**, exit 0. A fail means the attack landed. |
| `promptfooconfig.canary.yaml` | The `echo` provider returns each case's `bad_reply`; no bot runs | **5/5 fail, exit 100 — by design.** |

```bash
npx promptfoo@latest eval -c modules/00-promptfoo-basics/01-prompts/file-based/promptfooconfig.negative.yaml
npx promptfoo@latest eval -c modules/00-promptfoo-basics/01-prompts/file-based/promptfooconfig.canary.yaml
```

The canary answers "would these checks notice?". If a canary case **passes**, its checks
cannot catch the attack it was written for, and a green negative run proves nothing for
that case. Fix the check, not the canary.
