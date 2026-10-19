# Architecture

```text
Windows endpoint (LOQ)
        |
        | Wazuh agent / security events and FIM
        v
Wazuh manager (Kali Linux)
        |
        | Decoders and rules
        v
Security alerts
        |
        v
Wazuh dashboard / Threat Hunting
```

The current lab demonstrates Windows failed-logon alerting and real-time File Integrity Monitoring. Automated incident response is a planned next milestone, not a verified capability yet.
