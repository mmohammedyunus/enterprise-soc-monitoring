# Enterprise SOC Monitoring Using Wazuh

A student cybersecurity project demonstrating centralized security monitoring with Wazuh, a Windows endpoint agent, Windows failed-logon detection, and File Integrity Monitoring (FIM).

## Project status

- [x] Wazuh manager and dashboard running on Kali Linux
- [x] Windows endpoint agent (`LOQ`) connected to the manager
- [x] Windows failed-logon events (Event ID 4625) detected by Wazuh built-in rule `60122`
- [x] File Integrity Monitoring tested: file creation and integrity checksum-change alerts observed
- [ ] Custom repeated-failed-login rule `100100` verified
- [ ] Automated Active Response implemented and tested

> Automated incident response is not yet verified. Do not describe it as completed until a response action has been tested and its evidence captured.

## Architecture

Windows endpoint (`LOQ`) → Wazuh agent → Wazuh manager on Kali Linux → rule analysis and alerts → Wazuh dashboard.

## Environment

- Wazuh manager: 4.14.7 on Kali Linux
- Wazuh Windows agent: 4.14.5
- Windows endpoint name: `LOQ`
- Manager address used in the lab: `192.168.132.130`
- Dashboard: `https://192.168.132.130`

These are lab-network addresses and may differ on another machine.

## Demonstrated capabilities

### Failed-login monitoring

Windows Security Event ID 4625 was received and matched built-in Wazuh rule `60122` (`Logon Failure - Unknown user or bad password`).

See `screenshots/wazuh-failed-logon-alert.png` and `screenshots/wazuh-repeated-logon-failures.png`.

### File Integrity Monitoring

The Windows agent monitored `C:\SOC-FIM-Test`. The dashboard showed:
- Rule `554`: `File added to the system.`
- Rule `550`: `Integrity checksum changed.`

See `screenshots/wazuh-fim-alert.png`.

## Repository contents

- `commands/`: commands used for setup and validation
- `rules/local_rules.xml`: local custom rules from the lab
- `screenshots/`: evidence from the Wazuh dashboard
- `active-response/`: implementation notes and verification checklist
- `docs/`: additional project documentation

## Safety and reproducibility

- Run commands only on systems you own or are authorized to administer.
- Do not commit passwords, API keys, private certificates, or sensitive raw event logs.
- The manager IP and Windows paths in these notes are specific to this lab.
- The custom rule `100100` originally used `<if_matched_sid>60105</if_matched_sid>`. Because rule `60105` is a level 0 base rule that never generates alerts, the rule could not trigger. In `rules/local_rules.xml`, this was corrected to `<if_matched_sid>60122</if_matched_sid>`. It still requires live lab verification before claiming brute-force correlation works.
- Active Response is not enabled by this repository; use a controlled lab and verify any response action before enabling it.

## Next milestone

Verify the corrected custom repeated-failed-login rule in the lab, then configure and safely test one harmless Active Response action. Add screenshots and record the exact trigger, action, and result.
