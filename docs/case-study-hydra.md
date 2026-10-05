# Case study: a password-guessing attack on a test web app

A password-guessing attack from my lab's Kali machine against a test web application. Atlas showed me the attacker and the target, the model summarised the recorded facts with footnotes, and I confirmed that nobody got in.

This is a lab exercise: I ran the attack myself against a target built to be attacked. The point is what the workflow looks like on a real network alert, not the detection itself.

## The alert

| | |
| --- | --- |
| Rule | Wazuh 100720, level 10: "Suricata: HTTP login brute force (many POST to a login page)". My own rule over the Suricata feed |
| Technique | MITRE ATT&CK T1110, Brute Force |
| Seen by | suricata-01 (192.168.10.3), the IDS sensor on the switch's mirror port; the traffic was on VLAN 50, the DMZ |
| Attacker | 192.168.80.10, KALI-01 in the RED_TEAM network |
| Target | 192.168.50.10:80, DVWA, a deliberately vulnerable web app |
| What happened | 25 POST requests to `/dvwa/login.php` with the User-Agent `Mozilla/5.0 (Hydra)`; every one answered 302 back to `login.php` |

Two of my rules fire on this: 100710 (level 6) for each attempt, and 100720 (level 10) when ten attempts come from one source within a minute. Writing and tuning these rules on the Wazuh manager is part of the work; Atlas is only as good as the alerts it receives.

## What the analyst saw

The triage card for a network alert reads differently from one for a Windows process. It names the flow, attacker to target, and the sensor only as "seen by". Before v1.4.3 a network event was pressed into the template built for Windows process alerts, which made no sense here.

The card lists the recorded fields: source and destination, port, method, path, User-Agent, HTTP status and the redirect. The Enrich menu (public threat-intelligence lookups) offered nothing for either address. Both are private, and a private address never leaves the host; the card says so instead of pretending a lookup is possible.

When I started the investigation, the case opened with its facts in a network template, and the playbook's Collect step filled its rows from the recorded event. I did not retype anything.

| | |
| --- | --- |
| What the analyst saw | One card: KALI-01 to DVWA, 25 login attempts, all redirected back to the login page, seen by the sensor on VLAN 50 |
| What the model cited | The source, the target, the login path, the Hydra User-Agent and the 302 answers, each with a footnote to the recorded field; no verdict |
| What I decided | True positive: a brute-force attempt, no compromise. Closed with the playbook "Password brute force" |

## What the model cited

I pressed "Short triage". The answer summarised the recorded facts in a few sentences, each with a footnote pointing at the field it came from, and proposed no verdict. That is the contract: the model may describe what Atlas recorded and nothing else.

> The model's first answer failed the citation validator and Atlas refused to show it. That is exactly what the check is for.

## What I decided

True positive: a brute-force attempt, no compromise. Every one of the 25 requests was answered with a redirect to the login page, and there was no successful login. I closed the case with the "Password brute force" playbook, and the decision, the steps and the cited answer stayed in the record.
