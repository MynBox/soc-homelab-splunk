# Home Lab SOC / SIEM: Splunk & Sysmon

## Project Description

A local SOC lab built with Splunk and Sysmon to collect logs and detect threats on a Windows endpoint. The setup generates, collects, and analyzes logs to identify malicious behavior.

## Architecture & Technologies

* **SIEM:** Splunk Enterprise (physical machine)
* **Target Endpoint:** Windows 11 VM
* **Log Collector:** Splunk Universal Forwarder
* **Telemetry:** Microsoft Sysmon (SwiftOnSecurity configuration)
* **Network:** Local communication via port `9997`

## Phase 1: Infrastructure Deployment & Visibility (Completed)

Set up the SOC foundation to get full visibility into the target machine.

* Installed and configured the Splunk Enterprise server.
* Deployed an isolated Windows 11 VM.
* Installed Sysmon to capture process creation, network connections, and other advanced events.
* Deployed Splunk Universal Forwarder on the endpoint.
* Configured `inputs.conf` to ingest:
* `WinEventLog://Application`
* `WinEventLog://Security`
* `WinEventLog://System`
* `WinEventLog://Microsoft-Windows-Sysmon/Operational`



## Troubleshooting

Sysmon logs initially failed to appear in Splunk.

* **Issue:** System and Security logs ingested correctly. Zero Sysmon events appeared in the Splunk index, even with a valid `inputs.conf`.
* **Investigation:**
1. Verified Sysmon logs generated locally in Windows Event Viewer.
2. Checked Splunk agent internal logs (`splunkd.log`).
3. Found this error: `WinEventLogChannel::init: Init failed, unable to subscribe to Windows Event Log channel 'Microsoft-Windows-Sysmon/Operational': errorCode=5`


* **Resolution:** Error code 5 means "Access Denied". The SplunkForwarder service ran under a restricted user account and couldn't read the Sysmon log channel. Changed the service log-on account to Local System. Data ingestion started immediately.

## Phase 2: Attack, Detection & Threat Hunting (Upcoming)

* Generate malicious traffic on the VM (RDP brute force, malware execution, registry modifications).
* Analyze logs using SPL.
* Build dashboards to track endpoint security status.
* Configure automated alerts to detect compromise.
