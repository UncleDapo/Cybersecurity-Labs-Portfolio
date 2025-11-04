# Lab 10 — Anti-Virus Automation (INFO-6076)

## Overview
This lab demonstrates how to automate malware scanning and alerting on a Linux web server. Using **ClamAV** for virus detection and **Sendmail** for SMTP relay, the lab shows how to schedule regular scans, parse results, and notify administrators automatically when infections are found. The exercise combines system administration, shell scripting, logging, and secure email relay configuration.

## Goals
- Install and configure ClamAV (`clamav`, `clamav-daemon`, `clamav-freshclam`).
- Create a reusable shell script to perform recursive scans and write results to a log.
- Configure Sendmail with Gmail (using an app password) to relay alert emails.
- Automate scanning and alerting using a cron job (`/etc/cron.hourly`).
- Demonstrate and document the scan, log output, and alert email flow.

## Deliverables
- `simple_scan.sh` — manual scan script that logs results to `/var/log/clamav/manual_clamscan.log`.
- `auto_clam_scan` — cron script placed in `/etc/cron.hourly/` that runs scans and emails alerts when infections are found.
- Sendmail configuration snippets and `gmail-auth` file (note: DO NOT commit real credentials to public repos).
- Screenshots showing scan log output and alert emails.

## Tools & Technologies
- Ubuntu Server (lab VM)
- ClamAV (`clamscan`, `clamd`)
- Sendmail / `mailutils`
- Cron job scheduling
- Bash scripting
- Gmail (app password for SMTP relay)

## Security & Privacy Notes
- **Never** commit plaintext passwords or app passwords to a public repository. Replace secrets with placeholders (e.g., `<GMAIL_ADDRESS>`, `<APP_PASSWORD>`).
- Sanitize logs/screenshots to remove any sensitive data (real email addresses, network ranges, or production hostnames) before publishing.

## Quick Commands (examples)
```bash
# Install ClamAV
sudo apt update
sudo apt install -y clamav clamav-daemon clamav-freshclam

# Update definitions (freshclam may run as a service)
sudo freshclam

# Example manual scan script execution
sudo /home/youruser/simple_scan.sh

# View the manual scan log
sudo less /var/log/clamav/manual_clamscan.log

# Place the cron script (example)
sudo cp auto_clam_scan /etc/cron.hourly/
sudo chmod +x /etc/cron.hourly/auto_clam_scan

# Test Sendmail (after config)
echo "Test mail" | mail -s "Sendmail test" you@example.com

