# Security

Atlas runs AI next to real logs. So the first rule was: **the AI can never be the source of truth, a way to change things, or a way for data to leave.**

## The AI never decides

| Rule | How |
| --- | --- |
| Decisions are human | Only I can close, escalate or record a step. AI output has no route there |
| Every claim is checkable | Every sentence must cite real evidence, or the answer is rejected |
| Atlas counts, the AI explains | Frequency, rarity and process chains are computed, not guessed |
| No hidden AI | The model runs only when I ask. No background jobs, no agent loop |
| Unknown stays unknown | Missing data is shown as a gap, not as "nothing found" |

## Log text is data, not commands

A log can contain text an attacker wrote. Atlas passes it to the AI as labelled data, removes hidden model control tokens, and flags the attempt on the answer card.

## Deny by default

* Every action needs a named permission
* Everything runs on 127.0.0.1 — nothing listens on the network
* The AI model has no network access at all

## Evidence integrity

| What | How |
| --- | --- |
| Original event | Kept byte for byte with its SHA-256 |
| Derived views | Decoded commands and AI answers are labelled as derived |
| Findings | Copied from an event field, with that event's SHA-256 |
| History | Decisions, findings and steps are append-only |

## Secrets

* Kept in the macOS Keychain
* Only one small signed broker can read them — not the app, not the AI, not the tests
* Never written to logs, prompts or the browser

## Signed and checked

* The runtime, the web app and the broker are code-signed
* Every start and every install checks the signature; a mismatch stops instead of degrading

## Backups

* Daily, encrypted
* A backup counts only after a real test restore

## Local-first

* Logs, private addresses and internal names stay on the Mac
* Threat intel is one public value per click; private IPs are refused

## Not built yet

* Roles beyond the single owner
* Any action toward the lab (for example a firewall block) — only with approval, a dry run, a time limit and automatic rollback
