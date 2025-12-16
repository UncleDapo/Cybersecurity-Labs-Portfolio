# Phishing Awareness Home Lab

**GoPhish + MailHog (Ethical, Closed Environment)**

---

## Project Overview

This project demonstrates a **safe and ethical phishing-awareness simulation** designed to help understand common phishing techniques and how user education can reduce risk.
The lab was conducted in a **closed virtual environment** using **dummy users only**, with **no credential capture** and **no external email delivery**.

The goal is **awareness and defensive learning**, not exploitation.

---

## Project Objectives

* Understand how phishing simulations work end-to-end
* Learn how attackers use urgency and generic messaging
* Practice converting phishing clicks into **security awareness moments**
* Safely observe email content, headers, and user behavior
* Gain hands-on experience with GoPhish in a controlled lab environment

---

## Lab Environment

### Platform

* **VMware Workstation**
* **Windows 10 Virtual Machine**
* Network mode: **NAT**

### Tools Used

* **GoPhish** – phishing campaign management
* **MailHog** – local SMTP email catcher
* **Web browser** – simulates user interaction

### Why This Setup?

* Full isolation from host system
* No real emails sent
* Professional, enterprise-like environment
* Safe for screenshots and public documentation

📸 *Screenshot:* VMware Workstation showing Windows 10 VM running

---

## Lab Architecture

```
Windows 10 VM
 ├── GoPhish (Admin UI + Phish Server)
 ├── MailHog (Local SMTP Sink)
 └── Browser (User Simulation)
```

**Flow**

```
GoPhish → MailHog → User Opens Email → Clicks Link → Awareness Page
```

📸 *Screenshot:* Architecture diagram

---

## Step-by-Step Lab Setup

---

### Step 1: Prepare the Virtual Machine

1. Create a Windows 10 VM in VMware Workstation
2. Allocate:

   * 4–6 GB RAM
   * 2 CPU cores
   * NAT networking
3. Install VMware Tools
4. Create a working directory:

   ```
   C:\Phishing-Lab
   ```

📸 *Screenshot:* Windows 10 desktop inside VM

---

### Step 2: Install and Run MailHog

MailHog captures all emails locally so **nothing leaves the lab**.

1. Download the MailHog Windows binary
2. Place it in:

   ```
   C:\Phishing-Lab\MailHog
   ```
3. Open Command Prompt and run:

   ```cmd
   mailhog.exe
   ```
4. Open browser:

   ```
   http://localhost:8025
   ```

📸 *Screenshot:* MailHog web interface (empty inbox)

---

### Step 3: Install and Run GoPhish

1. Download GoPhish (Windows 64-bit)
2. Extract to:

   ```
   C:\Phishing-Lab\GoPhish
   ```
3. Start GoPhish:

   ```cmd
   gophish.exe
   ```
4. Watch the terminal for the **generated admin password**

📸 *Screenshot:* GoPhish running in Command Prompt (password blurred)

---

### Step 4: Sign In to GoPhish

1. Open browser:

   ```
   https://localhost:3333
   ```
2. Accept the self-signed certificate warning
3. Log in:

   * Username: `admin`
   * Password: *(from terminal output)*
4. Change the password immediately

📸 *Screenshot:* GoPhish dashboard after login

---

### Step 5: Configure SMTP (MailHog)

1. Go to **Sending Profiles**
2. Create a new profile:

   * Name: `MailHog SMTP`
   * Host: `127.0.0.1`
   * Port: `1025`
   * Encryption: None
   * From: `IT Support <it-support@demo.local>`
3. Send a test email
4. Confirm it appears in MailHog

📸 *Screenshot:* Test email visible in MailHog

---

### Step 6: Create Dummy Users

1. Go to **Users & Groups**
2. Create group:

   ```
   Demo Awareness Users
   ```
3. Add users:

   * Alice Demo — [alice@demo.local](mailto:alice@demo.local)
   * Bob Demo — [bob@demo.local](mailto:bob@demo.local)

📸 *Screenshot:* User group listing

---

### Step 7: Create the Phishing Email Template

1. Go to **Email Templates**
2. Subject:

   ```
   Action Required: Password Reset Needed
   ```
3. HTML body:

   ```html
   <p>Hello,</p>
   <p>We detected unusual activity on your account.</p>
   <p><a href="{{.URL}}">Reset Password</a></p>
   <p>IT Support Team</p>
   ```

**Design Intent**

* Urgent tone
* Generic greeting
* Single call-to-action

📸 *Screenshot:* Email template preview

---

### Step 8: Create the Awareness Landing Page

1. Go to **Landing Pages**
2. Page name:

   ```
   Phishing Awareness – Test Page
   ```
3. HTML content:

   ```html
   <h2>This Was a Phishing Simulation</h2>
   <p>You clicked a simulated phishing email.</p>
   <ul>
     <li>Urgent language</li>
     <li>Generic greeting</li>
     <li>Unexpected password reset</li>
   </ul>
   <p><a href="https://www.linkedin.com">Go to the real site</a></p>
   ```
4. Ensure **credential capture is disabled**

📸 *Screenshot:* Awareness page rendered in browser

---

### Step 9: Launch the Campaign

1. Go to **Campaigns**
2. Configure:

   * Name: `Phishing Awareness Test`
   * Template: Password Reset
   * Landing Page: Awareness Page
   * URL: `http://127.0.0.1`
   * Sending Profile: MailHog SMTP
   * Group: Demo Awareness Users
3. Launch immediately

📸 *Screenshot:* Campaign configuration screen

---

### Step 10: Simulate User Interaction

1. Open Alice’s email in MailHog
2. Click the phishing link
3. Observe redirect to awareness page
4. Repeat for Bob

📸 *Screenshot:* Awareness page after click

---

### Step 11: Review Campaign Results

Navigate to campaign results.

**Expected Metrics**

* Sent: 2
* Opened: 2
* Clicked: 2
* Submitted Data: 0
* Reported: 0

📸 *Screenshot:* Campaign results dashboard

---

## Ethical Disclaimer

> This project was conducted in a closed lab environment using dummy accounts and MailHog as a local SMTP sink. No real emails were sent, no credentials were collected, and no external systems were involved. This project is intended strictly for phishing awareness education and defensive security learning.

---

## Key Learning Points

* Urgent language combined with generic greetings can still drive clicks
* Awareness pages turn mistakes into learning opportunities
* MailHog enables safe inspection of phishing email content and headers
* GoPhish is effective for ethical training when properly configured
* Isolation via virtualization is critical for security labs

---

## What More Can Be Done (Future Enhancements)

* Compare multiple subject lines and urgency levels
* Add a simulated “Report Phish” button
* Analyze email headers in detail
* Map behavior to **MITRE ATT&CK (T1566 – Phishing)**
* Introduce role-based awareness messaging
* Add metrics comparison across campaigns

---

## Final Notes

This lab demonstrates **defensive security thinking**, ethical handling of phishing tools, and strong documentation practices suitable for a cybersecurity portfolio.

---
