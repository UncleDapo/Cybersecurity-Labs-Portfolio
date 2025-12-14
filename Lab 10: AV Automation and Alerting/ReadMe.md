# Lab 10 — Anti-Virus Automation

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
- Sendmail configuration snippets and `gmail-auth` file.
- Screenshots showing scan log output and alert emails.

## Tools & Technologies
- Ubuntu Server (lab VM)
- ClamAV (`clamscan`, `clamd`)
- Sendmail / `mailutils`
- Cron job scheduling
- Bash scripting
- Gmail (app password for SMTP relay)

## Security & Privacy Notes
- Secrets have been replaced with placeholders (e.g., `<GMAIL_ADDRESS>`, `<APP_PASSWORD>`).
- Screenshots have been redacted to remove any sensitive data.

## Installation of ClamAV 
```bash
#I installed the Clam Anti-Virus software on the Ubuntu Web server by running the commands
sudo apt update
sudo apt install -y clamav clamav-daemon clamav-freshclam

#Update definitions (freshclam may run as a service)
sudo freshclam

#The Clam Anti-Virus should already be running so to ensure that it starts up on boot, i added it to the services that automatically start up by running the commands below:

update-rc.d clamav-daemon defaults
update-rc.d clamav-freshclam defaults
service clamav-daemon start

#To create a script to scan the web server
#Scripts can then be automated so that the commands run as scheduled. Create a new file called **simple_scan.sh** and enter the following into the file:

#!/bin/bash
SCAN_DIR="/home"
LOG_FILE="/var/log/clamav/manual_clamscan.log"
/usr/bin/clamscan -ri $SCAN_DIR >> $LOG_FILE

#i saved the file and ensured that the script had execute permissions using chmod 755 before running the script as shown in the scrrenshot below.
```


**📸Screenshot of the scan_log.png — terminal showing cat /var/log/clamav/manual_clamscan.log (evidence of run)**
<img width="658" height="303" alt="scan_log" src="https://github.com/user-attachments/assets/0f42f929-2a18-4cf9-b256-56201a60c452" />


# Installation and configuration Send Mail
To install Send Mail and related utilities i used the command below

`apt-get install sendmail mailutils sendmail-bin`

The lab example will be using a gmail account in order to relay emails from the Ubuntu Web Server so to accomplish this, i did two things:

First, i ensure that my gmail account will be able to authenticate the web server.

i went to Gmail, and selected Manage Google Account -> Security -> 2-Step Verification

then scrolled down to see App Passwords

Under Select app, i choose Mail, then selected a device

i choose Other and gave it a name "Ubuntu Web Server"

i clicked on GENERATE for Gmail to create an app password that can be used for my emails. This password will not be shown again, so i noted it down to ensure that i have handy.

Then i created a Gmail Authentication file:
```bash
mkdir -m 700 /etc/mail/authinfo/
cd /etc/mail/authinfo/
```

Once in the new directory, i created an auth file with a following content:

`AuthInfo: "U:root" "I:YOUR GMAIL EMAIL ADDRESS" "P:YOUR GENERATED APP PASSWORD"`

** The password used here is the one generated for App passwords in Gmail **

i went ahead to create a hash map for the authentication file:

`makemap hash gmail-auth < gmail-auth`
![gmail page](https://github.com/user-attachments/assets/3a43dd1c-aef0-43db-9091-cfea1a9ad888)

before using a text editor (notepad) to edit the send mail configuration file by placing the following lines into  `/etc/mail/sendmail.mc` right above the first “MAILER” definition line:

```bash
define(`SMART_HOST',`[smtp.gmail.com]')dnl
define(`RELAY_MAILER_ARGS', `TCP $h 587')dnl
define(`ESMTP_MAILER_ARGS', `TCP $h 587')dnl
define(`confAUTH_OPTIONS', `A p')dnl
TRUST_AUTH_MECH(`EXTERNAL DIGEST-MD5 CRAM-MD5 LOGIN PLAIN')dnl
define(`confAUTH_MECHANISMS', `EXTERNAL GSSAPI DIGEST-MD5 CRAM-MD5 LOGIN PLAIN')dnl
FEATURE(`authinfo',`hash -o /etc/mail/authinfo/gmail-auth.db')dnl
```
then i saved the changes to the file and exited

```bash
#To re-build sendmail's configuration,

make -C /etc/mail
Reload sendmail service:
/etc/init.d/sendmail reload

#Send test email from terminal using the following command:

echo "Just testing my sendmail gmail relay" | mail -s "Sendmail 6076 Relay works!" sample@gmail/yahoo.com
```

`Note: You need to replace sample@gmail/yahoo.com with your email address`
The email may take a minute or so to come in so be patient…


**📸screeshot of the inbox_email.png — Gmail (or recipient inbox) showing the test email sent from server**
<img width="1366" height="573" alt="inbox_email" src="https://github.com/user-attachments/assets/722c0988-8c86-4030-8f94-e6cc1a4f4971" />


# Automate the AV scan and email alert with a cron job
In order to have the script run regularly, i created a script and added it to the cron jobs by creating a new file called auto_clam_scan and placing it in `/etc/cron.hourly/` by entering the following code into the new file:

```bash
#!/bin/bash
# auto_clam_scan - placed in /etc/cron.hourly/ to run hourly
SCAN_DIR="/home /tmp /var"
LOG_FILE="/var/log/clamav/auto_clam_scan.log"
SUBJECT="Potential Threat Detected"
EMAIL="sample@gmail/yahoo.com"
EMAIL_FROM="your_test_gmail@gmail.com"
AGGRESSIVE=0

# run scan
/usr/bin/clamscan -ri $([ $AGGRESSIVE -eq 1 ] && echo --remove) $SCAN_DIR >> $LOG_FILE

# check results and send mail if infections found
if [ `tail -n 12 ${LOG_FILE} | grep Infected | grep -v 0 | wc -l` != 0 ]; then
  SCAN_RESULTS=$(tail -n 10 $LOG_FILE | grep 'Infected files')
  INFECTIONS=${SCAN_RESULTS##* }
  EMAILMESSAGE=$(mktemp /tmp/virus-alert.XXXXX)
  {
    echo "To: ${EMAIL}"
    echo "From: ${EMAIL_FROM}"
    echo "Subject: ${SUBJECT}"
    echo "Importance: High"
    echo "X-Priority: 1"
    echo "***A Scan was carried out by oabolurin***"
    tail -n $((10 + $INFECTIONS)) $LOG_FILE
  } > ${EMAILMESSAGE}
  sendmail -t < ${EMAILMESSAGE}
fi
```

To ensure that the script has execute permissions `chmod +x /etc/cron.hourly/auto_clam_scan`

**📸slide03_cron_email.png — Inbox with automated cron-triggered alert email (or modified no-threat email).**
<img width="1287" height="918" alt="cron_email" src="https://github.com/user-attachments/assets/7c60212f-573f-468c-beac-6ecc064c3a6c" />

Run the script manually, If you have no infections, no email will be generated. However, the script can be adjusted so that it sends out an email notification when NO threats are detected.

Run the script again and check to see if you have received the email notification
