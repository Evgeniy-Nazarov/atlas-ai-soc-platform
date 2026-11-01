# Investigation Workflow

From an alert to a decision, with the evidence on one screen.

```text
Alert  →  Triage card  →  Evidence (chain, Decoded, 4104)  →  Case + playbook  →  Decision  →  Closing summary
```

## Triage card

When I open an alert, Atlas runs a set of named, read-only checks under one time budget:

* **What happened:** rule, user, host, process and parent
* **How often:** the same rule in the last day, week and month
* **How rare:** on how many hosts it was seen
* **What I decided before** on similar alerts
* **Collection coverage:** whether the time window was fully collected

A check that cannot finish in time is shown as unavailable with the reason — never silently dropped.

## Evidence in one click

| Tool | What it does |
| --- | --- |
| **Process chain** | The verified parent → child chain as a table, with hashes and command lines |
| **Decoded** | Decodes Base64 and `-EncodedCommand` arguments; marked as a derived view |
| **Script blocks (4104)** | The PowerShell script blocks around the alert, the child process first, with search |
| **Host network** | The host's connections around the alert |
| **Threat intel** | VirusTotal / AbuseIPDB for one public value, only on my click; private IPs are refused |
| **Inspector** | The raw event with its SHA-256 |

## Cases and playbooks

* A Board of open investigations
* A playbook I wrote, step by step; every step outcome is written by me and kept as history
* **+ Finding** copies a value straight from an event field, with the event's SHA-256
* A run that skipped a required step closes as "completed with gaps", with the list of what was not checked
* A closing summary is written over the closed record only

## The decision is mine

Close as expected activity, close as a false positive, or escalate. The AI can explain; it cannot decide.
