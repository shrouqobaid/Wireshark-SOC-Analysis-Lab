# Network Traffic Analysis: Brute Force Detection & SOC Lab

## Table of Contents
- [Project Overview](#project-overview)
- [Skills & Tools](#skills--tools)
- [Investigation Workflow](#investigation-workflow)
- [Key Findings](#key-findings)
- [Wireshark Filters Used](#wireshark-filters-used)
- [SOC Analyst Perspective](#soc-analyst-perspective)
- [Incident Response & Mitigation](#incident-response--mitigation)
- [Lab Architecture](#lab-architecture)
- [Repository Structure](#repository-structure)

---

## Project Overview
This project demonstrates the practical application of network traffic analysis to detect and investigate a **Brute Force Attack**. Using **Wireshark** and **Kali Linux**, I simulated a real-world attack scenario, captured live traffic, and performed a forensic analysis of packets to identify security risks, tool signatures, and evidence of compromise.

## Skills & Tools
* **Operating System:** Kali Linux
* **Packet Analyzer:** Wireshark
* **Attack Tool:** THC Hydra
* **Protocols Analyzed:** HTTP, DNS, OCSP, ICMP.
* **Key Skills:** Network Monitoring, Traffic Filtering, Attacker Fingerprinting, Deep Packet Inspection (DPI).

---

## Investigation Workflow

### 1. Attack Execution (The Source)
I simulated a dictionary-based brute force attack using **Hydra** against an HTTP login form to generate malicious traffic patterns.
* **Evidence:** ![Hydra Execution](screenshots/hydra-attack.png)

### 2. Reconnaissance & DNS Analysis
Monitoring the network for scanning activity and DNS resolution to identify the early stages of the attack.
* **Analysis:** Observed **ICMP Type 3** packets, indicating active host discovery and port scanning.
* **Evidence:** ![Recon Analysis](screenshots/dns-analysis.png)

### 3. Brute Force Detection & Fingerprinting
Isolated authentication attempts and identified the tool's digital signature via Deep Packet Inspection (DPI).
* **Forensic Evidence:** Identified the `User-Agent` string specifically linked to the **Hydra** tool.
* **Evidence:** ![Hydra Signature](screenshots/hydra-signature.png)

### 4. Vulnerability Assessment (Plaintext Exposure)
Investigated the lack of encryption that allowed sensitive data to be exposed during transit.
* **Finding:** Sensitive paths and data were visible in **Plaintext** due to the use of insecure HTTP.
* **Evidence:** ![Plaintext Leak](screenshots/plaintext-leak.png)

### 5. Incident Confirmation
Reconstructed the session stream to provide definitive proof of a successful account breach.
* **Result:** The server responded with a clear **"success"** message, confirming the impact.
* **Evidence:** ![TCP Stream Success](screenshots/tcp_stream_success.png)

---

## Key Findings
- **Detected brute force attack activity** via high-frequency POST requests.
- **Identified Hydra attack signature** through Deep Packet Inspection (DPI).
- **Observed reconnaissance scanning behavior** using ICMP and DNS logs.
- **Detected plaintext exposure risk** due to insecure HTTP communication.
- **Captured malicious authentication attempts** and confirmed account compromise.

---

## Wireshark Filters Used
I used these specific filters to isolate and analyze the malicious traffic:
```wireshark
http.request.method == "POST" # To isolate login attempts
dns                          # To monitor name resolution
icmp                         # To detect scanning & unreachable hosts
tcp                          # To analyze session streams
ip.addr == [Target_IP]       # To focus on the victim machine```

---


## Incident Response & Mitigation
To secure the environment against the identified threats, I recommend the following security controls:

* **Enforce Encryption:** Transition all web services from HTTP to **HTTPS (TLS 1.3)** to prevent sensitive data (like credentials) from being sniffed in plaintext.
* **Account Lockout Policy:** Implement strict lockout mechanisms after 5 failed attempts to thwart automated brute-force tools like **Hydra**.
* **Multi-Factor Authentication (MFA):** Add a second layer of defense to ensure that compromised passwords alone are not enough for an account takeover.


## SOC Analyst Perspective
> **Strategic Insight:** This investigation demonstrates how packet analysis serves as a foundational skill for incident response. 
> 
> By meticulously analyzing traffic, SOC analysts can:
> * **Identify Attack Patterns:** Recognize brute force and reconnaissance activity before they escalate.
> * **Understand Attacker Behavior:** Map out the TTPs (Tactics, Techniques, and Procedures) used by threat actors.
> * **Proactive Defense:** Detect insecure communications (like plaintext protocols) and implement mitigation steps before a breach occurs.
> 
> Mastering these forensic techniques is essential for effective **Threat Hunting** and building a proactive security posture.


## Lab Architecture
The following diagram illustrates the flow of the attack and the strategic point where traffic was captured for analysis:

```text
[ Attacker Machine ]               [ Target Server ]
(Kali Linux + Hydra)               (Web Login Form)
         |                                |
         |------- Brute Force POST ------>|
         |                                |
         |<------- HTTP 200 (Success) ----|
         |               |                |
         |        [ Wireshark ]           |
         |    (Traffic Capture/DPI)       |```

---

## 📂 Repository Structure

- **captures**: Contains the raw PCAP file for further investigation.
- **screenshots**: Annotated visual evidence of the forensic analysis.
- **README.md**: Professional project documentation and technical report.

