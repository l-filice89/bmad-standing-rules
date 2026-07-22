# bmad-standing-rules

Portable standing rules for `bmad-dev-auto`, harvested from a real production
project's 13 epic retrospectives (2026-07) in a retro-of-retros. Sixteen rules,
each born from a real incident; the incident stays in the wording because it is
the rationale. The collection is living — new projects feed new rules in.

## Install into a project

Copy the file — do **not** clone this repo into `_bmad/custom/` (that folder
also holds per-project `config.toml` files):

```sh
curl -fsSL https://raw.githubusercontent.com/l-filice89/bmad-standing-rules/main/bmad-dev-auto.toml \
  -o _bmad/custom/bmad-dev-auto.toml
```

or from a local clone:

```sh
cp ~/Development/bmad-standing-rules/bmad-dev-auto.toml <project>/_bmad/custom/
```

If the project already has a `_bmad/custom/bmad-dev-auto.toml` with its own
facts, merge the `persistent_facts` arrays by hand.

## What's inside

| Rule | One-liner |
|------|-----------|
| HAZARD-TEST | AC names a hazard → a test asserts exactly it, red-then-green |
| E2E-COVERAGE | Every UI-flow AC ships an e2e test or a coverage-map row, same story |
| PROBE-BEFORE-YOU-MAP | Probe real provider failure modes; fixtures from captured payloads |
| DEGENERATE-RESPONSE GUARD | Empty-but-valid external data fails closed before it drives a write |
| SAMPLE-OF-ONE | Never pin a population constant from one observed instance |
| RESOURCE-BUDGET | Budget claims enumerate every consumer, with arithmetic written out |
| TEST-THE-BYPASS | Guards get tests for the way through, not just the refusal |
| CAPABILITY-IS-NOT-AN-IDENTIFIER | Server-published values prove nothing; mint capabilities |
| FOLLOW-UP-REVIEW CONTRACT | Self-stamped stories need an independent pass before merge |
| FOLLOW-UP-REVIEW AUTO-FORCE | A HIGH finding forces that pass, non-declinable |
| DELEGATED-WORK-CARRIES-AN-AC | Cross-story delegation becomes an AC in the receiving story |
| UI-MOCK-GATE | UI-touching specs carry a signed-off placement mock before code |
| EXTERNAL-RISK-FLAG | Third-party touches carry a signed-off ToS/credential/legal risk flag |
| WCAG NUDGE | Interactive widgets name their ARIA pattern in the spec |
| POST-EPIC OPERATOR SWEEP | Pre-merge pass over the epic as a running system: ledger, time, budgets |
| PRESERVE-VS-CLEAR | Partial updates rule absent-vs-clear per field; 200-[] ruled per endpoint |

Not included: the bmad-loop foreground-only orchestration constraint — it is a
workaround for a specific orchestrator and belongs only in projects that use it.

## Enforcement model

These are prompt-level facts injected into every `bmad-dev-auto` session — not
deterministic gates. They hold because they are worded as BLOCK/finding checks
at named workflow steps (step-02 spec gate, step-03 implementation, step-04
review), which the agent treats as a checklist. Observed miss rate across 8
epics in the origin project: zero.
