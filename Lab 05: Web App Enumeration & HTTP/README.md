# Lab 05: Web App Enumeration & HTTP

This repository documents my work for the Web Security Lab 05 focusing on HTTP traffic analysis and web application enumeration.

## Lab Objectives
1. Capture and analyze HTTP requests/responses using Burp Suite
2. Demonstrate session hijacking vulnerabilities
3. Manipulate HTTP headers (User-Agent)
4. Perform brute force attacks using Burp Intruder

## Lab Components

### Part 1: HTTP Traffic Analysis
- Captured and analyzed raw HTTP requests/responses
- Identified key header fields (User-Agent, Set-Cookie)
- Documented 404 error responses

### Part 2: Session Takeover
- Created test accounts (oabolurin-01, oabolurin-02)
- Captured and manipulated session cookies
- Successfully hijacked sessions between accounts
- Gained admin access via cookie manipulation

### Part 3: User-Agent Manipulation
- Modified User-Agent header to inject JavaScript
- Implemented Firefox User-Agent Switcher add-on
- Created custom User-Agent with dynamic content

### Part 4: Burp Intruder Attack
- Generated password list using random.org
- Configured Burp Intruder positions and payloads
- Identified successful login via 302 redirect


# Lab 05 Technical Findings

## Session Vulnerabilities
1. **Cookie Security Issues:**
   - Session cookies were transmitted in cleartext
   - No HttpOnly or Secure flags set
   - Session IDs were predictable

2. **Successful Hijacking:**
   - Transferred cookies between user sessions
   - Gained admin access by modifying UID values

## HTTP Header Manipulation
1. **User-Agent Injection:**
   - Successfully injected JavaScript via User-Agent
   - Demonstrated XSS potential through header fields
   - Footer content reflected raw User-Agent without sanitization

2. **Browser Fingerprinting:**
   - Default User-Agent exposes system details
   - Add-ons can help obscure fingerprinting

## Brute Force Vulnerabilities
1. **Password Guessing:**
   - No account lockout mechanism
   - Clear differentiation between failed/successful attempts (200 vs 302)
   - Weak passwords easily guessable

## Mitigation Recommendations
1. **Session Management:**
   - Implement HttpOnly and Secure cookie flags
   - Use cryptographically strong session IDs
   - Implement proper session binding

2. **Input Validation:**
   - Sanitize all reflected headers
   - Implement CSRF tokens

3. **Authentication:**
   - Implement account lockout after failed attempts
   - Require strong passwords
   - Use multi-factor authentication
