# Network Traffic Analysis: Brute Force Detection Lab

## Project Overview
This project demonstrates the practical application of network traffic analysis to detect and investigate a **Brute Force Attack**. Using **Wireshark** and **Kali Linux**, I simulated an attack scenario, captured the live traffic, and analyzed the packets to identify security risks and tool signatures.

## Skills & Tools
* **Operating System:** Kali Linux
* **Packet Analyzer:** Wireshark
* **Attack Tool:** Hydra
* **Protocols Analyzed:** HTTP, DNS, OCSP, ICMP.
* **Key Skills:** Network Monitoring, Traffic Filtering, Fingerprinting, Forensic Analysis.

---

## Lab Scenarios & Analysis

### 1. Reconnaissance & DNS Analysis
Initially, I monitored the network for any scanning activity and DNS resolution. 
* **Findings:** Observed `ICMP Type 3 (Destination Unreachable)` messages indicating blocked ports or unreachable services, which is a key indicator during the reconnaissance phase.
* **Evidence:** `screenshots/dns_error_analysis.png`

### 2. Live HTTP Traffic Capture
Successfully captured unencrypted HTTP traffic to demonstrate the vulnerability of plaintext communication.
* **Findings:** Captured OCSP requests and identified connectivity checks.
* **Evidence:** `screenshots/http_and_ocsp_traffic_capture.png`

### 3. Brute Force Attack Detection (The Core Finding)
I simulated a dictionary-based brute force attack using **Hydra** against an HTTP login form.
* **Execution:** `hydra -L users.txt -P pass.txt [Target_URL] http-post-form ...`
* **Analysis:** Using the filter `http.request.method == "POST"`, I identified multiple authentication attempts from the same source IP within seconds.
* **Fingerprinting:** By inspecting the `User-Agent` header, the specific tool **"Hydra"** was identified, leaving a clear forensic signature.
* **Evidence:** `screenshots/brute_force_post_requests_list.png`

---

## Mitigation & Recommendations
Based on the analysis, I recommend the following security controls:
1.  **Encryption:** Enforce HTTPS to prevent credentials from being sniffed in plaintext.
2.  **Rate Limiting:** Implement account lockout policies and rate limiting to thwart brute force tools like Hydra.
3.  **Monitoring:** Deploy IDS/IPS signatures specifically looking for automated User-Agents and rapid POST request bursts.

---

## Repository Structure
* `/captures`: Contains the raw PCAP file for further investigation.
* `/screenshots`: Visual evidence of the attack and analysis.
* `README.md`: Project documentation and report.
