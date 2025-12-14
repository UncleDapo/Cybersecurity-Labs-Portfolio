🧪 Zero Trust IAM  — Microsoft Entra ID (Azure AD)
--
Objective:
The objective of this project is to build a functional Zero Trust Identity & Access Management lab using Microsoft Entra ID to practice MFA enforcement, Conditional Access design, break-glass accounts,
named locations, and modern identity security fundamentals.
📸 Zero Trust Architecture Diagram
<img width="6924" height="1746" alt="Zero Trust Architecture" src="https://github.com/user-attachments/assets/1b174568-2317-4efc-b413-b45091594035" />

Below is a summary of the key configurations and controls I implemented to achieve this and screenshots of the lab is also provided.

---
⚙️ Lab Prerequisites
✔ Internet connection
✔ A Microsoft account
✔ Access to Microsoft Entra Admin Center
✔ At least Entra ID Free (CA requires P1 or P2, but the 30-day trial is enough)

I activated and used the P2, 30-day trial
-

No costs are required — everything here works with:

Entra ID free

Entra ID P2 trial (it is important to note that at least a P1 is required to be able to create conditional access policies)

Azure 30-day free tenant

Create Test Users
-
Path:
Microsoft Entra ID → Users → New User

Create these users:

1. lab-admin (Global Admin) - the default user 

2. staff1

3. student1

4. Emergency-admin1

5. Emergency-admin1

📸 Screenshot for Users
<img width="1919" height="912" alt="Users" src="https://github.com/user-attachments/assets/18f731c3-e19c-46ea-8417-ec0d63781810" />

   
Create the Required Groups
-
Path:
Microsoft Entra ID → Groups → New Group

Create:

Staff → add staff1

Students → add student1

BreakGlass → add Emergency-admin1 + Emergency-admin2

📸 Screenshot for Groups
<img width="1919" height="912" alt="Groups" src="https://github.com/user-attachments/assets/3fe0bf29-a35a-4ae2-a2a2-a0d32939cd2e" />


DISABLE SECURITY DEFAULTS
-

Conditional Access cannot operate with Security Defaults ON.

Go to:
Microsoft Entra ID → Properties → Manage Security Defaults

Set Enable Security Defaults = No

Confirm “Disable”

CREATE A TRUSTED IP NAMED LOCATION
-

To find your public IP, In a browser → search What is my IP or use https://ipchicken.com

Copy the public IP

In portal:
Microsoft Entra ID → Protection → Conditional Access → Named locations

Click New location

Name: Trusted – Home

Add the IP (or /32)

Check Mark as trusted

Click Create

🚀Conditional Access Policies creation
-
I created 3 policies: 

🚀CONDITIONAL ACCESS POLICY A (Enforce MFA for Staff (All Apps, All Locations);
Path:
Microsoft Entra ID → Protection → Conditional Access → Policies → New policy

Name: CA – MFA for Staff (All Apps)

Users → Include → Select users/groups → Staff

Cloud apps → All cloud apps

Conditions → (none needed)

Grant → Require multi-factor authentication

Enable → Report-only

Create

To test Using Report-Only Mode
-
Open an incognito window

Sign in as staff1

Go to:
Microsoft Entra ID → Monitoring → Sign-ins

Select the sign-in

Confirm it shows:
"Would have required: MFA"

📸 Screenshot for MFA Required & MFA Sign in
<img width="1913" height="975" alt="MFA required" src="https://github.com/user-attachments/assets/9e57c5f8-b25c-402e-8c94-5f7570bee68a" />

<img width="1912" height="969" alt="MFA sign in" src="https://github.com/user-attachments/assets/afbb806d-56f2-4492-b4ae-e606c2652c55" />



🚀CONDITIONAL ACCESS POLICY B (Enforce MFA for Students ONLY When Off Trusted Network);
Path:
Conditional Access → Policies → New policy

Name: CA – Students MFA Off Trusted Network

Users → Include → Students group

Cloud apps → All cloud apps

Conditions → Locations:

Include → Any location

Exclude → Trusted – Home (select Named Locations)

Grant → Require MFA

Enable → Report-only

Create

📸 Screenshot for Students MFA required config
<img width="1919" height="917" alt="Students MFA required config" src="https://github.com/user-attachments/assets/839ab7e6-bf8d-44bc-9659-c62c39e2f83f" />



Validate With What-If Tool
-
Path:
Conditional Access → What If

User: student1

Target Resource: User actions

Location: choose a non-trusted IP

Click What If

Expected:

Student MFA policy → Applies

Staff MFA policy → Does NOT apply

📸 Screenshot for What If (a) & (b)
<img width="1919" height="917" alt="What If (a)" src="https://github.com/user-attachments/assets/f46e9b08-6f83-4f9c-8e0b-fb33915f9985" />
<img width="1919" height="912" alt="What If (b)" src="https://github.com/user-attachments/assets/9252f7de-add3-40a7-8e61-8e56c1c48e5d" />




## 🛟 Break-Glass Safety Net

### 1. Created Two Emergency Access Accounts  
- Configured cloud-only accounts  
- Excluded from **all Conditional Access policies**

### 2. Disabled Security Defaults  
- Ensured Conditional Access can operate normally  
- Tested safe fallback sign-ins for emergency admins

IMPLEMENT BREAK-GLASS ACCOUNTS
(Critical to prevent accidental admin lockout)

Create BreakGlass Accounts

(Already created: Emergency-admin1, Emergency-admin2)

Ensure they have:

Strong password

Assigned Global Administrator role (optional but recommended)


Exclude BreakGlass from ALL CA Policies

For each CA policy:

Open the policy

Users → Exclude → Select BreakGlass group

Save

📸 Screenshot for Conditional Access Policies
<img width="1919" height="918" alt="Conditional Access Policies" src="https://github.com/user-attachments/assets/3ff6b500-8d63-473b-82de-a1e810d7c5fd" />


---


## 🚫 Block Legacy Authentication

### 1. Prepared Conditional Access Policy  
- Targeted **Basic/Legacy authentication**  
- Configured to **block legacy sign-ins**  
- Helps reduce exposure to common password spray and brute-force attacks

🚀CONDITIONAL ACCESS POLICY C (Block Legacy (Basic) Authentication);
Path:
Conditional Access → Policies → New Policy

Name: CA – Block Legacy Authentication

Users → Include → All Users

Users → Exclude → BreakGlass

Cloud Apps → All Cloud Apps

Conditions → Client Apps → Configure = Yes

Check Legacy authentication clients

Grant → Block access

Enable = Report-only (first!)

Create

📸 Screenshot for CA -Block Legacy Authentication
<img width="1919" height="918" alt="CA -Block Legacy Authentication" src="https://github.com/user-attachments/assets/2bf6398d-0d57-458e-a203-47aeb7cd0ebc" />


---
### 1. Enforced MFA for Staff  
- Applied to **all apps**  
- Applied to **all locations**  

### 2. Enforced MFA for Students (Off-Trusted Network Only)  
- MFA enforced **only when students sign in from untrusted IPs**

### 3. Validated All Policies Using the What-If Tool  
- Used **Report-only** mode first  
- Verified expected MFA prompts and policy behavior

---

## 🎓 Skills & Experience Gained

Through this lab, I gained hands-on experience in:

- Zero Trust identity architecture  
- Designing Conditional Access policies  
- Implementing break-glass accounts  
- MFA governance & enforcement  
- Named locations and network-based access controls  

All of these are essential components of **modern identity security**.

---

## 🔜 Next Steps

In the upcoming phase of the lab, I plan to explore:

- Privileged Identity Management (PIM)  
- Identity Protection (risk-based policies)  
- App registration & consent governance  

---

