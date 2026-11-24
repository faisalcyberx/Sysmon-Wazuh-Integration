# Troubleshooting Guide

| Issue | Possible cause | Recommended check |
|---|---|---|
| Sysmon service is not running | Installation issue | Check the Sysmon service and installation |
| Sysmon events are not visible in Event Viewer | Sysmon installation or configuration issue | Check Sysmon service and active configuration |
| Wazuh Agent is not running | Agent service issue | Restart the Wazuh Agent and inspect its log |
| Sysmon events are missing from Wazuh | Sysmon channel not configured in `ossec.conf` | Verify the `<localfile>` block |
| Events are visible locally but not in Wazuh | Agent collection or connectivity issue | Check Agent service, logs, and Manager connectivity |
| Custom alert is not generated | Incorrect parent rule or custom rule configuration | Verify parent Rule ID `61613` and custom Rule ID `100120` |
| Wazuh Manager fails to restart | XML syntax error or duplicate Rule ID | Validate XML and inspect Manager logs |
| Custom description is not visible | Dashboard shows a different rule or older event | Filter by `rule.id:100120` and select a recent time range |
| No matching Dashboard events | Incorrect query or time filter | Check the DQL query and time range |

## Windows commands

Check Sysmon services:

```powershell
Get-Service *Sysmon*
```

Check Wazuh Agent:

```powershell
Get-Service -Name Wazuh
```

Check active Sysmon configuration:

```powershell
C:\Windows\Sysmon64.exe -c
```

Wazuh Agent log:

```text
C:\Program Files (x86)\ossec-agent\ossec.log
```

## Wazuh Manager commands

Search custom alerts:

```bash
sudo grep '"id":"100120"' /var/ossec/logs/alerts/alerts.json | tail -n 5
```

Validate local rules XML:

```bash
sudo xmllint --noout /var/ossec/etc/rules/local_rules.xml
```

Check Manager status:

```bash
sudo systemctl is-active wazuh-manager
```

Inspect Manager log:

```bash
sudo tail -n 50 /var/ossec/logs/ossec.log
```

## Important notes

- Rule ID `61613` is a built-in Wazuh parent rule in this lab. Do not duplicate it in `local_rules.xml`.
- Custom Rule ID `100120` is intended for Sysmon Event ID 11 through parent rule `61613`.
- A Dashboard alert view and raw telemetry/archive visibility are not the same thing.
- Do not classify the controlled file-creation test as malicious activity.
