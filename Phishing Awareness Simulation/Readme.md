# Phishing Awareness Home Lab Project

**GoPhish + MailHog (Ethical, Closed Environment)**

---

## Project Overview

This project demonstrates a safe and ethical phishing-awareness simulation designed to help understand common phishing techniques and how user education can reduce risk.
The lab was conducted in a closed virtual environment using dummy users only, with no credential capture and no external email delivery.

The goal is awareness and defensive learning, not exploitation.

---

## Project Objectives

* Understand how phishing simulations work end-to-end
* Learn how attackers use severity, urgency and generic messaging
* Practice converting phishing clicks into security awareness moments
* Safely observe email content, headers, and user behavior
* Gain hands-on experience with GoPhish in a controlled lab environment

---

## Lab Environment

### Platform

* VMware Workstation
* Windows 10 Virtual Machine
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
<img width="1612" height="924" alt="Windows 10 in VMware" src="https://github.com/user-attachments/assets/2e40b6bb-481a-4b8b-8028-406aa0756880" />

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
<img width="1536" height="1024" alt="phishing Architectural diagram" src="https://github.com/user-attachments/assets/9f090985-8cb2-4893-9cfa-130d25cb689f" />

---

## Step-by-Step Lab Setup

---

### Step 1: Prepare the Virtual Machine

1. Install a Windows 10 iso VM in VMware Workstation
2. Allocate resources based on the capacity of your host machine for optimum performance a configuration example is

   * 4–6 GB RAM
   * 2 CPU cores
   * NAT networking
3. Install VMware Tools
4. Create a working directory:

   ```
   C:\Phishing-Lab or a folder to host your installation files on the VM to keep things tidy
   ```
---

### Step 2: Install and Run MailHog

MailHog captures all emails locally so that nothing leaves the lab.

1. Download the MailHog Windows binary, i downloaded the MailHog_windows_386 for my 32bit Windows VM
2. Place it in a folder or under downloads:

3. Open Command Prompt and run:

   ```cmd
   mailhog.exe

    A black terminal window will open. Keep this open.

   ```
4. Open browser:

   ```
   Test it:

   http://localhost:8025
   ```

📸 *Screenshot:* MailHog web interface (empty inbox)
<img width="1615" height="841" alt="Empty MailHog inbox to show the Email Catcher" src="https://github.com/user-attachments/assets/4a6490b1-2ae6-4bbe-a96f-17a161d98e74" />

---

### Step 3: Install and Run GoPhish

1. Download GoPhish compartible with your OS (i made us of the V 0.9.0, Windows 32-bit)
(Note that this is not the most recent version, as recent versions are for 64 bit systems)

2. Extract the GoPhish zip to your choosen folder
 
3. Start GoPhish: Right click GoPhish.exe and run as administrator
   
   ```cmd
   gophish.exe
   ```
4. Watch the terminal for the generated admin password in more recent versions. For V 0.9.0 the password is hardcoded as

5. Username: admin
   Password: gophish

📸 *Screenshot:* GoPhish running in Command Prompt (password blurred)
<img width="967" height="488" alt="GoPhish CMD" src="https://github.com/user-attachments/assets/5cd2019b-005a-47ec-9cce-1bd68409c8df" />

---

### Step 4: Sign In to GoPhish

1. Open browser:

   ```
   https://localhost:3333
   ```
2. Accept the self-signed certificate warning
3. Log in:

   * Username: `admin`
   * Password: *(from terminal output in more recent versions)*
   * V 0.90 Password: gophish 

4. Change the password immediately

📸 *Screenshot:* GoPhish dashboard before login
<img width="1613" height="763" alt="GoPhish Admin Dashboard before logging in - 1" src="https://github.com/user-attachments/assets/460596c3-6345-48ef-b6ff-69f48a55d844" />

---

### Step 5: Configure SMTP (MailHog)

1. Go to Sending Profiles
2. Create a new profile:

   * Name: `MailHog SMTP`
   * Host: `127.0.0.1`
   * Port: `1025`
   * Encryption: None
   * From: `IT Support <it-support@demo.local>`
3. Send a test email
4. Confirm it appears in MailHog

📸 *Screenshot:* Test email visible in MailHog
<img width="1612" height="836" alt="Test mails dropping" src="https://github.com/user-attachments/assets/9fdc4de6-ef5e-4a49-a0b9-d3e7bc5bf230" />
<img width="1610" height="838" alt="Test mail" src="https://github.com/user-attachments/assets/c59effe7-d45a-426b-9d72-db7136c8e184" />

---

### Step 6: Create Dummy Users

1. Go to Users & Groups
2. Create group:

   ```
   Demo Awareness Targets
   ```
3. Add users:

   * Kemi Bright — Kemi@demo.local
   * Mark Denzel — Mark@demo.local

📸 *Screenshot:* User group listing
<img width="1612" height="754" alt="Demo awareness group" src="https://github.com/user-attachments/assets/dd620e4a-e6f2-414b-993f-5841aa0d4dae" />
<img width="716" height="628" alt="Group members" src="https://github.com/user-attachments/assets/9e4d913c-b073-4c37-b42b-61622d6f81a6" />

---

### Step 7: Create the Phishing Email Template

1. Go to Email Templates
          Email Templates -> New Template.

          Name: LinkedIn Password Reset

          Subject: Security Alert: Please verify your account

HTML Content:
2. Subject:

   ```
   Action Required: Password Reset Needed
   ```
3. HTML body:

```<html>
<body>
    <p>Hi {{.FirstName}},</p>
    <p>We detected unusual activity on your LinkedIn account.</p>
    <p>Please click the link below to verify your identity:</p>
    <p><a href="{{.URL}}">Verify Account Now</a></p>
    <p>Thanks,<br>The LinkedIn Security Team</p>
</body>
</html>
   ```
Note that: {{.URL}} and {{.FirstName}} are variables GoPhish replaces automatically.

**Design Intent**

* Urgent tone would include a time line e.g within 24 hrs
* severity
* Generic greeting
* Single call-to-action

📸 *Screenshot:* Email template preview
<img width="1610" height="835" alt="Email Template" src="https://github.com/user-attachments/assets/75412041-b55c-4127-bb9d-5a8e7cfdc36f" />

---

### Step 8: Create the Awareness Landing Page

1. Go to Landing Pages
        Landing Pages -> New Page.

        Name: LinkedIn Awareness Training

        HTML Content: Switch to the Source view (code icon <>) and paste this simplified awareness template:
2. Page name:

   ```
   Linkedin Awareness Awareness
   ```
3. HTML content:

   ```
   <html>
   <head>
    <title>Security Awareness Training</title>
    <style>
        body { font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", "Fira Sans", Ubuntu, Oxygen, "Oxygen Sans", Cantarell, "Droid Sans", "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Lucida Grande", Helvetica, Arial, sans-serif; background-color: #f3f2ef; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; }
        .container { background: white; padding: 40px; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.15); text-align: center; max-width: 500px; }
        h1 { color: #d11124; margin-bottom: 20px; }
        p { color: #555; line-height: 1.5; margin-bottom: 20px; }
        .linkedin-btn { background-color: #0073b1; color: white; padding: 10px 20px; text-decoration: none; border-radius: 20px; font-weight: bold; }
    </style>
   </head>
   <body>
    <div class="container">
        <h1>⚠ This was a phishing simulation</h1>
        <p>You successfully clicked a simulated phishing link. If this had been a real attack, your credentials could have been compromised.</p>
        <p><strong>Red Flag:</strong> Always check the sender's email address and hover over links before clicking.</p>
        <br>
        <a href="https://www.reference website.com" class="linkedin-btn">Continue to LinkedIn</a>
    </div>
  </body>
  </html>
  ```
   
4. Ensure **credential capture is disabled**

📸 *Screenshot:* Awareness page rendered in browser
<img width="1613" height="840" alt="Awareness Landing Page to show the educational moment" src="https://github.com/user-attachments/assets/79ce8ca4-aa2d-4564-b666-5e2766763586" />

---

### Step 9: Launch the Campaign

1. Go to Campaigns
    Go to Campaigns -> New Campaign.

2. Configure:

   * Name: `Phishing Awareness Test`
   * Template: Password Reset
   * Landing Page: Awareness Page
   * URL: `http://127.0.0.1`
   * Sending Profile: MailHog SMTP
   * Group: Demo Awareness Users

3. Click Launch Campaign.

📸 *Screenshot:* Campaign configuration screen
<img width="766" height="740" alt="Phishing Campaign setup" src="https://github.com/user-attachments/assets/2aa52cc8-0f56-40d3-8277-2987629b71e3" />

---

### Step 10: Simulate User Interaction

1. I opened Kemi’s email in MailHog
2. Click the phishing link
3. Observe redirect to awareness page

📸 *Screenshot:* Awareness page after click
<img width="1616" height="844" alt="GoPhish dashboard showing the clicked status" src="https://github.com/user-attachments/assets/4100c4ff-5fe0-4694-a664-f50a39347d6b" />

---

### Step 11: Review Campaign Results

Navigate to campaign results.
<img width="1609" height="840" alt="Phishing campaign details" src="https://github.com/user-attachments/assets/3ed6cd33-dfaf-43ea-814d-8124878fccf4" />


**Metrics**

* Sent: 2
* Opened: 2
* Clicked: 2
* Submitted Data: 0
* Reported: 0

📸 *Screenshot:* Campaign results dashboard
<img width="1612" height="753" alt="2 clicks" src="https://github.com/user-attachments/assets/1e980e87-75a2-44f7-bca6-ccd2236fccb9" />

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
* Map behavior to MITRE ATT&CK (T1566 – Phishing)
* Introduce role-based awareness messaging
* Add metrics comparison across campaigns

---

## Final Notes

This project demonstrates defensive security thinking, ethical handling of phishing tools, and strong documentation practices.



Author: Oladapo Abolurin
---
