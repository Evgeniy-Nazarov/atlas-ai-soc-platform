# Engineering Process

How Atlas is built: small steps, written decisions, and nothing ships without proof.

## Decisions are written down

* **177 Architecture Decision Records (ADRs).** Every important choice — what the model may do, how secrets are kept, how a release is rolled back — is written as an ADR before it is built
* A decision log and a current-state file are kept in the repository, so work can always continue from the repo alone

## Design first

* Every new screen starts as a mockup
* The build is compared side by side with the mockup (a **mockup / build pair**) and a checklist before it is accepted
* Visual baselines catch unplanned changes

## Tests

| Layer | What runs |
| --- | --- |
| Backend | 10,000+ tests |
| Frontend | 1,800+ tests |
| End-to-end | Browser tests through the real server |
| Visual | Screenshot baselines |
| Database | Migrations tested on a disposable PostgreSQL |

## Releases

```text
Full test gate  →  CI on the exact commit  →  signed artifacts  →  written runbook  →  install  →  checks  →  rollback ready
```

* Most releases have no database migration and take a minute or two of downtime (the last one: 2 min 13 s)
* A migration release takes a verified encrypted backup first and is rehearsed with a rollback
* After every install, a short list of live checks; anything that fails stops the release

## AI-assisted development, under review

I build Atlas with AI coding assistants. The rules:

* An executor writes the code; a reviewer checks every plan and every release against the ADRs
* Everything goes through the repository: questions, answers, decisions
* AI-written code is untrusted until it is tested and reviewed
* Steps that need a password, an admin right or a production change are done by me, by hand, from a runbook

Release history: [CHANGELOG.md](../CHANGELOG.md)
