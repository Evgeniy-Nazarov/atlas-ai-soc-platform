# Lab Exercises (Purple Team)

I test my own detections in my own lab. **Atlas never runs attacks** — I do, inside a planned window.

## The cycle

```text
Plan  →  VM checkpoint  →  Window opens  →  I run Atomic Red Team  →  Alerts are marked  →  Investigate  →  After-action report
```

1. **Plan** with rules of engagement: target host, MITRE ATT&CK technique, stop condition
2. **Checkpoint** the VM on the Hyper-V host first, so the lab can be rolled back
3. **Window** — Atlas marks alerts in the window as exercise alerts and keeps them out of real metrics. Level 12 and above are never hidden
4. **Run** the Atomic Red Team test by hand on the endpoint
5. **Investigate** it like a real ticket: playbook, findings, decision
6. **Report** — Atlas writes the after-action report

## The after-action report

* Summary and timeline
* What was detected and how fast, what was missed
* Playbook steps and what I reviewed
* Findings and the evidence with SHA-256
* An optional AI second opinion — every point must cite the evidence, labelled "AI-generated"

## MITRE ATT&CK coverage

Home shows which techniques the lab has really detected (marked "lab ✓") and where the blind spots are.

## Example

The T1059.001 (PowerShell) exercise: an encoded PowerShell command on WIN11-USER-01, decoded in Atlas from event 4104 and escalated. See [Case Studies](../11-Case-Studies/).
