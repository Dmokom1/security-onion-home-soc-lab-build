# Personal Build Notes

## Firewall hostgroup notes
Added Windows Server IP to:
- elastic_agent_endpoint
- fleet
- manager

## Broken Elastic Agent issue
Initial installer created service but executable path was missing. Suspected incomplete or broken local install.

## Recovery steps
- Removed broken install state
- Restarted installation
- Re-ran installer
- Verified host appeared healthy in Fleet

## AD promotion issue
One PowerShell switch parameter did not paste cleanly and needed correction before successful execution.

## General lesson learned
This lab required real troubleshooting and validation, not just following steps blindly.