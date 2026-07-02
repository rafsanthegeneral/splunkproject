# 🏹 Adversary Emulation & Detection Engineering with Atomic Red Team

## 📝 Introduction

Defensive security is only as strong as the telemetries it can successfully validate. Rather than waiting for a real-world security breach to test infrastructure defenses, this project adopts a proactive **Purple Team** approach using Red Canary’s **Atomic Red Team** framework. 

Atomic Red Team provides highly predictable, small, and automated test cases ("atomics") that map directly to the **MITRE ATT&CK®** framework. By safely simulating malicious tactics, techniques, and procedures (TTPs) on a controlled Windows endpoint, this project demonstrates how to generate rich security telemetry, validate log ingestion via a Splunk Universal Forwarder, and build precise **Splunk SPL detection queries** to spot active threat actors.

---

## 🚀 Key Highlights

* **Framework Alignment:** All simulated attacks are mapped directly to specific **MITRE ATT&CK** matrices, tracking malicious activity from Initial Access to Impact.
* **Controlled Adversary Emulation:** Executes discrete, scriptable test cases designed to safely mimic real-world threat actor behaviors without destabilizing the target OS.
* **Telemetry Validation:** Verifies that critical event visibility vectors (such as Windows Security Event Logs and **Sysmon** process creation logs) are actively capturing critical forensic evidence.
* **Detection Loop Closure:** Transitions raw, simulated attack telemetry into production-ready Splunk alerts, reducing the Mean Time to Detect (MTTD).

---
# Projects 

1. [ATOMIC RED TEAM (ART) SETUP](https://github.com/rafsanthegeneral/splunkproject/blob/master/project%232-Atomic-Red-Team-Analysis/ART%23SetupModuleOnWindows.md) **Atomic Red Team** is an open-source framework developed by Red Canary that allows security teams to safely, quickly, and predictably test their defenses against real-world adversary behaviors.Here Showing how to install and run on windows using powershell.
2. [T1055.001->Dynamic-link Library Injection Semulation]()T1055.001 (Dynamic-link Library Injection) allows adversaries to run malicious code by injecting a DLL into a legitimate process . This technique is used to evade security defenses by masking malicious activity under a trusted process and to escalate privileges by gaining access to the target process's resources.