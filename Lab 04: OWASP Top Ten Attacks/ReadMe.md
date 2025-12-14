# Lab 04: OWASP Top Ten Attacks

This repository documents my work in Web Security focusing on OWASP Top Ten Attacks.

## Lab Objectives
1. Set up IIS 10.0 with FTP services
2. Install OWASP Juice Shop on Windows Server 2016
3. Configure Burp Suite for intercepting HTTP traffic
4. Demonstrate directory traversal vulnerability
5. Execute XML External Entity (XXE) injection attack

## Lab Components

### Part 1: IIS Setup
- Installed IIS 10.0 with FTP services
- Verified access to default IIS page from Kali Linux

📸 See `screenshots/slide 1.png`
<img width="1100" height="661" alt="Slide 1" src="https://github.com/user-attachments/assets/8b676c83-6366-40a6-b659-3924f6ccf7b3" />

### Part 2: OWASP Juice Shop Installation
- Installed Node.js and Git for Windows
- Cloned and set up Juice Shop repository
- Launched Juice Shop on port 3000

📸 See `screenshots/slide 2.png`
<img width="1554" height="798" alt="Slide 2" src="https://github.com/user-attachments/assets/0b7fd9ab-a0d6-4589-84ba-e1b09bf6d799" />

### Part 3: Burp Suite Configuration
- Configured Burp Suite proxy on port 8888
- Set up Firefox to route traffic through Burp
- Installed PortSwigger CA certificate

📸 See `screenshots/slide 3.png` and `slide 4.png`
<img width="1530" height="842" alt="Slide 3" src="https://github.com/user-attachments/assets/c5d8f78e-7db6-4e94-8723-6c4351a664dc" />
<img width="1186" height="301" alt="Slide 4" src="https://github.com/user-attachments/assets/c2dbb287-4e9a-4abb-9705-267cf7f81801" />

### Part 4: Directory Traversal
- Exploited misconfiguration in Mutillidae to view directory contents
- Accessed `/mutillidae/documentation/` directory listing

📸 See `screenshots/slide 5.png` 
<img width="912" height="670" alt="Slide 5" src="https://github.com/user-attachments/assets/da5a628b-757f-4395-b7fc-a710182a3b0e" />

### Part 5: XXE Injection
- Demonstrated basic XML validation
- Created external entity references
- Extracted system file contents (/etc/passwd) via XXE

📸 See `screenshots/slide 6.png`
<img width="1304" height="690" alt="Slide 6" src="https://github.com/user-attachments/assets/912f0a1f-954d-4fd9-aab4-749761ad1d1c" />



# Lab 04 Findings Report

## Directory Traversal Vulnerability
Discovered that the Mutillidae application had directory listing enabled for the documentation folder. This allowed viewing of sensitive files including installation guides and test scripts.

**Impact:** Unauthenticated users could access potentially sensitive documentation and server information.

## XXE Injection Vulnerability
Successfully exploited the XML validator to:
1. Demonstrate basic entity reference
2. Read system files (/etc/ssh/ssh_config)
3. Extract user information (/etc/passwd)

**Impact:** This vulnerability could allow attackers to read arbitrary files on the server, potentially exposing sensitive configuration and user data.

## Mitigation Recommendations
1. **Directory Traversal:**
   - Disable directory listing in web server configuration
   - Implement proper access controls

2. **XXE Injection:**
   - Disable external entity processing in XML parsers
   - Use less complex data formats (JSON) when possible
   - Implement input validation for XML content

