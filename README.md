# Active-Directory-Splunk
# Active Directory + Splunk SIEM Lab

A hands-on cybersecurity lab built to explore Windows authentication, endpoint telemetry, SIEM log collection, and security investigation using Active Directory, Splunk Enterprise, and Sysmon.

## Overview

The objective of this project was to build an isolated virtual environment, simulate authentication activity, collect Windows security events, and investigate the resulting telemetry in Splunk.

The lab helped me understand how endpoint events move from a Windows machine to a SIEM and why generating an event is different from detecting suspicious activity.

## Lab Architecture

The lab was built using Oracle VirtualBox with the following virtual machines:

| Machine | Role |
|---|---|
| Windows Server 2022 | Active Directory Domain Controller |
| Windows 10 Pro | Domain-joined target machine |
| Ubuntu | Splunk Enterprise server |
| Kali Linux | Attacker machine |

The Windows machines used Sysmon and Splunk Universal Forwarder to collect and forward telemetry to Splunk Enterprise.

## Tools and Technologies

- **Virtualization:** Oracle VirtualBox
- **Directory Services:** Windows Server 2022, Active Directory, DNS
- **SIEM:** Splunk Enterprise
- **Endpoint Telemetry:** Sysmon, Splunk Universal Forwarder
- **Security Testing:** Kali Linux, Hydra, Crowbar, FreeRDP
- **Adversary Simulation:** Atomic Red Team
- **Framework:** MITRE ATT&CK

## Implementation

### 1. Active Directory Setup

- Configured an Active Directory domain.
- Created domain user accounts.
- Joined the Windows target machine to the domain.
- Configured DNS and verified lab connectivity.

### 2. Splunk and Endpoint Monitoring

- Deployed Splunk Enterprise on Ubuntu.
- Installed Splunk Universal Forwarder on both Windows machines.
- Installed Sysmon using Olaf Hartong's Sysmon configuration.
- Forwarded Windows security telemetry to Splunk for investigation.

### 3. RDP Authentication Testing

Performed controlled RDP authentication testing against the lab target using **Hydra and Crowbar** to generate failed login attempts.

Crowbar did not successfully establish an RDP session using the password included in the test password file. To verify successful authentication separately, I used **FreeRDP** to log in to the target machine and establish an interactive RDP session.

### 4. Windows Event Investigation

Investigated Windows Security events in Splunk to verify that authentication activity was being captured:

- **Event ID 4625 — Failed logon:** Observed during unsuccessful authentication attempts using Hydra and Crowbar.
- **Event ID 4624 — Successful logon:** Observed following a successful RDP login using FreeRDP.

This exercise helped demonstrate how failed and successful authentication attempts can be investigated through Windows security logs.

### 5. MITRE ATT&CK and Atomic Red Team

Tested the following techniques in the lab:

- **T1059.001 — PowerShell:** Observed PowerShell-related telemetry in Splunk.
- **T1136.001 — Create Account: Local Account:** Identified a visibility gap that requires further investigation.

**Installation note:** If downloading Atomic Red Team through PowerShell does not work, an alternative is to download the repository ZIP file manually and extract it. Verify that the Atomic Red Team files are present in the extracted directory before running tests.

These exercises highlighted the importance of validating event generation, forwarding, indexing, and searchability before relying on telemetry for detection.

## Key Learnings

- Understanding Active Directory, domain joining, and DNS dependencies.
- Configuring endpoint telemetry collection and SIEM log forwarding.
- Investigating successful and failed Windows authentication events.
- Using Atomic Red Team to explore MITRE ATT&CK techniques.
- Troubleshooting virtual machine resource constraints and configuration issues.
- Understanding the difference between executing a test and validating a detection pipeline.

## Challenges

The project involved several practical challenges:

- Limited disk space and slow performance while running multiple virtual machines and ISO images. Ideally first while setting up allot it 4gb RAM then while testing reduce it to 2gb RAM to avoid 100% cpu utilisation.
- While installing windows 10 install the Windows 10 Pro version or else joining the Active Directory Domain is not possible.
- Troubleshooting RDP connectivity and authentication testing - Try to ping Target Machine through Kali - use hydra and crowbar if it fails use freerdp.
- Investigating why expected account-creation telemetry was not visible in Splunk.
- Make sure all the machines are in sync when it comes to the time zones so that events are generated as expected. It may take time for events to pop-up.

Resolving these issues reinforced the importance of understanding the underlying systems rather than simply following setup instructions.

## Screenshots

Screenshots documenting the lab configuration, authentication testing, and Splunk investigations are available in the [`screenshots`](./screenshots) directory.

## Acknowledgements

Thanks to **Steven Mah and the myDFIR tutorials** for guidance during the lab setup, and to the wider cybersecurity community for the tools and documentation used throughout the project.

This project also uses Sysmon and Olaf Hartong's Sysmon configuration. Refer to the original projects for their respective licences and attribution requirements.

## Disclaimer

This project was created for educational purposes in an isolated virtual lab. Authentication testing and adversary simulation were performed against lab systems under my control.
