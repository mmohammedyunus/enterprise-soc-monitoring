# Windows agent and FIM commands

Run PowerShell as Administrator on the Windows endpoint.

## Check agent service

```powershell
Get-Service -Name WazuhSvc
```

## Start the agent if it is stopped

```powershell
Start-Service -Name WazuhSvc
Get-Service -Name WazuhSvc
```

## Confirm the FIM path is in the agent configuration

```powershell
Select-String -Path "C:\Program Files (x86)\ossec-agent\ossec.conf" -Pattern "SOC-FIM-Test"
```

The lab configuration contains this line inside the `<syscheck>` section:

```xml
<directories realtime="yes">C:\SOC-FIM-Test</directories>
```

Whenever you modify `ossec.conf`, restart the Wazuh agent service so changes take effect:

```powershell
Restart-Service -Name WazuhSvc
```

## Create and modify a test file

```powershell
New-Item -ItemType Directory -Force -Path "C:\SOC-FIM-Test"
Set-Content -Path "C:\SOC-FIM-Test\fim-check.txt" -Value "Initial file created"
Start-Sleep -Seconds 5
Set-Content -Path "C:\SOC-FIM-Test\fim-check.txt" -Value "File modified for FIM detection"
```

## Inspect the agent log

```powershell
Get-Content "C:\Program Files (x86)\ossec-agent\ossec.log" -Tail 40
```

Expected evidence includes a connected-to-server message and real-time FIM monitoring startup. In the dashboard, search for `syscheck` and set the time range to the test period.
