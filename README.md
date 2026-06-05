# Splunk SIEM LOG ANALISYS PROJECT's

A curated collection of hands-on Splunk SIEM projects designed for log analysis. Each project includes sample datasets, step-by-step ingestion guides, and structured workflows to extract actionable security insights from diverse log sources.

## Projects 

1. [Splunk Installation And Forwader Setup](https://github.com/rafsanthegeneral/splunkproject/blob/master/project%231-Setup-Splunk-Enterprise%26SplunkUniversalForwader.md) A comprehensive guide to setting up a distributed Splunk architecture. This project demonstrates how to deploy, configure, and connect Splunk Forwarders and Receivers in a mixed environment of Linux and Windows servers.

2. [🏹 MITRE ATT&CK Analysis with Atomic Red Team](https://github.com/rafsanthegeneral/splunkproject/blob/master/project%232-Atomic-Red-Team-Analysis.md)

**An advanced adversary simulation and detection engineering project focused on validating SIEM defenses.**

This project bridges the gap between offensive and defensive security by executing simulated cyber attacks using Red Canary's **Atomic Red Team** framework. By triggering specific techniques mapped directly to the **MITRE ATT&CK** matrix on a Windows endpoint, this lab demonstrates how a Security Analyst tracks malicious footprints, parses telemetry, and crafts robust **Splunk SPL queries** to detect active adversarial behaviors in real time.

#### 🛠️ Key Highlights:
* **Adversary Simulation:** Executed automated, controlled attack scripts simulating real-world threat actors.
* **MITRE ATT&CK Mapping:** Aligned telemetry directly with specific TTPs (Tactics, Techniques, and Procedures).
* **Detection Validation:** Proved exactly how default Windows Event Logs and Sysmon capture complex behaviors like execution, persistence, or privilege escalation.
