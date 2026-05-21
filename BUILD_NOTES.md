# Build Notes
# Security Onion Home SOC Lab with Windows Server 2022 Domain Controller

This file provides supporting build context for the main README. It documents the lab foundation, validation points, evidence interpretation, limitations, and screenshot mapping.

The README explains the full project story. These notes focus on what was built, what was validated, and what the screenshots support.

---

## Purpose of This File

This project was built as the foundation for later SOC, detection, and Active Directory security projects.

These build notes focus on:

- Security Onion node validation
- Elastic Fleet agent health
- Security Onion dashboard visibility
- Windows Server 2022 lab setup
- Vulnerable Active Directory lab preparation
- Lab preservation settings
- Screenshot-supported evidence

---

## Lab Environment

| Component | Details |
|---|---|
| Hypervisor | VMware Workstation Player 17 |
| SIEM / SOC Platform | Security Onion |
| Security Onion Version | 2.4.211 |
| Security Onion Node | `soc-lab` |
| Security Onion IP Shown | `192.168.9.132` |
| Windows Server | Windows Server 2022 |
| Windows Hostname | `WIN-HS48GJMNOGP` |
| Active Directory Domain | `cs.org` |
| Fleet / Endpoint Monitoring | Elastic Fleet / Elastic Agent |
| Lab Purpose | Foundation for later AD, SIEM, and detection projects |

---

## Corrected Lab Sequence

The final lab workflow followed this order:

1. Reviewed Security Onion Grid status.
2. Confirmed the Security Onion standalone node was visible.
3. Confirmed key Security Onion containers were running.
4. Confirmed Elastic Fleet showed healthy agents.
5. Reviewed Security Onion dashboard telemetry and datasets.
6. Verified Windows Server was active through Server Manager.
7. Ran vulnerable AD lab setup content.
8. Disabled automatic Windows updates through Local Group Policy.
9. Confirmed the lab was ready to support later detection and investigation work.

---

## Important Values and Observations

| Item | Value |
|---|---|
| Security Onion node | `soc-lab` |
| Security Onion IP shown | `192.168.9.132` |
| Security Onion role | Standalone |
| Security Onion version | `2.4.211` |
| Windows endpoint hostname | `WIN-HS48GJMNOGP` |
| Healthy Fleet agents shown | `WIN-HS48GJMNOGP`, `FleetServer-soc-lab`, `soc-lab` |
| Active Directory domain | `cs.org` |
| Dashboard datasets shown | `system.syslog`, `soc.server`, `zeek.dns`, `zeek.conn`, `elastic_agent`, `suricata`, `winlog` |
| Lab preservation setting | Automatic Windows updates disabled |

---

## Phase 1: Security Onion Grid Review

Security Onion Grid was reviewed to confirm the node and container status.

### Screenshot Evidence

| Screenshot | What It Supports |
|---|---|
| `screenshots/01-security-onion-grid.png` | Security Onion node status and running containers |

### Observation

The Grid view showed:

- Node: `soc-lab`
- Address: `192.168.9.132`
- Role: `Standalone`
- Version: `2.4.211`

Several Security Onion containers were shown as running.

This confirmed that the Security Onion node was operational enough to support later lab work.

The screenshot also showed high memory usage and an OS update notice. This is useful context because resource pressure and maintenance status can affect home lab stability.

---

## Phase 2: Elastic Fleet Agent Health

Elastic Fleet was reviewed to confirm that agents were enrolled and healthy.

### Screenshot Evidence

| Screenshot | What It Supports |
|---|---|
| `screenshots/02-fleet-healthy-agents.png` | Healthy Fleet agents, including Windows Server and Security Onion-related agents |

### Observation

Fleet showed three healthy agents:

- `WIN-HS48GJMNOGP`
- `FleetServer-soc-lab`
- `soc-lab`

This confirmed that the Windows endpoint and Security Onion-related agents were checking in successfully.

This supports endpoint enrollment and basic agent health. It does not prove every telemetry type was fully validated.

---

## Phase 3: Security Onion Dashboard Review

The Security Onion dashboard was reviewed to confirm log visibility.

### Screenshot Evidence

| Screenshot | What It Supports |
|---|---|
| `screenshots/03-security-onion-dashboard.png` | Security Onion dashboard showing log volume and datasets |

### Observation

The dashboard showed more than one million documents in the selected time window.

Visible datasets and modules included:

- `system.syslog`
- `soc.server`
- `zeek.dns`
- `zeek.conn`
- `elastic_agent`
- `suricata`
- `winlog`

This confirmed that the lab had active log visibility across several data sources.

This screenshot supports general telemetry visibility, not complete monitoring coverage.

---

## Phase 4: Windows Server 2022 Validation

Windows Server Manager was reviewed on the Windows Server system.

### Screenshot Evidence

| Screenshot | What It Supports |
|---|---|
| `screenshots/04-domain-controller-server-manager.png` | Windows Server Manager dashboard |

### Observation

The screenshot showed Server Manager open with one managed server.

This supports that the Windows Server system was active and available for lab configuration.

The screenshot does not prove every Active Directory configuration detail by itself.

---

## Phase 5: Vulnerable Active Directory Lab Content

A vulnerable AD lab setup script was executed to prepare the domain for later security testing.

### Screenshot Evidence

| Screenshot | What It Supports |
|---|---|
| `screenshots/05-vulnerable-ad-script-output.png` | Vulnerable AD lab setup script output |

### Observation

The script output showed intentionally vulnerable lab content being created or configured.

Visible items included:

- Managed service accounts
- Kerberoasting setup
- AS-REP roasting setup
- Passwords in descriptions
- Default password patterns
- Password spraying setup
- DCSync-related setup
- SMB signing disabled

This prepared the environment for later Active Directory attack and detection exercises.

These settings were intentionally created in an isolated lab and should not be used in production.

---

## Phase 6: Lab Preservation Policy

Automatic Windows updates were disabled through Local Group Policy.

### Screenshot Evidence

| Screenshot | What It Supports |
|---|---|
| `screenshots/06-disable-auto-updates-gpo.png` | Group Policy view showing automatic updates disabled |

### Observation

The Local Group Policy Editor showed Windows Update settings with automatic updates disabled.

This was done to reduce unexpected reboots, patching, or lab drift during future testing.

This is reasonable for a controlled lab, but it is not a production recommendation.

---

## Evidence Interpretation Notes

These notes keep the project explanation accurate:

- This is a home SOC foundation project, not an enterprise SOC deployment.
- The Security Onion Grid screenshot supports node and container visibility.
- The Fleet screenshot supports agent health and enrollment status.
- The dashboard screenshot supports active log visibility, not complete detection coverage.
- The Server Manager screenshot supports Windows Server availability, not every AD configuration detail.
- The vulnerable AD script output supports intentional lab weakness creation.
- The Group Policy screenshot supports disabling automatic updates for lab repeatability.
- Troubleshooting details should not be overstated unless separately documented with logs or screenshots.

---

## Key Lessons Learned

1. Lab infrastructure must be validated before attack simulation begins.
2. Fleet agent health is a key checkpoint before relying on endpoint telemetry.
3. Security Onion dashboards help confirm that logs are arriving and searchable.
4. Vulnerable AD lab content should be clearly labeled as intentional and isolated.
5. Disabling updates can help preserve a repeatable lab state, but only in a controlled lab.
6. Home lab documentation should explain the foundation without pretending it is a production SOC.
7. Screenshots matter because they keep the documentation tied to what was actually built.

---

## Improvements for a Future Version

Future improvements could include:

- Adding a network diagram showing Security Onion, Windows Server, and Kali placement.
- Capturing clearer Active Directory Domain Services confirmation.
- Documenting the Elastic Agent enrollment command.
- Capturing any Elastic Agent troubleshooting logs if installation problems occur.
- Adding screenshots of Windows Event Logs arriving in Elastic.
- Adding validation queries for endpoint, Suricata, Zeek, and Windows logs.
- Documenting Security Onion firewall hostgroup changes if used.
- Recording CPU, memory, and storage allocation decisions.
- Adding a short baseline checklist for confirming the lab is ready before future projects.

---

## Screenshot Map

| Screenshot | What It Supports |
|---|---|
| `screenshots/01-security-onion-grid.png` | Security Onion node status and running containers |
| `screenshots/02-fleet-healthy-agents.png` | Healthy Fleet agents, including Windows Server and Security Onion-related agents |
| `screenshots/03-security-onion-dashboard.png` | Security Onion dashboard showing log volume and datasets |
| `screenshots/04-domain-controller-server-manager.png` | Windows Server Manager dashboard |
| `screenshots/05-vulnerable-ad-script-output.png` | Vulnerable AD lab setup script output |
| `screenshots/06-disable-auto-updates-gpo.png` | Group Policy view showing automatic updates disabled |