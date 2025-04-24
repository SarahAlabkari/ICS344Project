# Phase 1 – Service Setup and Exploitation

## 🧩 Objective
Set up the attacker (Kali Linux) and victim (Metasploitable3) machines, and successfully exploit a vulnerable service using both Metasploit and a custom script.

## 🔧 Environment Setup
- **Kali Linux:** Attacker VM with Metasploit
- **Metasploitable3:** Victim VM with vulnerable services
- **Network:** Host-only adapter for VM communication

## 🛠️ Selected Service for Attack
- **Service Name:** e.g., vsftpd FTP
- **Port:** e.g., 21

## 🚨 Attack 1 – Using Metasploit
- **Exploit Used:** `exploit/unix/ftp/vsftpd_234_backdoor`
- **Steps Taken:** `msfconsole → search → use → set RHOST → exploit`
- ✅ *Screenshot attached in `metasploit_attack_screenshots/`*

## 🧪 Attack 2 – Custom Script
- **Script Language:** Python
- **Script Functionality:** Connect to service, execute a command
- ✅ *Script and screenshot available in `custom_script_attack/`*

---
