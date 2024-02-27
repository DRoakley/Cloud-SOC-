# Building a SOC + Honeynet in Azure (Live Traffic)

![image](https://github.com/DRoakley/Cloud-SOC-/assets/161369916/8ba4de83-6ec7-4cf0-9230-e055801452b1)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)
  

## Architecture Before Hardening / Security Controls
![image](https://github.com/DRoakley/Cloud-SOC-/assets/161369916/9a02dd68-f9d9-48dc-82b6-0beecc96f675).




## Architecture After Hardening / Security Controls
(![image](https://github.com/DRoakley/Cloud-SOC-/assets/161369916/8a73f0b3-2518-4eec-a308-23b7eb21592b)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/DRoakley/Cloud-SOC-/assets/161369916/0954b9cc-041b-4728-8c4a-5cc1a74d2c71)<br>




## Architecture After Hardening / Security Controls
![Linux Syslog Auth Failures](https://github.com/DRoakley/Cloud-SOC-/assets/161369916/c8c23a53-e087-41f5-b38a-76385f6569d2)<br>



## Metrics After Hardening / Security Controls
![Windows RDP/SMB Auth Failures](https://github.com/DRoakley/Cloud-SOC-/assets/161369916/5d937215-ce55-4c36-842d-1b493c41c113)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-02-22 3:20:58
Stop Time 2024-02-23T 3:20:58

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 45672
| Syslog                   | 8511
| SecurityAlert            | 10
| SecurityIncident         | 270
| AzureNetworkAnalytics_CL | 2565

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-02-23 3:39:38
Stop Time	2024-02-24 3:39:38

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9829
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In summary, this project successfully established a mini honeynet within Microsoft Azure, integrating various log sources into a Log Analytics workspace and utilizing Microsoft Sentinel to detect and respond to security threats. By measuring metrics before and after implementing security measures, a significant decrease in security events and incidents was observed, highlighting the effectiveness of the security controls deployed. It's important to acknowledge that under heavier resource utilization by regular users, more security events and alerts might have been generated post-implementation, indicating the ongoing need for vigilance and adaptation in cybersecurity strategies.

It's important to consider that in case the network resources were extensively used by regular users, there could have been a higher probability of generating additional security events and alerts within the 24-hour period after implementing the security controls.
