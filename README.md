# Built a Security Onion Home SOC Lab with a Monitored Windows Server 2022 Domain Controller

## Objective

The goal of this project was to build a working blue-team home lab that could support future adversary emulation, SIEM analysis, endpoint monitoring, and Active Directory attack simulations.

To achieve this, I deployed a standalone Security Onion instance in VMware Workstation Player, enrolled a Windows Server 2022 endpoint into Elastic Fleet, promoted that server into an Active Directory domain controller, and seeded the domain with intentionally vulnerable content for later attack simulation from Kali Linux.

This project focused on the infrastructure and monitoring foundation required before moving into actual attack, detection, and investigation workflows.

---

## Lab Architecture

The lab was built in VMware Workstation Player 17 and currently consists of:

- Security Onion standalone node acting as the SOC / SIEM platform
- Windows Server 2022 endpoint enrolled into Elastic Fleet
- Windows Server 2022 promoted to an Active Directory Domain Controller
- Vulnerable Active Directory content created for later attack simulation
- Kali Linux planned as the future attacker VM for adversary emulation

### Core Lab Details

- Hypervisor: VMware Workstation Player 17
- Security Onion IP: 192.168.9.132
- Windows Server hostname: WIN-HS48GJMNOGP
- AD domain: cs.org
- Security Onion node name: soc-lab

---

## Tools and Technologies Used

- Security Onion 2.4.211
- Elastic Fleet / Elastic Agent
- Windows Server 2022
- Active Directory Domain Services (AD DS)
- DNS
- VMware Workstation Player 17
- PowerShell
- Local Group Policy Editor
- GitHub-hosted PowerShell scripts from the book lab resources

---

## Implementation Summary

### 1. Deployed Security Onion as a standalone SOC platform

I installed Security Onion in VMware and validated the node state through the Grid interface. This provided the central monitoring platform for the lab, including the SIEM components, containerized services, and management interface.

### 2. Enrolled Windows Server 2022 into Elastic Fleet

I downloaded the customized Windows Elastic Agent installer from Security Onion, configured the required firewall hostgroup allowlists, and enrolled the Windows Server into Fleet. After resolving an initial failed install, the endpoint successfully checked in and appeared as Healthy in Elastic Fleet.

### 3. Verified endpoint monitoring and policy assignment

After successful enrollment, I confirmed that the Windows Server endpoint was active in Fleet and associated with the expected endpoint policy. This established host-level telemetry collection, not just network-level visibility.

### 4. Promoted Windows Server to an Active Directory Domain Controller

Using PowerShell and the chapter lab script, I promoted the server into an AD Domain Controller for the lab domain:

- Domain: cs.org
- NetBIOS name: CS

After reboot, Server Manager confirmed that AD DS and DNS were active, indicating successful domain controller promotion.

### 5. Seeded vulnerable Active Directory content

I then ran the vulnerable AD setup script to create intentionally insecure lab conditions for future red-vs-blue exercises. The script created and configured vulnerabilities including:

- Kerberoasting exposure
- AS-REP roasting exposure
- Passwords stored in descriptions
- Shared passwords suitable for password spraying
- DCSync-related privilege assignments
- SMB signing disabled
- Nested group issues
- Weak/default credential patterns

This transformed the domain controller from a plain server into an attackable enterprise-style AD target.

### 6. Disabled automatic updates to preserve lab state

To prevent the server from patching itself, auto-restarting, or drifting away from the intentionally vulnerable configuration, I disabled automatic Windows updates through Local Group Policy Editor.

Policy path used:

`Computer Configuration > Administrative Templates > Windows Components > Windows Update > Configure Automatic Updates`

Setting applied:

- Configure Automatic Updates = Disabled

This preserved the repeatability and vulnerability profile of the lab.

---

## Key Troubleshooting and Fixes

This project involved real troubleshooting rather than simple click-through setup.

### Elastic Agent installation failure

The first Elastic Agent installation attempt failed. I investigated the log output and found that the Windows service had been created, but the expected executable path was missing. This indicated a broken local installation rather than a network communication issue.

### Firewall hostgroup configuration

Before the endpoint could enroll successfully, I had to add the Windows Server IP to the required Security Onion firewall hostgroups used by the agent installer and Fleet communication path:

- elastic_agent_endpoint
- fleet
- manager

This allowed the Windows endpoint to communicate properly with the Security Onion services.

### Recovery and clean reinstall

After identifying the incomplete Elastic Agent installation, I removed the broken state, restarted the process, and reran the installer cleanly. The endpoint then appeared healthy in Fleet.

### PowerShell script correction during AD promotion

During the domain controller promotion phase, the original copied command required adjustment because one of the switch parameters did not paste cleanly for direct execution. After correcting the command, the AD DS installation completed successfully and the server rebooted normally.

---

## Final Validated State

At the end of this project, the lab was in the following working state:

- Security Onion standalone node operational
- Fleet server operational
- Windows Server endpoint enrolled and healthy in Elastic Fleet
- Windows Server promoted to Domain Controller for cs.org
- Vulnerable Active Directory content successfully created
- Automatic Windows updates disabled to preserve the lab state

### Healthy Elastic Fleet agents observed

- WIN-HS48GJMNOGP
- FleetServer-soc-lab
- soc-lab

---

## Why This Matters for SOC Work

This project is directly relevant to SOC and blue-team skill development because it establishes the infrastructure required for realistic monitoring and investigation workflows.

It demonstrates hands-on experience with:

- SIEM platform deployment
- Endpoint enrollment and health validation
- Security Onion administration
- Elastic Fleet usage
- Windows Server monitoring
- Active Directory lab creation
- Vulnerable identity infrastructure preparation
- Troubleshooting broken agent installation and service issues
- Preserving repeatable lab conditions for future attack simulation

This lab now supports future work involving:

- Adversary emulation from Kali Linux
- Attack detection in Security Onion
- Host and network telemetry analysis
- Active Directory attack investigation
- Alert triage and hunting workflows

---

## Screenshots

### 1. Security Onion node health and container status

![Security Onion Grid](screenshots/01-security-onion-grid.png)

### 2. Elastic Fleet healthy agents

![Fleet Healthy Agents](screenshots/02-fleet-healthy-agents.png)

### 3. Security Onion dashboard overview

![Security Onion Dashboard](screenshots/03-security-onion-dashboard.png)

### 4. Domain Controller confirmation

![Domain Controller Confirmation](screenshots/04-domain-controller-server-manager.png)

### 5. Vulnerable AD content creation

![Vulnerable AD Script Output](screenshots/05-vulnerable-ad-script-output.png)

### 6. Lab preservation policy

![Disable Auto Updates GPO](screenshots/06-disable-auto-updates-gpo.png)

---

## Next Steps

The next phase of this lab will focus on adversary simulation and blue-team analysis by:

- Preparing or validating the Kali Linux attacker VM
- Performing reconnaissance and exploitation activity against the vulnerable domain controller
- Monitoring resulting telemetry in Security Onion
- Analyzing detections, logs, and endpoint activity
- Documenting findings as a separate attack-and-detection case study