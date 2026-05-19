# Network Traffic Analysis: Brute Force Detection & SOC Lab

## Project Overview
This project demonstrates the practical application of network traffic analysis to detect and investigate a **Brute Force Attack**. Using **Wireshark** and **Kali Linux**, I simulated a real-world attack scenario, captured live traffic, and performed a forensic analysis of packets to identify security risks, tool signatures, and evidence of compromise.

## Skills & Tools
* **Operating System:** Kali Linux
* **Packet Analyzer:** Wireshark
* **Attack Tool:** THC Hydra
* **Protocols Analyzed:** HTTP, DNS, OCSP, ICMP.
* **Key Skills:** Network Monitoring, Traffic Filtering, Attacker Fingerprinting, Deep Packet Inspection (DPI).

---

## Lab Scenarios & Deep Analysis

### 1. Attack Execution (The Source)
To generate realistic forensic data, I simulated a dictionary-based brute force attack using **Hydra** against an HTTP login form.
* **Execution:** `hydra -L users.txt -P pass.txt [Target_URL] http-post-form ...`
* **Technical Insight:** This phase resulted in identifying valid credentials, providing a clear pattern of malicious traffic for analysis.
* **Evidence:** ![Hydra Execution](./screenshots/hydra_execution_results.png)

### 2. Reconnaissance & DNS Analysis
I monitored the network for scanning activity and DNS resolution to identify the early stages of the attack.
* **Findings:** Observed `ICMP Type 3 (Destination Unreachable)` messages.
* **Deep Dive:** These packets indicate that the adversary was performing **Port Scanning** to map active services. Identifying this phase is crucial for early Threat Hunting.
* **Evidence:** ![Recon Analysis](./screenshots/recon_dns_analysis.png)

### 3. Brute Force Detection & Fingerprinting
Using the filter `http.request.method == "POST"`, I isolated the authentication attempts.
* **Fingerprinting:** By inspecting the `User-Agent` header, I identified the specific forensic signature of **"Hydra"**.
* **SOC Analyst Note:** Identifying tool-specific strings is key to **Attacker Profiling**, allowing us to understand the tools and techniques (TTPs) used by the threat actor.
* **Evidence:** ![Hydra Signature](./screenshots/hydra_bruteforce_signature.png)

### 4. Vulnerability Assessment (Plaintext Exposure)
I investigated the captured traffic to identify why the system was vulnerable.
* **Forensic Finding:** As shown in the **Hex Dump**, sensitive directory paths and file requests (e.g., `success.txt`) are visible in **Plaintext**. 
* **Security Risk:** This proves the lack of TLS/SSL encryption, making the environment highly susceptible to **Packet Sniffing** and **Man-in-the-Middle (MitM)** attacks.
* **Evidence:** ![Plaintext Leak](./screenshots/plaintext_data_leak.png)

### 5. Incident Confirmation (The Smoking Gun)
To conclude the investigation, I reconstructed the communication stream to confirm the breach.
* **Analysis:** Following the **TCP Stream** allowed me to see the full session reconstruction.
* **Result:** The server’s response containing the word **"success"** provides definitive proof of a successful account takeover.
* **Evidence:** ![TCP Stream Success](./screenshots/tcp_stream_success.png)

---

## Mitigation & Recommendations
Based on the forensic analysis, I recommend the following security controls:
1. **Enforce Encryption:** Transition all web services to **HTTPS** to prevent data from being sniffed in plaintext.
2. **Account Lockout & Rate Limiting:** Implement policies to block IPs after multiple failed login attempts to thwart automated tools.
3. **Behavioral Monitoring:** Configure IDS/IPS rules to alert on high-frequency POST request bursts and non-standard User-Agents.

---

## Repository Structure
* `/captures`: Contains the raw PCAP file for further investigation.
* `/screenshots`: Annotated visual evidence of the attack and forensic analysis.
* `README.md`: Professional project documentation and technical report.
