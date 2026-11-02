# Roadmap

What comes next, in order. The rule stays the same for everything new: **the analyst decides.**

---

## Now: users and roles

Until now Atlas had exactly one user: me. The next phase lets other people use it safely.

* **Accounts and sign-in:** a login and a password for each person (Argon2id hashing, a temporary password that must be changed at first sign-in, a growing pause after failed attempts).
* **Three roles:** *Owner* (everything), *Analyst* (the SOC screens of one organisation), *Learner* (only the Learning Centre).
* **Organisations as walls:** the SOC lab and a separate learning organisation. A learner never sees an alert, a host or a log.
* **Personal data follows the person:** progress, mistakes and answers belong to that user only. A role never opens another person's own learning.
* **Books by choice:** the owner ticks which shelves and books each person can open. Nothing is open by default.

The database schema for this is being built now.

---

## Next: the Learning Centre on the phone

* A native iPhone app for the Learning Centre, with the same login.
* It reaches the Mac only through a private, encrypted network tunnel, and only the learning API — never the SOC side.

---

## Then

| What | Why |
| --- | --- |
| **One controlled response action** | A single firewall block with my approval, a dry run, an audit record, a time limit and automatic rollback. The first time Atlas would change anything, so it has to be the safest possible change |
| **More exercises** | More ATT&CK techniques tested in the lab, starting with Tier 2 scenarios and the blind spots shown on Home |
| **Sensor catalogs** | Pinned Suricata and FortiGate signature catalogs, so noise rules can cover sensor alerts too |
| **A golden set** | Real lab alerts with my verdicts, checked on every release |
| **More case studies** | Written up from real alerts as they happen |
