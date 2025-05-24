#Cloud SOC & Honeynet in Azure (Live Attacks Tracked)

![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## What’s This All About?

This project walks you through how I built a small honeynet in Azure. I hooked it up to a Log Analytics Workspace and fed that data straight into Microsoft Sentinel to map attacks, fire off alerts, and open incidents.

I let the environment sit exposed for 24 hours, collected the chaos, then locked it down with some solid security controls. After that, I let it run another 24 hours and pulled the new numbers to see how things changed. Here’s what I tracked:

* **SecurityEvent** (Windows Event Logs)
* **Syslog** (Linux Event Logs)
* **SecurityAlert** (Triggered Alerts)
* **SecurityIncident** (Sentinel Incidents)
* **AzureNetworkAnalytics\_CL** (Malicious traffic that got through)

## Before Lockdown (Everything Wide Open)

![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## After Lockdown (Fort Knox Mode)

![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The honeynet setup looked like this:

* Virtual Network (VNet)
* Network Security Group (NSG)
* 2 Windows VMs + 1 Linux VM
* Log Analytics Workspace
* Azure Key Vault
* Azure Storage Account
* Microsoft Sentinel

Before I hardened it, everything was exposed to the internet. Firewalls and NSGs? Basically nonexistent. No private endpoints, just raw and open.

After locking it down, I shut the doors. NSGs were tightened to only allow traffic from my admin box. Other resources got firewalled and hidden behind private endpoints.

## What the Attackers Were Up To (Pre-Hardening)

![image](https://github.com/user-attachments/assets/d6e3a6d7-9418-40c1-ba94-69e1c9be6296)<br>
![image](https://github.com/user-attachments/assets/8fedae24-a7c0-4752-9fc4-98b89c8219fa)<br>
![image](https://github.com/user-attachments/assets/993d00f2-ef9a-4bbe-b2c6-1e1fe8d12f12)<br>
![image](https://github.com/user-attachments/assets/fad25459-3d37-4e35-99bc-156b03ae25bf)

## Microsoft Sentinel in Action (KQL on Deck)

I automated alerts and incidents using Sentinel’s KQL rules, making sure the SOC was loud and reactive when stuff went down.

![image](https://github.com/user-attachments/assets/175448d7-b5f3-4fce-9af9-b0f5a5b8cc6f)
![image](https://github.com/user-attachments/assets/184b44f4-2786-4a92-b93d-b8504e39798d)

I followed the NIST 800-61 incident response playbook to manage incidents and sharpen threat visibility across the whole setup.

![image](https://github.com/user-attachments/assets/472fbaa7-d396-4f61-a607-0909b523ad2b)

## Raw Stats Before Lockdown

**Timeframe:**
Start: 2024-06-18 13:54
End: 2024-06-19 13:54

| Metric                        | Count  |
| ----------------------------- | ------ |
| SecurityEvent                 | 47,884 |
| Syslog                        | 2,399  |
| SecurityAlert                 | 4      |
| SecurityIncident              | 288    |
| Malicious Flows Allowed (NSG) | 1,808  |

## Locking It Down with Defender for Cloud

Enabled Microsoft Defender for Cloud, added NIST compliance, slapped private endpoints and NSG rules on everything. No more public exposure.

![image](https://github.com/user-attachments/assets/9ba6e854-f9cf-4dc5-8514-66a1dd0cbf85)
![image](https://github.com/user-attachments/assets/fda85ef3-a078-4960-9b38-4ed753aaa0f4)

## Raw Stats After Lockdown

**Timeframe:**
Start: 2024-06-19 23:02
End: 2024-06-20 23:02

| Metric                        | Count  |
| ----------------------------- | ------ |
| SecurityEvent                 | 16,534 |
| Syslog                        | 6      |
| SecurityAlert                 | 0      |
| SecurityIncident              | 0      |
| Malicious Flows Allowed (NSG) | 0      |

![image](https://github.com/user-attachments/assets/e940e227-2039-4286-8920-63122bd0c3a9)

## Wrap Up

This whole project was about turning an open, vulnerable environment into a hardened one—and watching how that flipped the script. Sentinel helped catch the noise, and once I locked things down, the noise dropped off a cliff.

If this setup was powering production workloads with users hitting it daily, we’d expect more signals post-hardening. But even in this test run, the results speak for themselves.

---

Let me know if you want a version that tones it down or adds anything else.
