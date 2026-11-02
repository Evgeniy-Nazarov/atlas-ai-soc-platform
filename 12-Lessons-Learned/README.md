# Lessons Learned

What broke, what surprised me, and what I changed because of it.

---

## 1. Prove whose bug it is before fixing it

**What happened:** one night the lab logged about 11,500 logons by the LAB-OPS account in 3.5 hours. LAB-OPS is the Atlas component that reads Hyper-V health, so Atlas looked guilty.

**What I did:** I checked my own side first. LAB-OPS opens one session per ten-minute cycle, which is about ten logon events per cycle; the storm was roughly fifty times that. The pattern pointed to repeated delivery on the log side, not to Atlas. I am confirming it with read-only checks on Wazuh.

**Lesson:** a scary number is a question, not an answer. Measure the normal rate first, then compare.

---

## 2. "It works" is not "it's done"

**What happened:** a Learning Centre screen shipped working, but not matching its approved design — and I accepted it.

**What changed:** every screen now ships with a mockup / build pair side by side and a checklist. The designs are drawn by the reviewer, not by the one who builds the screen.

**Lesson:** check it against what was agreed, not only that it runs.

---

## 3. A gap must look like a gap

**Why:** in a SOC, "no alerts" and "no data" mean very different things. A host whose agent died looks exactly like a quiet, healthy host.

**How:** missing time windows are shown as gaps; hosts are shown as OK, QUIET or NO DATA; "not found" is only said where data was actually collected.

**Lesson:** an honest "I don't know" beats a quiet screen that looks safe.

---

## 4. A refusal must say why

**What happened:** I asked the local model for a second opinion on an exercise report. The answer was refused by Atlas's citation check — correctly — but the message was a small grey line that said "cited nothing", and it did not explain the real reason.

**What changed:** a refusal now has to name its reason (not valid JSON, no point cited a record, a point contained a command) with counts, and it is shown next to the button.

**Lesson:** a safety check that the user cannot understand looks like a bug.

---

## 5. Measure during the change, not after it

**What happened:** during an install, the monitor that watches the database through the cutover sat waiting for my password, so it took no measurements while the change was happening.

**What changed:** the cutover now refuses to start until the monitor has taken a recent sample.

**Lesson:** a monitor that starts after the risky moment is not a monitor.

---

## 6. A citation is not proof

**What happened:** while taking screenshots for this repository, I asked the local model for a short triage of an encoded PowerShell alert. Every sentence cited a real field, and Atlas marked the answer *Validated*. But one sentence said the command ran `whoami`. It did not: the decoded command only printed *Hello, from PowerShell!*. The citation pointed to the right field; the words did not match it.

**What I am changing (the next release):** the check goes one level deeper. Every value a sentence names (a program, a file, a quoted command, a hash, an IP) must actually appear in the record it cites. If it does not, the sentence is not shown, and *Validated* appears only when every sentence passed.

**Lesson:** checking that the AI cited something is not the same as checking that the source says it. Verify the words, not just the footnote.

---

## 7. Small releases win

**Numbers:** about 50 releases in two months, most without a database migration, each with a runbook and a rollback, and about two minutes of downtime.

**Lesson:** small, boring releases are the safe ones. When something goes wrong, the change to look at is small.
