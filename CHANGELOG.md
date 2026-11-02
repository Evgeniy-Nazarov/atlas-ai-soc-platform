# Changelog

Atlas ships in small releases. Every release passes the full test gate and CI on the exact commit, and is installed with a written runbook and a rollback plan. This page lists the highlights, not every fix.

---

## v1.9.17 — November 2026 (installed)

* Study: the player on its own row, the translation card without opening Ask, hold Space to answer, "Heard" for the current try only
* Script blocks: long blocks scroll inside their card, and text stored with a broken encoding is shown as stored, with a hint
* Exercise report: level 12+ alerts in the window kept as real and counted for detection
* Chat and Practice layout fixes from daily use

## v1.9.16 — November 2026

* **Process chain as a table** under the graph, with hashes and actions for the selected process
* **Script blocks:** the child process first, search over every block read, suspicious patterns marked, module autoload folded
* Interview → Study redesigned: word help under the chosen word, key points, a four-step study cycle
* Reader: the hint over a word shows its first meaning

## v1.9.15 — November 2026

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
