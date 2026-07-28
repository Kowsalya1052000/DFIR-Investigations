# 📡 Network Traffic Sampling & Packet Forensics Lab (`tcpdump` & Wireshark)

![Domain](https://img.shields.io/badge/Domain-Network_Security_%26_Forensics-blue?style=for-the-badge)
![Tool](https://img.shields.io/badge/Tool-TCPDump_%7C_Wireshark-orange?style=for-the-badge)
![Environment](https://img.shields.io/badge/Environment-WSL_Ubuntu_Linux-green?style=for-the-badge)
![Certification](https://img.shields.io/badge/CompTIA-Security%2B_Certified-success?style=for-the-badge)

## 📌 Scenario & Objective
**Role:** Intermediate-level Network Administrator (Accounting Firm)  
**Scenario:** The organization suspects workstation compromise. The objective is to engineer background bash scripts utilizing `tcpdump` to sample network traffic based on specific criteria (host targets, time intervals, file size limits) and output structured `.pcap` dump files for forensic analysis.

---

## 🛠️ Key Technical Takeaways & Highlights
* **Interface Enumeration:** Identified active system interfaces using `tcpdump -D` (`eth0`, `lo`, `any`).
* **Automated Packet Rotation:** Configured `tcpdump` with `-G` (time interval) and `-C` (file size limit) flags to generate sequenced output files without dropping kernel packets.
* **Traffic Inspection:** Exported raw PCAPs to Wireshark for layer-4 TCP handshake analysis, TLS 1.2 SNI header extraction (`apod.nasa.gov`), and NTP packet inspection.
* **Environment Execution:** Built and executed custom automation scripts (`watchdog.sh` and `sequenced.sh`) within WSL Linux (`Ubuntu`).

---

## 🚀 Lab Tasks & Workflow Summary

### Task 1 & 2: Interface Discovery & Live Filtering
* Discovered interfaces via `sudo tcpdump -D`. Selected `eth0` (`172.29.53.47`).
* Filtered specific host traffic using target domain resolutions (`host coursera.org`).

### Task 3: Dump File Creation & Payload Analysis
* Written packet stream directly to disk using `tcpdump -i eth0 -w capture.pcap`.
* Analyzed hex and ASCII representation using `tcpdump -r capture.pcap -XXtttt`.

### Task 4: Sequenced Capture Automation
* Developed automated Bash scripts to split traffic dumps every 15 seconds (`-G 15`) using timestamped naming schemes (`capture-%Y-%m-%d_%H-%M-%S.pcap`).

### Task 5: Wireshark Forensics & SSL Key Log Export
* Inspected captured `.pcap` files in Wireshark. Verified TLS 1.2 client handshakes, application data frames, and NTP synchronization payloads (`prod-ntp-3.ntp1.ps5.canonical.com`).

---

## 💻 Automation Scripts Used

### 1. `watchdog.sh` (Background Capture & Key Logging)
```bash

#!/bin/bash
# Export SSL Key Log file for session decryption
export SSLKEYLOGFILE=C:\Users\Kowsalya\sslkeys.log

# Launch browser session in background
/usr/bin/google-chrome-stable &

# Capture traffic to target domain with 600s rotation and 1MB size limit
sudo tcpdump -i eth0 host apod.nasa.gov -w capture.pcap -G 600 -C 1

#!/bin/bash
# Rotate dump files every 15 seconds with timestamp formatting
sudo tcpdump -i eth0 -G 15 -w capture-%Y-%m-%d_%H-%M-%S.pcap host coursera.org &

# Allow background process to run for 60 seconds
sleep 60  

# Terminate tcpdump process cleanly
sudo pkill tcpdump

🔬 Forensic Report & Permission Notes
Local Machine Hostname: LAPTOP-MBQ3RBSR (WSL Ubuntu)

Assigned Local IP: 172.29.53.47

Primary Interface: eth0

Storage & Permission Troubleshooting:
Issue Observed: Running tcpdump -C 1 -G 15 in default home directories triggered a Permission denied error when writing output .pcap files.

Resolution Implemented: Relocated target working directory to /mnt/c/Users/Kowsalya/OneDrive/Desktop/coder/Coursera/ using mv ~/capture-*.pcap commands to ensure proper Linux-to-Windows filesystem write permissions.

Author: Kowsalya)

Role: Cybersecurity & DFIR Engineer
