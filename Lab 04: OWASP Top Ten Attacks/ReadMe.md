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

### Part 2: OWASP Juice Shop Installation
- Installed Node.js and Git for Windows
- Cloned and set up Juice Shop repository
- Launched Juice Shop on port 3000

📸 See `screenshots/slide 2.png`

### Part 3: Burp Suite Configuration
- Configured Burp Suite proxy on port 8888
- Set up Firefox to route traffic through Burp
- Installed PortSwigger CA certificate

📸 See `screenshots/slide 3.png` and `slide 4.png`

### Part 4: Directory Traversal
- Exploited misconfiguration in Mutillidae to view directory contents
- Accessed `/mutillidae/documentation/` directory listing

📸 See `screenshots/slide 5.png` 

### Part 5: XXE Injection
- Demonstrated basic XML validation
- Created external entity references
- Extracted system file contents (/etc/passwd) via XXE

📸 See `screenshots/slide 6.png`



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

