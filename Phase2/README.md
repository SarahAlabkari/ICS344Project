# Phase #2 – SIEM Dashboard Analysis

## Objective

Analyze SSH brute-force login activity by forwarding log data from the victim machine to a SIEM platform (Splunk), then visualize and compare successful vs. failed login attempts.

## SIEM Tool Used: Splunk Enterprise

**Source Logs:** `/var/log/auth.log`  
**Attacker Machine:** Kali Linux  
**Victim Machine:** Metasploitable3

## Steps Taken to Set Up SIEM and Analyze Logs

### 1. Export `auth.log` File from Metasploitable3

- File path: `/var/log/auth.log`
- Copied from victim VM to Kali using:
  ```bash
  sudo cp /var/log/auth.log /home/vagrant/
  chmod 644 /home/vagrant/auth.log
  scp vagrant@192.168.56.103:/home/vagrant/auth.log .
  ```

### 2. Upload `auth.log` to Splunk on Another Machine

- Accessed Splunk Web Interface at `http://<splunk-ip>:8000`
- Uploaded the log via **Add Data** → **Upload**
- Assigned:
  - Source type: `RanaANDSarah`
  - Host: `kali`
  - Source: `auth.log`

## Environment Setup

Metasploitable3 (Victim):
Host-Only IP: 192.168.56.103
User: vagrant

Kali Linux (Attacker):
Host-Only IP: 192.168.56.104
User: kali

Splunk Host:
External system with Splunk Enterprise Web Interface installed

## Visualization & Queries

### 1. Count of Successful vs. Failed Logins

Search query:

```spl
source="auth.log" host="kali" sourcetype="RanaANDSarah" ("Accepted password" OR "Failed password")
| eval Result=if(searchmatch("Failed password"),"Failed", "Accepted")
| stats count by Result
```

**Result:**

- Accepted: 6 logins
- Failed: 42 attempts

**Screenshot:**
[](image.png)

### 2. Search for Successful Logins Only

Query:

```spl
source="auth.log" host="kali" sourcetype="RanaANDSarah" "Accepted password"
```

**Screenshot:**
[](image-1.png)

### 3. Search for Failed Logins Only

Query:

```spl
source="auth.log" host="kali" sourcetype="RanaANDSarah" "Failed password"
```

**Screenshot:**
[](image-2.png)

## Conclusion

The SIEM analysis revealed that the SSH service on Metasploitable3 experienced a high number of failed login attempts relative to successful ones. The visual insights confirm brute-force behavior, emphasizing the importance of implementing protective measures, such as Fail2Ban.
