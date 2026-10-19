# Wazuh setup and service checks

Commands recorded from the lab. Review paths and addresses before reusing them.

## Kali Linux — check manager service

```bash
sudo systemctl status wazuh-manager
```

## Kali Linux — inspect local rules

```bash
sudo cat /var/ossec/etc/rules/local_rules.xml
```

## Kali Linux — inspect Active Response command definitions

```bash
sudo sed -n '185,245p' /var/ossec/etc/ossec.conf
```

## Kali Linux — check for custom-rule alerts

```bash
sudo grep '"id":"100100"' /var/ossec/logs/alerts/alerts.json | tail -5
```

## Kali Linux — check confirmed Windows failed-login alerts

```bash
sudo grep '"id":"60122"' /var/ossec/logs/alerts/alerts.json | tail -3
```

## Validate manager configuration after a rule/config edit

```bash
sudo /var/ossec/bin/wazuh-analysisd -t
```

If validation succeeds, it should not report configuration errors. Restart the manager only after validating a deliberate configuration change:

```bash
sudo systemctl restart wazuh-manager
sudo systemctl status wazuh-manager
```

## Kali Linux — test rule logic interactively (wazuh-logtest)

You can verify rules and decoders without waiting for live events by running:

```bash
sudo /var/ossec/bin/wazuh-logtest
```

Paste a test log message into the prompt to see which decoder and rule IDs match. Press `Ctrl + C` to exit.
