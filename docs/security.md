# Security design

Atlas runs a language model next to real logs, so the first requirement was that the model can never become a source of truth, a write path or an exit for data. The rules below are enforced in code and in the database schema, not in prompt wording.

## The model never decides

| Rule | How it is enforced |
| --- | --- |
| Decisions are human | Alert resolution, investigation closure and playbook step outcomes are written only by a human principal. Model output has no route to those tables |
| Every claim is checkable | Answers are strict JSON; each sentence cites server-issued handles (event fields, card facts, computations). Citations are validated before display; an answer citing what it was not given is rejected |
| Atlas counts, the model explains | Frequencies, rarity and lineage are computed deterministically and labelled as Atlas facts. The model is never asked to count |
| No hidden generation | The model runs only on an explicit question or button. No background summaries, no automatic retries |
| No agent loop | At most one bounded planning pass selects from fixed server-side query templates. The model cannot write SQL, call URLs, run commands or widen the time range |
| Unknown stays unknown | Uncollected windows and unavailable actions are shown as gaps. "Not found" is claimed only inside collected windows |
| AI is optional | If the model is unavailable, every saved fact and decision is still there and the case can be closed |

## Deny by default

- Every data route requires an authenticated principal with an explicit permission; there is no implicit read. Service accounts (collection, search preparation) carry their own narrow permissions and never act as the analyst.
- Every table and every query is scoped to one organization.
- The frontend is a static export served by the API process on loopback from one origin: no cross-origin requests, no CDN, a Content-Security-Policy limited to the application's own origin, and an Origin check plus a CSRF token on every write.
- Nothing listens beyond the loopback address. The model worker has no network at all.

## Immutable evidence and audit

- The original alert is stored byte for byte with its SHA-256. Evidence snapshots inside an investigation are immutable.
- Decisions, playbook step revisions and findings are append-only journals. Current state is a projection; history is never rewritten.
- Collection cycles, evidence reads, decisions, playbook steps and enrichment requests are audited with actor, time and correlation. The collection attempt journal lives in a fixed-size file outside the database, so it survives a database restore.

## Log content is data, not commands

A SOC log is attacker-controlled text by definition: a User-Agent, a URL, a file name, a command line. Atlas treats it as data.

- Evidence enters the prompt as typed, labelled fields inside a versioned template, never as free text the model is told to follow.
- The answer schema is strict and the validator rejects anything that cites outside the supplied evidence, so an injected instruction cannot smuggle a verdict in.
- Control tokens (v1.4.1). I proved with the pinned tokenizer that evidence text could become real control tokens in the model's prompt. Now one check sits in front of every model call: the control strings generated from the tokenizer files are neutralised in data, in plain, upper-case, full-width and zero-width-split forms, kept, and flagged on the answer card as a prompt-injection attempt in data. A typed question that contains one is refused.

## Secrets only in the Keychain

- Provider and database credentials live in the macOS Data Protection Keychain, in an access group that belongs to exactly one executable: a separately signed credential broker embedded in the runtime.
- The runtime, its embedded Python, provider adapters, shells and tests have no access. Secrets are delivered over a private XPC connection, held in memory for the request, and never written to logs, audit records, prompts or API responses.
- The database has separate least-privilege roles: the runtime role cannot migrate, the migration credential is delivered only to a signed migration helper, and the schema owner cannot log in.

## Signed, verified runtime

- The backend runtime, the frontend snapshot and the broker are code-signed artifacts with immutable manifests. Installation and every peer connection check the exact code identifier, the signing certificate hash and the designated requirement; a mismatch stops the operation instead of degrading.
- A release candidate is installed only after the quality workflow has passed on that exact commit.
- Steps that need administrator rights, a Keychain approval or a password run only in an attended window with a written runbook.

## Backups that have been restored

Backups go to a dedicated encrypted volume. A backup counts only after a real restore into an isolated database, with digest comparison and migration-head validation. Restores were rehearsed before the first production install and again before the v1.4 install, as the rollback path.

## Local-first data handling

- Raw alerts, private addresses and internal names stay on the machine.
- Threat-intelligence lookups are opt-in per value and per click. A private, loopback, link-local, multicast, reserved or documentation address is refused before any network call, and the card says "never sent". Internal host names are never sent. Local rate budgets stay below each provider's public limit.
- The chat's general mode, for concept questions, has no access to lab data at all.

## Bounded everything, fail closed

Page counts, bytes, time windows, context sizes and provider budgets have fixed limits. Hitting a limit is a named, visible outcome, not a partial result presented as complete. The model worker admits a request only when memory, swap growth and the context budget allow it.

## What is not there yet

- Roles beyond the single owner: designed, not built.
- Any write toward the lab (a firewall block, an agent response, an account disable): proposed only, and it requires explicit approval, a dry run, an audit record, a time limit with automatic rollback and a protected never-target list before a single action ships.
- Automatic noise suppression: the proposal allows human-approved suggestions only.
