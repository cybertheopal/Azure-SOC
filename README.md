# Building a SOC and Honeynet in Azure (Live Traffic Monitoring)

![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Overview

In this project, I set up a small honeynet in Microsoft Azure to simulate real-world attacks. Logs from various resources were ingested into a Log Analytics Workspace and monitored using Microsoft Sentinel.

The environment was left unsecured for 24 hours to observe attack activity. After collecting baseline metrics, I applied a series of security controls and monitored the environment for another 24 hours to compare results.

### Metrics Tracked:

-SecurityEvent (Windows event logs)
-Syslog (Linux event logs)
-SecurityAlert (Alerts triggered in Sentinel)
-SecurityIncident (Incidents created in Sentinel)
* **AzureNetworkAnalytics\_CL* (Malicious inbound traffic allowed by NSGs)

## Architecture

### Before Applying Security Controls

![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

### After Applying Security Controls

![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The Azure honeynet included:

* A virtual network (VNet)
* Network security groups (NSGs)
* Three virtual machines (2 Windows, 1 Linux)
* Log Analytics Workspace
* Azure Key Vault
* Azure Storage Account
* Microsoft Sentinel

In the initial setup, the environment was intentionally left open to the internet. All VMs had minimal firewall and NSG restrictions. Resources were publicly accessible, with no private endpoints configured.

After the first data collection phase, the environment was secured. NSGs were tightened to only allow traffic from a specific management workstation, and all public access was removed using private endpoints and firewall rules.

## Pre-Hardening Attack Visualization

![image](https://github.com/user-attachments/assets/d6e3a6d7-9418-40c1-ba94-69e1c9be6296)<br>
![image](https://github.com/user-attachments/assets/8fedae24-a7c0-4752-9fc4-98b89c8219fa)<br>
![image](https://github.com/user-attachments/assets/993d00f2-ef9a-4bbe-b2c6-1e1fe8d12f12)<br>
![image](https://github.com/user-attachments/assets/fad25459-3d37-4e35-99bc-156b03ae25bf)

## Sentinel Automation with KQL

Microsoft Sentinel was used to create alert rules and automate incident response using KQL queries.

![image](https://github.com/user-attachments/assets/175448d7-b5f3-4fce-9af9-b0f5a5b8cc6f)
![image](https://github.com/user-attachments/assets/184b44f4-2786-4a92-b93d-b8504e39798d)

Incident handling followed the NIST 800-61 Incident Response Lifecycle, improving detection and response efficiency.

![image](https://github.com/user-attachments/assets/472fbaa7-d396-4f61-a607-0909b523ad2b)

## Metrics: Before Hardening

**Timeframe:**
Start: 2024-12-18 13:54
End: 2024-12-19 13:54

| Metric                        | Count  |
| ----------------------------- | ------ |
| SecurityEvent                 | 47,884 |
| Syslog                        | 2,399  |
| SecurityAlert                 | 4      |
| SecurityIncident              | 288    |
| Malicious Flows Allowed (NSG) | 1,808  |

## Improving Security with Microsoft Defender for Cloud

Microsoft Defender for Cloud was enabled to enhance security posture. Regulatory compliance settings were activated, and public access was blocked using private endpoints, NSGs, and firewalls.

![image](https://github.com/user-attachments/assets/9ba6e854-f9cf-4dc5-8514-66a1dd0cbf85)
![image](https://github.com/user-attachments/assets/fda85ef3-a078-4960-9b38-4ed753aaa0f4)

## Metrics: After Hardening

**Timeframe:**
Start: 2024-12-19 23:02
End: 2024-12-20 23:02

| Metric                        | Count  |
| ----------------------------- | ------ |
| SecurityEvent                 | 16,534 |
| Syslog                        | 6      |
| SecurityAlert                 | 0      |
| SecurityIncident              | 0      |
| Malicious Flows Allowed (NSG) | 0      |

![image](https://github.com/user-attachments/assets/e940e227-2039-4286-8920-63122bd0c3a9)

## Final Thoughts

This project demonstrated how exposing an environment to the internet attracts malicious activity, and how applying the right security controls can drastically reduce it. Microsoft Sentinel and Defender for Cloud provided clear visibility into threats and helped automate response and containment.

---
