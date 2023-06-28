## Building a SOC + Honeynet in Azure (Live Traffic)
![infographics](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/e6d83660-73ad-487f-8356-74e012b705a5)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. 

<b>The metrics we will show are:</b>
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

<b>The architecture of the honeynet consists of:</b>
- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

<b>The technology used:</b>
-	Windows Remote Desktop (for remoting into the VMs)
-	SQL Server Installation on the VM 
- Command Line Interface (for for SQL audit log setup)
-	Visual Studio Code (for simulated attacks)
-	PowerShell (for simulated attacks)


## Architecture Before Hardening / Security Controls
![before_infographics](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/9b88b108-b3b6-4d84-ac24-2273d217b51c)

For the "BEFORE" metrics, all resources were initially deployed and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet. This setup did not use Private Endpoints.

## Simulated Attacks
I simulated attacks using Visual Code and PowerShell scripts. The results were observed in log querying in Log Analytics Workspace and investigating incidents in Sentinel.
-	Linux Brute Force Attempt
-	AAD Brute Force Success
-	Windows Brute Force Success
-	Malware Detection (EICAR Test File)
-	Privilege Escalation


## Architecture After Hardening / Security Controls
![after_infographics](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/7c0f5e44-8330-4a8f-959d-b9aa57288e10)

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint
-	Network Security Groups (NSGs): I hardened the NSGs by blocking all inbound and outbound traffic, with the sole exception of my own public IP address. This ensured that only authorized traffic from a trusted source was allowed to access the virtual machines.
-	Built-in Firewalls: I configured the built-in firewalls on the virtual machines to restrict access and protect the resources from unauthorized connections. This step involved fine-tuning the firewall rules based on the specific requirements of each VM, thereby minimizing the potential attack surface.
-	Private Endpoints: To enhance the security of other Azure resources, I replaced the public endpoints with Private Endpoints. This ensured that access to sensitive resources, such as storage accounts and databases, was limited to the virtual network and not exposed to the public internet. As a result, these resources were protected from unauthorized access and potential attacks.

## Further Hardending Steps
The initial 24-hour study revealed that the lab was vulnerable to multiple threats due to its visibility on the public internet. To address these findings, I activated NIST SP 800-53 r4 within the compliance section of Microsoft Defender and focused on fulfilling the compliance standards associated with SC.7.*. Additional assessments for SC-7 - Boundary Protection.
<b>Before</b>
![compliance_before](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/83fd9d84-4cdd-4131-8420-026ba9d1f485)
<b>After</b>
![compliance_after](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/e6b0b94b-3dad-4bbe-9bf5-5985500a503c)

## Attack Maps Before Hardening / Security Controls
![mssql-auth-fail](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/e9a0b8dd-65aa-49e2-bd94-b57b976a71a1)<br><br>
![nsg-malicious-allowed-in](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/1e11f511-af05-44dd-bd8c-beeef61528fd)<br><br>
![syslog-ssh-auth-fail](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/bcdea77a-bb67-4d5e-b43a-e881676c027d)<br><br>
![windows-rdp-smb-auth-fail](https://github.com/spencermoy/azure-soc-honeynet/assets/137566643/aaba341d-6360-4da2-8695-224ea1d43755)<br><br>

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

## Attack Maps After Hardening / Security Controls

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

In this project, a mini honeynet was built in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was utilized to trigger alerts and create incidents based on the ingested logs. Security metrics were measured in the insecure environment before security controls were applied and then again after implementing security measures. The results showed that the number of security events and incidents were significantly reduced after the security controls were applied, demonstrating their effectiveness. 

