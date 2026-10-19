# Active Response — implementation pending

Wazuh manager configuration contains built-in command definitions such as `firewall-drop`, `host-deny`, and Windows-related commands. However, the lab configuration shown so far has no specific `<active-response>` block enabled for a chosen alert.

> **Status:** Automated incident response is **NOT yet implemented or verified**. Do not claim it is working until an active response block is configured, tested in a controlled lab, and execution logs/screenshots are collected.

## Safe implementation checklist

- [ ] Choose a test alert rule already confirmed to fire (e.g., rule `60122` or rule `550`)
- [ ] Start with a harmless action (e.g., writing an audit entry or running a benign test script)
- [ ] Configure the `<command>` and `<active-response>` block in `/var/ossec/etc/ossec.conf` on the Wazuh manager
- [ ] Validate configuration before restarting: `sudo /var/ossec/bin/wazuh-analysisd -t`
- [ ] Restart manager: `sudo systemctl restart wazuh-manager`
- [ ] Trigger the alert in the isolated lab environment
- [ ] Verify execution in the active response log files:
  - **Manager log:** `/var/ossec/logs/active-responses.log`
  - **Windows agent log:** `C:\Program Files (x86)\ossec-agent\active-responses.log`
- [ ] Capture screenshots and evidence of the response execution
- [ ] Document how to safely disable and revert the configuration

Do not enable aggressive remediation (such as IP blocking, firewall dropping, route modifications, or account lockout) without a controlled lab test and a verified recovery plan.
