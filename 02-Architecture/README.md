# Architecture

## Atlas + SOC Lab

Atlas (left) runs on the Mac Studio inside my [SOC homelab](https://github.com/Evgeniy-Nazarov/soc-homelab-enterprise) (right). The numbers show how the two work together.

<div align="center">
  <a href="atlas-topology.png"><img src="atlas-topology.png" width="100%"></a>
</div>

| # | Link | What happens |
| - | ---- | ------------ |
| ① | **WAZUH-01 → Collector** | Every alert and archived event flows into Atlas through a read-only profile |
| ② | **Hyper-V host → LAB-OPS** | Atlas sees VM state and checkpoints over JEA, read-only |
| ③ | **Mac Studio = Atlas host** | Atlas and its AI models run on the Mac in VLAN 40; the UI is on 127.0.0.1 only |
| ④ | **Exercises ↔ Red Team** | Atlas plans the exercise window; I run Atomic Red Team; the alerts come back through ① |

## The Atlas Platform

<div align="center">
  <a href="atlas-platform.png"><img src="atlas-platform.png" width="100%"></a>
</div>

## Components

| Component | What it does |
| --- | --- |
| Collector | Reads alerts and events from Wazuh, read-only |
| PostgreSQL catalog | Keeps every original event with its SHA-256, plus decisions, cases and audit |
| Atlas runtime | The API and the web app on 127.0.0.1: triage card, cases, exercises, learning |
| Local AI | Qwen models on MLX, no network access |
| Keychain broker | The only part that can read secrets from the macOS Keychain |
| LAB-OPS | Reads Hyper-V health over JEA, read-only |

## Trust boundaries

| Boundary | What crosses it | What never crosses it |
| --- | --- | --- |
| Lab → Mac | Wazuh alerts, pulled read-only | Any write to Wazuh or the lab |
| Mac → Internet | One public IP, domain or hash, on my click | Logs, private addresses, internal names |
| Atlas → AI model | The evidence Atlas picked for this question | Database access, commands, URLs |
| AI model → Atlas | A JSON answer that must cite the evidence | Any write to decisions or audit |
| Atlas → Keychain | Only through the signed broker | Secrets in logs, prompts or the browser |

## The life of one alert

```text
Wazuh  →  Collector stores the original  →  I open the alert  →  triage card
       →  I ask a question  →  the AI answers with citations  →  shown only if every citation checks out
       →  I decide  →  the decision goes into the audit log
```

## Design rules

* **PostgreSQL is the only source of truth.** Everything else can be rebuilt from it
* **History, not edits.** Decisions and steps are added, never changed in place
* **Named failures.** Every limit has a name; hitting one is shown, never hidden
* **Small releases.** Most installs ship without a database migration
