# bmad-standing-rules

Portable standing rules for BMAD dev flows, harvested from a real production
project's epic retrospectives in a retro-of-retros. Sixteen rules, each born
from a real incident; the incident stays in the wording because it is the
rationale. The collection is living — new projects feed new rules in.

## Layout

Rules live once, in two markdown files; thin per-skill TOML stubs include them
via the `file:` mechanism of `persistent_facts`:

| File | Contents | Loaded by |
|------|----------|-----------|
| `standing-rules-core.md` | 12 flow-agnostic engineering rules | all three flows |
| `standing-rules-epic-process.md` | 4 rules tied to epic/story machinery | dev-auto, dev-story |
| `bmad-dev-auto.toml` | include stub | `bmad-dev-auto` |
| `bmad-dev-story.toml` | include stub | `bmad-dev-story` |
| `bmad-quick-dev.toml` | include stub (core only) | `bmad-quick-dev` |

## Install into a project

Copy all five files into `<project>/_bmad/custom/` — do **not** clone the repo
into that folder (it also holds per-project `config.toml` files).

macOS / Linux / Windows Git Bash:

```sh
cd <project>/_bmad/custom
for f in standing-rules-core.md standing-rules-epic-process.md \
         bmad-dev-auto.toml bmad-dev-story.toml bmad-quick-dev.toml; do
  curl -fsSLO https://raw.githubusercontent.com/l-filice89/bmad-standing-rules/main/$f
done
```

Windows PowerShell:

```powershell
cd <project>\_bmad\custom
'standing-rules-core.md','standing-rules-epic-process.md',
'bmad-dev-auto.toml','bmad-dev-story.toml','bmad-quick-dev.toml' | ForEach-Object {
  Invoke-WebRequest "https://raw.githubusercontent.com/l-filice89/bmad-standing-rules/main/$_" -OutFile $_
}
```

Or, on any OS, clone somewhere else and copy:

```sh
git clone https://github.com/l-filice89/bmad-standing-rules
cp bmad-standing-rules/standing-rules-*.md bmad-standing-rules/*.toml <project>/_bmad/custom/
```

(PowerShell: same with `copy` instead of `cp`.)

**Careful:** each download replaces any same-named file already in
`_bmad/custom/`. If the project already has one of the per-skill TOMLs with its
own `persistent_facts`, do NOT download that stub — instead open the existing
file and add the two `file:` lines to its array:

```toml
persistent_facts = [
  "file:{project-root}/_bmad/custom/standing-rules-core.md",
  "file:{project-root}/_bmad/custom/standing-rules-epic-process.md",
  "your existing project-specific facts stay here...",
]
```

## What's inside

Core — any flow:

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
| UI-MOCK-GATE | UI-touching specs carry a signed-off placement mock before code |
| EXTERNAL-RISK-FLAG | Third-party touches carry a signed-off ToS/credential/legal risk flag |
| WCAG NUDGE | Interactive widgets name their ARIA pattern in the spec |
| PRESERVE-VS-CLEAR | Partial updates rule absent-vs-clear per field; 200-[] ruled per endpoint |

Epic process — dev-auto and dev-story only (they need merge gates, review
stamps, cross-story delegation, and a deferred-work ledger to bind to):

| Rule | One-liner |
|------|-----------|
| FOLLOW-UP-REVIEW CONTRACT | Self-stamped stories need an independent pass before merge |
| FOLLOW-UP-REVIEW AUTO-FORCE | A HIGH finding forces that pass, non-declinable |
| DELEGATED-WORK-CARRIES-AN-AC | Cross-story delegation becomes an AC in the receiving story |
| POST-EPIC OPERATOR SWEEP | Pre-merge pass over the epic as a running system: ledger, time, budgets |

Not included: the bmad-loop foreground-only orchestration constraint — it is a
workaround for a specific orchestrator and belongs only in projects that use it.

## Feeding the collection

`skills/bmad-retro-of-retros/` is a Claude Code skill that runs the harvest
this repo was born from: at project end it collects retro-born rules from
`_bmad/custom/*.toml` and the epic retros, classifies them (core /
epic-process / project-specific), generalizes the travelers, gets your ruling
on each, and commits the survivors here. Install it per project:

```sh
cp -r bmad-standing-rules/skills/bmad-retro-of-retros <project>/.claude/skills/
```

Then say "retro of retros" in a session inside the project.

Contributions land as PRs against `main` — the branch is protected (no direct
pushes, maintainer merge is the last word). The skill does this automatically;
manual contributions follow the same route.

## Enforcement model

These are prompt-level facts injected into every dev session — not
deterministic gates. They hold because they are worded as BLOCK/finding checks
at named workflow moments (spec time, implementation, review), which the agent
treats as a checklist. Observed miss rate across 8 epics in the origin
project: zero.
