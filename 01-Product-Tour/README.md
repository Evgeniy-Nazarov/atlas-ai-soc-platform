# Product Tour

## Why I built Atlas

Wazuh in my lab fires thousands of alerts a day. Most are routine, a few matter, and telling them apart means opening raw JSON, checking the host, the user, the process and its parent, and remembering what I decided last time. The first time I worked an alert like that by hand, it took me about 30 minutes ([case study 92205](../11-Case-Studies/powershell-file-write-92205.md)).

Cloud AI was not an option: command lines and internal names are exactly the data I will not send to a third party. And a model that summarises logs freely will invent a parent process or a verdict. In security work, a confident wrong answer is worse than no answer.

So Atlas keeps the AI next to the data and in its place: it reads recorded evidence and explains. **I decide.**

## What Atlas does not do

* It does not block, isolate or change anything in the lab
* It does not close or escalate an alert on its own
* It does not run attacks — I run lab exercises myself
* It does not send logs, command lines or internal names to the internet

## The main screens

Screenshots are being refreshed for v1.9.16. Coming here:

| Screen | What it shows |
| --- | --- |
| Home | Detection posture, MITRE ATT&CK view of the lab, alerts over time, lab health |
| Alerts | Every alert with search, filters and a timeline by severity |
| Alert card | What happened, process chain, decoded command, script blocks, threat intel on click |
| Investigations | Board, playbook, workspace, closing summary |
| Exercise review | After-action report with evidence and an optional AI second opinion |
| Learning Centre | Study, practice out loud, mock interview |
| System | Collection, hosts, AI & memory, database & backup |

A short video walkthrough will be added here as well.
