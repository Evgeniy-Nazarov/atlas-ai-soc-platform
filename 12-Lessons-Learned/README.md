# Lessons Learned

## 1. Prove whose bug it is before fixing it

* **What happened:** one night the lab logged about 11,500 logons by the LAB-OPS account in 3.5 hours
* **What I did:** checked my own side first — LAB-OPS opens one session per cycle, it cannot make thousands of logons. The pattern points to repeated delivery on the log side; I am confirming it with read-only checks on Wazuh
* **Lesson:** a scary number is a question, not an answer

## 2. "It works" is not "it's done"

* **What happened:** a Learning Centre screen shipped working, but not matching its approved design — and I accepted it
* **What changed:** every screen now ships with a mockup / build pair side by side and a checklist
* **Lesson:** check it against what was agreed, not only that it runs

## 3. A gap must look like a gap

* **Why:** in a SOC, "no alerts" and "no data" mean very different things
* **How:** missing time windows are shown as gaps; "not found" is only said where data was collected
* **Lesson:** an honest "I don't know" beats a quiet screen that looks safe

## 4. Small releases win

* **Numbers:** about 50 releases in two months, most without a database migration
* **How:** each with a runbook and a rollback, a couple of minutes of downtime
* **Lesson:** small, boring releases are the safe ones
