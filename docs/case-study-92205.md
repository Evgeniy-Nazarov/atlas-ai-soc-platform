# Case study: a PowerShell file write that looked like a loader

A Windows alert from a workstation in my lab: PowerShell, running as SYSTEM, wrote a script file into a system folder. That is what malware staging looks like. It was the Wazuh agent's own security configuration check. I closed it as expected activity, and it took four releases to get the workflow from half an hour of clicking to a few minutes.

## The alert

| | |
| --- | --- |
| Rule | Wazuh 92205: "Powershell process created an executable file in Windows root folder" (Sysmon Event ID 11, file created), tagged T1105 |
| Host | WIN11-USER-01, a domain-joined Windows 11 workstation with Sysmon and a Wazuh agent |
| Process | `C:\Windows\SysWOW64\WindowsPowerShell\v1.0\powershell.exe` (32-bit PowerShell) |
| User | `NT AUTHORITY\SYSTEM` |
| File | `C:\Windows\SystemTemp\__PSScriptPolicyTest_<random>.ps1` |

Why it looks alarming: PowerShell as SYSTEM writing a script into a system temp folder is the shape of a loader. Why it is probably nothing: every start of PowerShell writes a `__PSScriptPolicyTest` file to test the execution policy, and something on this host starts 32-bit PowerShell as SYSTEM all day. Telling the two apart needs the parent process, the frequency and what the host did on the network at that moment.

## What the analyst saw

The first time, on the pre-v1 workspace, this took about 30 minutes. The reasons were all workflow: ten separate forms with hand-typed notes, the data I needed on other screens, a wrong skip reason that could not be corrected, and a triage card that was empty for this event shape.

I set a target of 2 to 3 minutes and no more than 15 actions, and wrote it into a browser test that counts clicks and fails above 15. The redesigned workspace put the alert, the triage card, the playbook run, the evidence panel and the chat on one screen. The replay closes this case in 13 actions.

Then I ran it on the real alert, and the real data found what the synthetic one had not:

| What I saw | What it was |
| --- | --- |
| Lineage "depth 0", "nothing found", no command line | The parent is `wazuh-agent.exe`, a service started at boot. Lineage follows process-start events inside a 24-hour window, and a service started days ago has none. Fixed: the card now names the parent as a long-lived service root and says why the chain ends |
| "Host not in registry" for a host I had registered | Full name versus short name. Fixed: both resolve to the same record |
| Most playbook steps showed a header only | Steps now render their collected data and outcome controls |
| A bare ATT&CK id with no context | Shown as "Wazuh rule tag T1105" |

| | |
| --- | --- |
| What the analyst saw | PowerShell as SYSTEM writing `__PSScriptPolicyTest_*.ps1` into `C:\Windows\SystemTemp`; parent `wazuh-agent.exe`; the same behaviour on other hosts all day |
| What the model cited | On the first run, nothing useful: the chat was not linked to the case and the summary restated the rule description. Later, one Explain turn refused rather than answer from a cut-down context; the recorded facts stood on their own |
| What I decided | Expected activity: correctly detected, and authorised. The Wazuh agent's configuration check runs 32-bit PowerShell as SYSTEM, and every PowerShell start writes that file |

## What the model cited

Honestly, in this case the model was the least useful part, and that is fine.

> On the first run the chat was not linked to the investigation, and the AI summary restated the rule description instead of the evidence. On a later run the Explain turn stopped with `MODEL_UNAVAILABLE` after 191 seconds: the question did not fit the model's context budget, and the worker refused rather than answer from a truncated context. The saved facts, the steps and the card were still there, so the case never depended on the model.

A short triage of the sibling rule 92066 once ended in `CITATION_INVALID`: the answer cited something it had not been given, and Atlas rejected it. That is the validator doing its job.

## What I decided

Expected activity. The parent of the PowerShell process is the Wazuh agent itself; the agent's security configuration assessment runs 32-bit PowerShell as SYSTEM, and every PowerShell start writes the policy-test file. For SYSTEM on Windows 11 it lands in `SystemTemp`, which is why the same behaviour fires a different rule on the domain controller.

What Atlas did not do: it did not write to Wazuh. The exclusion rule that would drop exactly this process, user and path combination exists as a draft and stays unapplied until I apply it by hand on the manager. Atlas and the model have no write path to Wazuh or to any lab host.

## What this case changed

- The one-screen investigation workspace and the "at most 15 actions" browser test.
- Lineage that explains why a chain ends instead of reporting "depth 0".
- Host registry matching by both short name and full name.
- A threat-intelligence lookup per playbook step, starting with the process image hash.
- A documented, unapplied exclusion draft, and the rule that Atlas never applies one.
