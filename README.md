## Windows Authentication Event Investigation with Wazuh

A hands-on security monitoring project investigating Windows failed and successful logon events using Wazuh SIEM.

# Project Overview

This project demonstrates how Windows Security logs can be collected and investigated in Wazuh. The investigation focused on failed logins (Event ID 4625), successful logins (Event ID 4624), the source address, and the sequence of authentication events.

Outcome: Wazuh detected the failed and successful authentication events. The evidence examined showed a local incorrect-password attempt followed by successful local authentication. It did not establish a remote brute-force attack.

# Lab Environment

Component	Configuration
SIEM	Wazuh 4.14.4
Wazuh Manager / Indexer / Dashboard	Kali Linux VM
Endpoint	Windows 10 Pro VM
Windows agent	DESKTOP-B8KCHD4, Agent ID 002
Windows endpoint IP	192.168.100.17
Wazuh Manager IP	192.168.100.15
Virtualization	Oracle VirtualBox
Network mode	Bridged

# Objectives

* Detect Windows failed logon events.
* Identify the account, logon type, and source address.
* Check for successful logons associated with the account.
* Build a timeline from the event timestamps.
* Review the generated alerts in the Wazuh Dashboard.
* Document findings and limitations.

# Investigation Process

1. Confirmed that the Windows Wazuh agent was active and connected to the manager.
2. Reviewed Windows Security events in Event Viewer.
3. Investigated Event ID 4625 for failed logons.
4. Investigated Event ID 4624 for successful logons.
5. Queried Wazuh alerts on Kali and verified that the events appeared in the Dashboard.
6. Compared event timestamps, account names, logon types, and source addresses.

# Findings

Failed logon

Field	Value
Windows Event ID	4625
Wazuh Rule ID	60122
Account	BENSIRA
Logon type	2 — Interactive
Source address	127.0.0.1
Failure substatus	0xc000006a — Incorrect password
Event time	November 18, 2026, 13:57:16 WAT

The source address 127.0.0.1 is the local loopback address. This event does not identify a remote source IP.

# Successful logon

A successful Event ID 4624 for BENSIRA was recorded at approximately 13:57:41 WAT, about 25 seconds after the failed attempt.

The successful event also showed Logon Type 2 and source address 127.0.0.1.

The Wazuh Dashboard showed one failed-login alert for Rule 60122 and four hits for successful-login Rule 60118. The successful-login hits were not automatically treated as four distinct sign-ins: linked elevated and non-elevated records can represent the same interactive login session.

# Timeline

Time (WAT)	Event	Observation
13:57:16	4625 — Failed logon	Local interactive logon with an incorrect password
13:57:41	4624 — Successful logon	Local interactive logon for the same account

# Conclusion

Wazuh successfully collected and generated alerts for Windows authentication events. The investigated failed logon was local and was followed shortly by a successful local logon.

Based on the available evidence, the activity is consistent with an incorrect-password attempt followed by successful authentication. The evidence does not confirm a remote brute-force attack or identify a remote attacker.

# Evidence

* 01-windows-event-4625.png
* 02-wazuh-failed-login-alert.png
* 03-windows-event-4624.png
* 04-wazuh-success-login-alerts.png
* 05-failed-login-count.png


# Skills Demonstrated

* Windows Security event investigation
* Wazuh SIEM alert review
* Authentication event correlation
* Log analysis and timeline construction
* Incident reporting and evidence documentation

# Disclaimer

This project was conducted in a controlled virtual lab for educational purposes. Findings are limited to the events and records reviewed during the investigation.
