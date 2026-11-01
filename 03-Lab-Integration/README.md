# Lab Integration

How Atlas reads my [SOC homelab](https://github.com/Evgeniy-Nazarov/soc-homelab-enterprise). Every link is read-only: **Atlas never changes the lab.**

## ① WAZUH-01 → Collector

```text
Lab sensors and agents
        ↓
WAZUH-01 (manager + indexer)
   wazuh-alerts-*    rule hits
   wazuh-archives-*  every event, including PowerShell 4104
        ↓  dedicated read-only profile
Atlas collector  →  PostgreSQL catalog (original + SHA-256)
```

* Bounded, resumable batches with an exact total for each batch
* A batch that hits a limit stops with a named reason and continues later
* **A gap is shown as a gap**, never as "nothing happened"
* Archive reads (script blocks, process chain, host network) happen only when I click, and are bounded

## Telemetry sources

| Source | What it gives Atlas |
| --- | --- |
| Sysmon (WIN11-USER-01/02, DC01) | Process starts (1), network connections (3), file creates (11), DNS queries (22) |
| Windows Security | Logons 4624 / 4625, process creation 4688 |
| PowerShell | Script block logging 4104 |
| DC01 (SOCLAB.LOCAL) | Kerberos, logons, group changes |
| Suricata (SPAN) | Network alerts as EVE JSON |
| FortiGate 60F | Firewall logs over syslog |
| AWS-HUB | Cloud host telemetry over IPsec |

## Freshness

Atlas checks that each source is still talking: the last event per host, and syslog hosts through the archive. A quiet host is shown as QUIET or NO_DATA instead of silently disappearing.

## ② Hyper-V host → LAB-OPS

* A JEA endpoint on the Hyper-V host that only allows read commands
* Only VMs on an approved list
* VM state and checkpoints, one session per cycle
* A kill switch in Atlas turns it off

## ③ Mac Studio = Atlas host

Atlas runs on the Mac Studio in VLAN 40 (MGMT_NET). The UI listens on 127.0.0.1 only.

## ④ Exercises ↔ Red Team

Atlas plans the exercise window; I run the Atomic Red Team test myself; the alerts come back through ① and are marked as exercise alerts. More in [08-Lab-Exercises](../08-Lab-Exercises/).
