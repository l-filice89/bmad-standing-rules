---
name: bmad-retro-of-retros
description: Harvest retro-born rules from a finished BMAD project, judge which generalize, and feed the survivors into the shared bmad-standing-rules repo. Use at project end (or after several epics) when the user says "retro of retros", "harvest the retro rules", or "feed the standing rules".
---

# Retro of Retros

Meta-retrospective over a project's epic retrospectives. Goal: any rule the
project minted that would hold in OTHER projects gets generalized and fed into
the shared standing-rules collection; project-specific rules stay behind.

## Inputs

1. **Project rules** — `persistent_facts` in every `{project-root}/_bmad/custom/bmad-*.toml`
   (skip `file:` entries pointing at standing-rules files — those are the
   collection itself, already harvested).
2. **Retros** — the project's retrospective artifacts (typically
   `epic-*-retro-*.md` under the implementation-artifacts folder; resolve via
   `_bmad/scripts/resolve_config.py` if unsure). Cross-check that every rule
   named in a retro made it into a toml — a rule born but never persisted is
   still a candidate.
3. **The collection** — the user's clone of the standing-rules repo (ask for
   the path if unknown). Read `standing-rules-core.md` and
   `standing-rules-epic-process.md` so you know what already exists.

## Procedure

1. **Harvest**: list every fact/rule from inputs 1–2 that is not already in
   the collection. For each, find its origin incident in the retros — the scar
   is the rationale and must survive into the generalized wording.
2. **Classify** each candidate:
   - **core** — flow-agnostic engineering rule, holds in any project
   - **epic-process** — holds anywhere epic/story machinery exists (merge
     gates, review stamps, delegation, deferred-work ledger)
   - **project-specific** — bound to this project's tooling or context; stays
     in the project's toml, does not travel
3. **Generalize** the travelers:
   - person names → role names ("the product owner")
   - project/product names, epic/story numbers, dates → dropped; incidents
     retold generically ("the origin bug", "an early epic")
   - tooling specifics → framework-agnostic, keeping the original as an
     illustration when it teaches (e.g. a platform's request ceiling)
   - workflow step names (`step-03`) → generic moments ("at spec time",
     "during implementation", "at review")
   - check for OVERLAP with an existing rule: extend the existing rule's
     wording rather than adding a near-duplicate; siblings cross-reference
     each other by name
4. **Present for rulings** — a table: rule, origin scar, classification,
   proposed generalized wording (or the diff against an existing rule).
   The user rules on each: adopt / rework / drop. Do not write until ruled.
5. **Feed the repo**: append adopted rules to the matching markdown file in
   the clone, update the README's rule table, commit on a branch and push
   (PR if the repo requires it). One commit, message naming the source
   project's epics harvested.

## Constraints

- Never weaken an existing collection rule during a merge — extending is
  additive; a rewording that narrows scope needs an explicit user ruling.
- Scars stay: a rule stripped of its incident reads as cargo cult and dies
  in review. Anonymize the scar, never delete it.
- This skill only feeds the collection. Installing the updated collection
  into projects is the README's install procedure, not this skill's job.
