## Threat Model Report – Web Application (STRIDE)

Author: Oladapo Abolurin

Role: Information Security Officer

# 📌 Executive Summary

This threat model analyzes a web application using the STRIDE methodology (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege). The assessment identifies key risks such as identity spoofing, data tampering, unauthorized access, and service disruption. Mitigations focus on strong authentication, encryption, secure development practices, and continuous monitoring.

# 🏗️ Solution Architecture Overview

The web application follows a structured workflow to ensure secure data processing and communication. Users interact with the frontend, which securely communicates with the backend API using HTTPS. The API processes requests, enforces authentication and authorization policies, and interacts with the database layer. Security controls such as WAF, IDS/IPS, and SIEM solutions continuously monitor network traffic and application behavior for anomalies. Role-based access control ensures that only authorized users can perform specific actions, while encrypted storage and secure transmission methods protect sensitive data. Automated security scans and regular audits further strengthen the system against emerging threats.


- Frontend: Web / Mobile clients

- Backend: API services secured with HTTPS

- Security Controls: WAF, IDS/IPS, SIEM

- Access Control: Role-Based Access Control (RBAC)

- Data Protection: Encryption at rest and in transit

- Assurance: Automated scans and periodic audits
  

> System Architecture Diagram
<img width="863" height="622" alt="image" src="https://github.com/user-attachments/assets/762e8431-9019-4e95-b8cd-998bd11dc52a" />

# Data Flow
The Data Flow Diagram (DFD) highlights the critical pathways for data movement within the web application. By implementing robust security controls at each stage, potential vulnerabilities can be mitigated, ensuring the confidentiality, integrity, and availability of data. Regular security audits and continuous monitoring are essential to maintaining a secure and resilient system.


> Data Flow Diagram
<img width="964" height="532" alt="image" src="https://github.com/user-attachments/assets/091abb2c-f0a1-447c-987e-62bfc4f851f5" />


Security controls are applied at each trust boundary to preserve confidentiality, integrity, and availability (CIA).

# 🔍 Key Findings by STRIDE Category

1️⃣ Spoofing (Identity Impersonation)
- Threats:
  > Web application spoofing (T-0001)
  >
  > Human user impersonation (T-0003)
  
- Impact: Unauthorized access, data breaches

- Mitigations:
  > Enforce Multi-Factor Authentication (MFA) and device binding.
  > 
  > Use digital certificates for identity verification.

2️⃣ Tampering (Data Modification)
- Threats:
  >	SQL database tampering (T-0002, T-0006).
  >
  >	HTTP data interception (T-0004).
  
- Impact: Data corruption, privilege escalation.
- Mitigations:
  >Encrypt sensitive database columns.
  >
  >Enforce TLS 1.2/1.3 and input validation.

3️⃣ Repudiation (Lack of Accountability)
- Threats:
  >Users denying actions (T-0019).
  
- Impact: Inability to trace malicious activity.
-	Mitigations:
>Implement detailed logging and audit trails.
>
>	Use cryptographically signed logs.

4️⃣ Information Disclosure (Data Exposure)
- Threats:
>Sensitive data in error messages (T-0024).
>
>Insecure API responses (T-0029).
- Impact: Leakage of confidential data.
- Mitigations:
>Sanitize error messages; disable debug mode in production.
>
>Encrypt API responses and validate access controls.

5️⃣ Denial of Service (DoS)
-	Threats:
>	Resource exhaustion (T-0011).
>
>	SQL database overload (T-0023).

-	Impact: Application downtime.
-	Mitigations:
>	Implement rate limiting, auto-scaling, and query optimization.
>
>	Use DDoS protection (e.g., AWS Shield).

6️⃣ Elevation of Privilege
-	Threats:
>	Unauthorized privilege escalation (T-0014, T-0015).

- Impact: Admin-level access compromise for attackers.
-	Mitigations:
>	Enforce strict RBAC and least-privilege principles.
>
>	Monitor for exploit attempts using SIEM tools.


# 🛡️ Top Mitigation Recommendations
- Authentication & Access Control

- Enforce MFA and RBAC

- Secure APIs using OAuth 2.0

- Data Protection

- Encrypt data at rest and in transit

- Prevent SQLi/XSS via input validation and CSP

- Monitoring & Resilience

- Enable SIEM and anomaly detection

- Implement auto-scaling and load balancing

- Secure Development

- Use secure cookies (HttpOnly, Secure, SameSite)

- Integrate static code analysis into CI/CD

# 📊 Threat Summary Table (Excerpt)
| ID       |	Category  | Risk	Status             |
|----------|------------|--------------------------|
| T-0001 | Spoofing	| App impersonation	Mitigated |
|T-0005 |	Tampering	| XSS	Partially Mitigated |
|T-0010	| Spoofing	| NoSQL spoofing	Not Mitigated |
|T-0018	| Spoofing |	Weak API auth	Needs Review |
|T-0030 |	Info Disclosure |	NoSQL encryption	Needs Review |

Full threat register available in the detailed report.

# 📦 Impacted Assets

- Web Application

- SQL Database

- NoSQL Database

- API Endpoints

- Cloud Storage

- Human Users
