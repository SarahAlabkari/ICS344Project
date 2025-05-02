# Phase #3 – Defensive Strategy Proposal

## Objective

Apply a security defense mechanism to mitigate the exploited SSH brute-force vulnerability, and validate that the defense effectively prevents future exploitation attempts.

## Defense Overview

- **Tool Used:** Fail2Ban
- **Targeted Service:** SSH
- **Defense Type:** Intrusion Prevention
- **Mechanism:** Automatically bans an IP address after multiple failed login attempts.

## Steps Taken to Implement the Defense

### 1. Install Fail2Ban on Metasploitable3 (Victim)

```bash
sudo apt update
sudo apt install fail2ban
```

### 2. Configure Fail2Ban for SSH Protection

#### Copy the default configuration:

```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

#### Edit the jail.local file:

```bash
sudo nano /etc/fail2ban/jail.local
```

#### Add or modify the `[sshd]` section:

```
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 600
```

### 3. Restart Fail2Ban

```bash
sudo service fail2ban restart
```

### 4. Verify Fail2Ban is Monitoring SSH

```bash
sudo fail2ban-client status sshd
```

## Environment Setup

- Metasploitable3 (Victim):
  NAT IP Address: 10.0.2.15
  Host-Only IP Address: 192.168.56.102
  Username: vagrant
  Password: vagrant

- Kali Linux (Attacker):
  Hostname: kali
  Host-Only IP Address: 192.168.56.101
  Username: kali
  Password: kali

## Testing the Defense

### 1. Connectivity Check

Ping from Kali to Metasploitable3 to confirm network reachability.

[![Screenshot of results]](image.png)

### 2. Before the Attack – SSH Jail Status

Verified Fail2Ban is active for SSH and no IPs are banned yet.

[![Screenshot of results]](image-1.png)

### 3. Simulating a Brute-Force Attack

- Used SSH from Kali to attempt login with wrong password.
- Failed intentionally three times.
- SSH session was forcefully closed, indicating Fail2Ban triggered the ban.

[![Screenshot of results]](image-2.png)

### 4. After the Attack – SSH Jail Status

Confirmed that the attacker’s IP was automatically banned by Fail2Ban.

[![Screenshot of results]`](image-3.png)

## Conclusion

Fail2Ban successfully defended the Metasploitable3 machine against brute-force SSH login attempts. After three failed login attempts, the attacker’s IP (`192.168.56.101`) was banned for 10 minutes, validating that the defense was properly configured and effective.
