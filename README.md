![image](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/fe81a033-9ebe-4417-9f4e-633798e65448)# Building a SOC + Honeynet in Azure (Live Traffic)
![infographics](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/e6d83660-73ad-487f-8356-74e012b705a5)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![before_infographics](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/9b88b108-b3b6-4d84-ac24-2273d217b51c)

## Architecture After Hardening / Security Controls
![after_infographics](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/7c0f5e44-8330-4a8f-959d-b9aa57288e10)

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
![mssql-auth-fail](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/e9a0b8dd-65aa-49e2-bd94-b57b976a71a1)<br>
![nsg-malicious-allowed-in](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/1e11f511-af05-44dd-bd8c-beeef61528fd)<br>
![syslog-ssh-auth-fail](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/bcdea77a-bb67-4d5e-b43a-e881676c027d)<br>
![windows-rdp-smb-auth-fail](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/aaba341d-6360-4da2-8695-224ea1d43755)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:<br>
Start Time 2023-06-14 17:04:29<br>
Stop Time 2023-06-15 17:04:29<br>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 84500
| Syslog                   | 4560
| SecurityAlert            | 36
| SecurityIncident         | 283
| NSG Inbound Malicious Flows Allowed | 7456

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:<br>
Start Time 2023-06-16 15:37<br>
Stop Time	2023-06-17 15:37<br>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 11382
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| NSG Inbound Malicious Flows Allowed | 0

## Results

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | -86.53%
| Syslog                   | -99.45%
| SecurityAlert            | -100.00%
| SecurityIncident         | -100.00%
| NSG Inbound Malicious Flows Allowed | -100.00%

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
