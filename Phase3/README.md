# Phase 3 – Defensive Strategy Proposal

## 🔐 Objective
Apply a security defense to mitigate the exploited vulnerability and validate that the defense prevents future exploitation.

## 🛡️ Defense Mechanism Implemented
- **Type:** e.g., Disable FTP service
- **Steps Taken:**
  ```bash
  sudo systemctl stop vsftpd
  sudo systemctl disable vsftpd
