# Phase #3 – Defensive Strategy Implementation with Fail2Ban

## Objective

Defend against SSH brute-force login attacks on Metasploitable3 by configuring and validating a Fail2Ban setup that automatically bans IP addresses after multiple failed login attempts.

## Defense Tool Used: Fail2Ban

**Monitored Log File:** `/var/log/auth.log`
**Attacker Machine:** Kali Linux
**Victim Machine:** Metasploitable3

## Steps Taken to Apply and Test the Defense

### 1. Configure Fail2Ban on Metasploitable3

- Opened the Fail2Ban jail configuration file:

  ```bash
  sudo nano /etc/fail2ban/jail.local
  ```

- Added the following configuration:

  ```ini
  [sshd]
  enabled = true
  port = ssh
  filter = sshd
  logpath = /var/log/auth.log
  maxretry = 3
  bantime = 600
  ```

- Restarted the Fail2Ban service:

  ```bash
  sudo service fail2ban restart
  ```

**Screenshot:**
[![Screenshot of results]](image-6.png)

### 2. Trigger the Attack from Kali Linux

- Launched an SSH brute-force simulation using repeated incorrect passwords:

  ```bash
  ssh msfadmin@192.168.56.103
  ```

- Repeated login attempts until the server blocked access:

  ```
  Permission denied
  ...
  ssh: connect to host 192.168.56.103 port 22: Connection refused
  ```

**Screenshot:**
[![Screenshot of results]](image-5.png)

### 3. Confirm the Attack Was Blocked

- Checked Fail2Ban status:

  ```bash
  sudo fail2ban-client status sshd
  ```

- Verified failed login entries in the auth log:

  ```bash
  sudo tail -n 15 /var/log/auth.log
  ```

**Screenshot:**
[![Screenshot of results]](image-7.png)

## Environment Setup

**Metasploitable3 (Victim)**
Host-Only IP: 192.168.56.103
User: vagrant

**Kali Linux (Attacker)**
Host-Only IP: 192.168.56.104
User: kali

## Conclusion

Fail2Ban successfully detected and blocked the brute-force SSH attack from the Kali machine. It did so by monitoring `/var/log/auth.log`, identifying repeated failed login attempts, and banning the attacker IP after 3 retries. The defense was confirmed by both system logs and blocked SSH access.
