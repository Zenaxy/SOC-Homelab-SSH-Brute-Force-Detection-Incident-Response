# Detection Analysis – SSH Brute Force Attack

## 🕒 Timeline Summary

| Time | Activity                                         |
| ---- | ------------------------------------------------ |
| T0   | Normal SSH login from Kali to Windows (baseline) |
| T1   | Multiple failed SSH authentication attempts      |
| T2   | High-frequency connections from single source IP |
| T3   | Successful authentication after brute force      |
| T4   | Incident response: attacker IP blocked           |

---

## 🎯 Incident Overview

A suspected **SSH brute force attack** was detected against a Windows endpoint running **OpenSSH Server**.
The attack originated from a Kali Linux machine within the same isolated lab network.

The objective of this analysis was to:

* Identify brute force behavior
* Attribute the activity to a source IP
* Validate detection using endpoint telemetry
* Perform basic containment actions

---

## 📡 Data Sources

| Source             | Description                   |
| ------------------ | ----------------------------- |
| Sysmon             | Network connection telemetry  |
| Windows Event Logs | OpenSSH authentication events |
| Splunk SIEM        | Centralized log analysis      |

---

## 🔎 Detection 1 – SSH Network Activity (Sysmon Event ID 3)

### Observation

Multiple TCP connections to **destination port 22 (SSH)** were observed in a short time window.

### Relevant Fields

* `EventID`: 3
* `Image`: `C:\Windows\System32\OpenSSH\sshd.exe`
* `SourceIp`
* `DestinationIp`
* `DestinationPort`

### SPL Query

```spl
index=endpoint EventID=3 Image="*sshd.exe*" DestinationPort=22
| stats count by SourceIp
| where count > 5
```

### Analysis

A single source IP generated an unusually high number of SSH connections to the same destination, indicating **automated authentication attempts** rather than normal user behavior.

---

## 🔐 Detection 2 – Authentication Failure Pattern

### Observation

Repeated failed SSH login attempts occurred before a successful authentication.

### Behavioral Indicators

* Multiple failed attempts
* Same username targeted repeatedly
* Short time interval between attempts

### SOC Interpretation

This pattern is consistent with **credential brute force attacks**, where attackers test multiple passwords rapidly until success.

---

## 🧠 Detection 3 – Successful Login After Failures

### Observation

After multiple failed attempts, a successful SSH authentication was observed from the same source IP.

### Why This Matters

A successful login following numerous failures significantly increases confidence that:

* Credentials were guessed or cracked
* The system was compromised
* Immediate response is required

---

## 🛑 Incident Response Performed

### Action Taken

* Attacker IP address was **blocked using Windows Firewall**
* SSH access from the malicious source was denied

### Verification

* Subsequent SSH connection attempts failed
* Firewall logs confirmed blocked traffic
* No further successful logins observed

---

## ⚠️ False Positive Considerations

Potential benign scenarios that may resemble this activity:

* Misconfigured automation scripts
* Repeated manual login attempts by a user

Mitigations:

* Apply thresholds (e.g., >5 attempts)
* Correlate with successful login events
* Review source IP reputation and behavior

---

## 🧠 Conclusion

This analysis demonstrates how **SSH brute force attacks can be detected using endpoint network telemetry**, even without deep packet inspection.

By correlating:

* Network connection volume
* Authentication behavior
* Source IP patterns

SOC analysts can quickly identify suspicious activity and take containment actions.

---

## 📘 Lessons Learned

* Sysmon Event ID 3 is effective for SSH activity monitoring
* Threshold-based detection reduces false positives
* Context (failed → successful login) is critical
* Detection must be paired with response to reduce risk
