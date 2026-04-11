# Personal Build Notes

## Elastic Agent Issue
Initial install failed. Service created but executable missing.

## Fix
Reinstalled agent and verified in Fleet.

## Firewall
Added Windows IP to:
- elastic_agent_endpoint
- fleet
- manager

## Lesson
Need to validate installs instead of assuming success.