# Test plan and observed results

## 1. Windows failed-login detection

1. Generate a failed login only on the test Windows machine and account you are authorized to test.
2. Open Wazuh Threat Hunting.
3. Set an appropriate recent time range and search for `60122` or the Windows Security Event ID `4625`.
4. Verify the event belongs to the Windows agent `LOQ`.

**Observed:** Wazuh built-in rule `60122`, “Logon Failure - Unknown user or bad password,” was present in `alerts.json` and visible in the dashboard under Event ID 4625 for agent `LOQ`.
- Evidence: `screenshots/wazuh-failed-logon-alert.png` (4 hits on Oct 19, 2026) and `screenshots/wazuh-repeated-logon-failures.png` (14 hits).

## 2. File Integrity Monitoring

1. Confirm the Windows agent is running.
2. Create and then modify a file under `C:\SOC-FIM-Test`.
3. In Threat Hunting, search `syscheck`.
4. Confirm alerts reference the test file and correct time.

**Observed:** rule `554` (“File added to the system”) and rule `550` (“Integrity checksum changed”) appeared in the dashboard for agent `LOQ`.
- Evidence: `screenshots/wazuh-fim-alert.png`.

## 3. Custom repeated-failed-login rule (Pending Live Verification)

- **Root cause identified:** The initial lab rule was configured with `<if_matched_sid>60105</if_matched_sid>`. In Wazuh default rulesets (`0580-win-security_rules.xml`), rule `60105` is an informational grouping rule with `level="0"`. Because level 0 rules do not produce alerts, and `<if_matched_sid>` only tracks rules that actually triggered alerts, rule `100100` never fired.
- **Fix:** In `rules/local_rules.xml`, `<if_matched_sid>` has been corrected to target `60122` (the level 5 rule that triggers on Event ID 4625).
- **Verification steps needed in lab:**
  1. Update `/var/ossec/etc/rules/local_rules.xml` on the Kali manager.
  2. Validate syntax: `sudo /var/ossec/bin/wazuh-analysisd -t`.
  3. Restart manager: `sudo systemctl restart wazuh-manager`.
  4. Generate 4 failed logons for the same user on `LOQ` within 60 seconds (since `frequency="3"` triggers on the 4th event).
  5. Check `sudo grep '"id":"100100"' /var/ossec/logs/alerts/alerts.json` and the Threat Hunting dashboard.
- **Current status:** **Not yet verified on live server.** Do not claim brute-force detection is complete until test evidence is captured.

## 4. Automated incident response

Not yet implemented or verified. Once added, document:
- The alert rule that triggers the response
- The exact action and execution location
- The response log or other evidence
- The safe test procedure and outcome
