siap 👍
kita bikin **Repo #2** dengan **rapi, SOC-oriented, dan konsisten** sama yang barusan kamu kerjain.

Aku mulai dari **README.md FINAL** yang **tinggal copy–paste ke GitHub**.

---

# 🛡️ SOC Homelab – SSH Brute Force Detection & Incident Response (Splunk)

## 📌 Overview

This project demonstrates a **SOC (Security Operations Center) homelab** focused on **detecting and responding to SSH brute force attacks** using **Splunk SIEM** and **Sysmon** on a **Windows endpoint**.

The lab simulates attacker behavior from a Kali Linux machine, generates real endpoint telemetry, detects suspicious activity in Splunk, and performs **incident response by blocking the attacker IP**.

This project is designed to reflect **real-world SOC workflows**, not exploitation.

---

## 🎯 Objectives

* Simulate SSH brute force attacks in a controlled lab
* Generate Windows endpoint telemetry using Sysmon
* Detect brute force behavior using Splunk SPL
* Visualize attack activity in a Splunk dashboard
* Perform basic incident response (IP blocking)
* Practice SOC-style investigation and documentation

---

## 🧪 Lab Environment

### Attacker Machine

* **OS:** Kali Linux
* **Role:** Simulated attacker (SSH brute force generation)
* **Tools:** Hydra, SSH client

### Target Machine

* **OS:** Windows 10
* **Services:** OpenSSH Server
* **Logging:**

  * Sysmon
  * Windows Event Logs
  * Splunk Universal Forwarder

### SIEM

* **Platform:** Splunk Enterprise
* **Index:** `endpoint`
* **Log Source:** Sysmon (Event ID 3 – Network Connections)

### Network Configuration

* **VM Network:** Host-Only / Internal Network
* **Purpose:** Fully isolated lab environment

---

## 🔧 Tools Used

* Splunk Enterprise
* Splunk Universal Forwarder
* Sysmon
* OpenSSH Server (Windows)
* Kali Linux
* Hydra (SSH brute force simulation)

---

## 🧠 Attack Simulation

The following activities were performed **only to generate logs for detection and analysis**:

1. Normal SSH login to establish baseline activity
2. SSH brute force attack using Hydra from Kali Linux
3. Multiple failed authentication attempts against Windows OpenSSH
4. Successful login after password discovery

⚠️ All activity was conducted in an **isolated virtual lab**.

---

## 🔍 Detection & Analysis

Detection was performed using **Sysmon network events** and Splunk SPL queries, focusing on:

* SSH network connections (Sysmon Event ID 3)
* Source IP analysis
* High-volume connection attempts
* Threshold-based brute force detection

Detailed investigation steps:
➡️ `analysis/detection-analysis.md`

---

## 🚨 Detection Rules

SOC-style detection logic and SPL queries are documented here:
➡️ `analysis/detection-rule.md`

Rules include:

* Detection name & description
* Data source
* SPL logic
* Thresholds
* False positive considerations
* Recommended response actions

---

## 📊 Splunk Dashboard

A custom Splunk dashboard was created to visualize:

* Total SSH connection attempts
* Unique source IPs
* Potential brute force IPs
* Connection volume per attacker IP

The dashboard XML is aligned with **Windows Sysmon telemetry**, not Linux logs.

---

## 🛑 Incident Response

After confirming brute force behavior:

* The attacker IP was **blocked using Windows Firewall**
* SSH access from the attacker was denied
* Logs confirmed blocked connection attempts

This simulates **basic SOC containment actions**.

---

## 📸 Screenshots

Screenshots are included to demonstrate:

* Brute force attack execution (Hydra)
* Sysmon SSH network events
* Splunk detection queries
* Brute force dashboard
* Firewall rule blocking the attacker

---

## 🚨 Disclaimer

This project is for **educational and defensive security purposes only**.
No real-world systems were targeted.
All testing was performed in a **fully isolated lab environment**.

---

## 📘 What I Learned

* How SSH brute force attacks appear in Windows telemetry
* How to detect brute force behavior using network-based logs
* How to build SOC-style SPL detections
* How dashboards help analysts quickly identify threats
* How to perform basic incident response actions

---

