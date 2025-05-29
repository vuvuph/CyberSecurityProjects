# Documentation of Network Scanning and Security Assessment: Using Nmap, Shodan, and Wireshark

This project showcases a practical exploration of information gathering and vulnerability assessment techniques using industry-standard tools such as **Nmap**, **Shodan**, and **Wireshark** within a **Kali Linux** environment. The objective was to identify exposed devices and systems across the internet, analyze their network behavior, and evaluate potential security weaknesses such as open ports, default credentials, outdated services, and expired SSL certificates.

The workflow includes:
- Performing targeted searches on Shodan to discover vulnerable devices and services.
- Conducting port scans with Nmap to investigate service exposure and firewall behavior.
- Capturing and analyzing network traffic with Wireshark to assess potential data leakage and insecure communications.

This project serves as a foundational exercise in ethical hacking and penetration testing, emphasizing passive and active information gathering methods. All activities were conducted in a controlled and educational context, with a strong emphasis on responsible cybersecurity practices.

---

## 🧰 Tools Used

| Tool | Description |
|------|-------------|
| **Kali Linux** | A Debian-based OS packed with tools for digital forensics and penetration testing. It includes numerous security tools (e.g. Nmap, Wireshark) and is ideal for penetration testers. |
| **Shodan** | A search engine specifically designed for finding Internet-connected devices and systems. |
| **Wireshark** | A free and open-source packet analyzer tool, pre-installed in Kali Linux. |

---

## 🖥️ Environment Setup

### Performing Basic Searches in Shodan

#### Searching for Legacy Windows Operating Systems

Search Query:

```bash
os:”Windows Vista”
```

![image](https://github.com/user-attachments/assets/7bef9b84-aa3e-41c4-bf85-412c72d9b471)


#### Searching for Devices with Default/Generic Credentials

Search Query:

```bash
default password
```

![image](https://github.com/user-attachments/assets/57ab89a2-b706-4fc6-b147-ca393a78a212)


#### Searching for Webcam 7 (the most popular webcam and network camera software for Windows)

Search Query:

```bash
server: “webcam 7”
```

![image](https://github.com/user-attachments/assets/8bfd8807-6bfb-4e8d-aa4e-87bc216d189b)

---

## 🔍 Port Scanning

Performing a **TCP SYN port scan** using Nmap based on one of the IP addresses retrieved from **Shodan**.
Command:

```bash
nmap -sS 176.119.58.49
```

![image](https://github.com/user-attachments/assets/afbde321-2ede-4990-a418-df392a446fde)

All scanned ports are **open**, indicating a highly exposed system with many potential entry points for an attacker (e.g., no firewall or filtering in place).

### ⚠️ Notable Ports and Risks

- **Port 22 (SSH):**  
  May be **brute-forced** or exploited if weak credentials or outdated software are used.

- **Port 23 (Telnet):**  
  Transmits all data, including passwords, in **plain text**, making it a critical vulnerability if left unsecured.

- **Ports 80 / 81 / 443 (HTTP/HTTPS):**  
  Commonly used for **web apps**. May expose outdated software, admin panels, or poorly secured APIs.

### 🛠️ Other Useful Nmap Commands

#### `-A` (Aggressive Scan)
Enables advanced options like:
- OS detection  
- Version scanning  
- Script scanning  
- Traceroute

#### `-T4` (Timing Template, Level 4 = Aggressive)
Specifies how aggressive the scan should be.  
Ranges from:
- `T0` → **Paranoid**  
- `T5` → **Insane**  

`T4` is suitable for **broadband or Ethernet connections**, offering speed without being too noisy.

#### `-sT` (TCP Connect Scan)
Used when SYN scan (`-sS`) is not possible.  
This type uses the system’s `connect()` call to establish a full TCP connection.  
💡 **Note:** This is the default TCP scan if you don’t have raw packet privileges.

#### `-sA` (TCP ACK Scan)
Used to **map firewall rules** and determine:
- Which ports are **filtered**
- Whether the firewall is **stateful**

---

## 🚧 Traffic Analysis using Wireshark

### Step 1: Identify a Web Server with expired SSL Certificate (using Shodan)

Search Query:

```bash
ssl.cert.expired:true
```

#### Attempting to connect to the website results in a warning:

![image](https://github.com/user-attachments/assets/8a7e5a8f-d459-446a-8f81-d85f85b352cc)

This warning confirms the certificate is expired, making it a suitable target for traffic analysis and further inspection.

### Step 2: Capture and Analyze Network Traffic Using Wireshark

Using Wireshark while interacting with the website to capture the traffic:

![image](https://github.com/user-attachments/assets/4ad257c0-66d1-42a6-b8f8-730a64a814b2)

#### Filters used for analysis:
- ‘http’
- ‘http.request.method == "POST"’

These filters helped narrow down requests that may potentially expose sensitive data, such as login credentials or unencrypted form submissions.
