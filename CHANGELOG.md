# Changelog

Atlas ships in small releases. Every release passes the full test gate and CI on the exact commit, and is installed with a written runbook and a rollback plan. This page lists the highlights, not every fix.

---

## v1.9.15 — November 2026 (installed)

* **Decoded view** for Base64 and `-EncodedCommand` arguments
* **PowerShell script blocks (4104)** around an alert in one click
* Faster Alerts navigation and a live exercise run count
* Redesigned Chat and System pages (collection, hosts, AI & memory, database & backup)
* Analyst findings and AI second opinions stored as append-only records
* An Apple code-signing check among the health checks on Home

## v1.9.14 — October 2026

* **LAB-OPS:** read-only Hyper-V health (VM state and checkpoints over JEA)
* Exercise review fixes and the MITRE ATT&CK card: technique names, an alert timeline by severity
* Small UI fixes from daily use

## v1.9.0 – v1.9.13 — October 2026

* **Purple-team exercises:** plan, run window, alert queue, after-action report
* MITRE ATT&CK coverage on Home
* Security review passes on the whole platform

## v1.5 – v1.8 — October 2026

* **Learning Centre:** SOC topics, English reading and speaking, pronunciation feedback
* **Interview trainer:** study a question, answer out loud, voice mock interview
* Encrypted backup job with in-place update and rollback

## v1.4.3 — September 2026 (first public version)

* Alert collection from Wazuh with the original kept byte for byte
* Triage card, playbooks, decisions and an append-only audit
* Evidence-bound chat with a local Qwen model: every sentence cites a recorded field
* About 1.9 million lab alerts in the catalog at the time
