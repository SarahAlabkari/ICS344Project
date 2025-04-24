# Phase 2 – SIEM Dashboard Analysis

## 🎯 Objective
Use Splunk to ingest and analyze log files from both attacker and victim machines to understand attack behavior.

## 🛠️ Tool Used
- **SIEM Platform:** Splunk (Free edition)

## 📥 Data Sources
- **Victim Logs:** `/var/log/auth.log`, `syslog`
- **Attacker Logs:** `.bash_history`, any relevant output logs

## 📊 Visualizations Created
- Timeline of attack
- IP source tracking
- File access attempts
- Login attempts

✅ *Screenshots of dashboards are in `attack_visualization/`*
✅ *Screenshots of data import are in `siem_integration_screenshots/`*

## 📌 Notes
All logs were transferred manually to Splunk and indexed. Filters and queries were used to identify attack stages.

---
