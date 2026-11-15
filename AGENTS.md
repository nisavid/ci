# Repository instructions

## Agent skills

### Issue tracker

Issues, PRDs, and Wayfinder maps are tracked in GitHub Issues for
`nisavid/ci`. See `docs/agents/issue-tracker.md`.

### Triage labels

Use the canonical `needs-triage`, `needs-info`, `ready-for-agent`,
`ready-for-human`, and `wontfix` labels. See
`docs/agents/triage-labels.md`.

### Domain docs

This is a single-context repository with a root `GLOSSARY.md` (the CI
vocabulary) and system-wide ADRs under `docs/adr/`, both created when domain
modeling first needs them. Neither exists yet. See `docs/agents/domain.md`.

## Git and validation

Use Conventional Commits for commits and pull request titles; Cocogitto
enforces them through the `commit-msg` and `pre-push` hooks that
`cog install-hook --all` installs. Every change requires `git diff --check`.

## Operating Policy

- This repository uses agentic engineering and operations. Agents should perform assigned tasks autonomously until they reach a boundary that requires stakeholder policy or an unavailable control surface.
- The user reserves authority over project initiatives and over initiation or continuation of work sessions. Within an active user-directed session, agents should drive execution, review loops, commits, publication steps, and cleanup unless escalation is required.
- Escalate when a decision or action impacts stakeholder concerns and the stakeholder's policy is unknown or uncertain.
- Escalate when an action must be taken but the agent lacks an autonomous control surface for it.
- When escalating a decision and a set of plausible, distinct choices is known, use a multiple-choice input tool if one is available in the interactive context. Include a way for the human operator to provide custom input.
- When escalating an action with a known prescribed path, present the steps clearly for the human operator to perform. Prefer fewer steps; present commands in easily copyable blocks, and prefer a single one-line command when practical.
- For every escalation, make the return contract clear: state exactly what result, confirmation, artifact, or output is needed to hand control back to the agent, and make it easy to validate.
- Prefer verified repository facts over guesses or aspirational guidance.
- When adding new agent-facing instructions, ask whether the information is durable, non-obvious, and useful before scouting a task.
- Remove guidance that becomes redundant with ordinary file discovery.

## Private role reentry

If you hold an existing Proto-Factory Director, Dispatch, or Sage appointment for this repository, resolve its private role entry on reentry and before covered work. Existing Codex recipients use this repository’s installed route: read the matching absolute path using `git config --local --get` with `ci-codex.directorEntry`, `ci-codex.dispatchEntry`, or `ci-codex.sageEntry`. Resolve relative references from the private entry's directory. Follow its role and admission references and recover current holds. If the pointer or recipient binding is missing or mismatched, preserve the actual assignment and return the specific missing binding to the entry owner before an affected start. Other harnesses follow their own accepted role-entry bindings. Workers and other recipients follow their actual assignment and admission route. A pointer supplies instructions for an existing appointment; it does not create one.
