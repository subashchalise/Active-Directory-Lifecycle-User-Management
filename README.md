# 🛠️ Active Directory User Lifecycle & Ticketing Project

## 📋 Project Overview
This project simulates a real-world enterprise IT environment using the **JobSkillShare (JSS)** professional lab sandbox. I designed and implemented a tiered **Organizational Unit (OU)** structure from scratch to manage the full user lifecycle while resolving common Help Desk tickets.

---

### 💻 Environment & Tools
| Component | Specification |
| :--- | :--- |
| **Lab Environment** | JobSkillShare (JSS) IT Pro Sandbox |
| **Domain Controller** | Windows Server 2022 |
| **Client Workstation** | Windows 11 Pro (Domain Joined) |
| **Domain Name** | `ACILABS.COM` |
| **Organization Name** | Forest City Tech Solutions (FCTS) |

---

## 📂 Active Directory Design (OU Structure)
I built a professional, scalable hierarchy under the FCTS root OU to ensure the directory stays organized as the company grows:

```text
ACILABS.COM (Root)
└── FCTS (Top Level OU)
    ├── Groups (Security Groups)
    ├── Users (Employee Accounts)
    │   ├── IT
    │   ├── HR
    │   └── Marketing
    └── Computers (Workstations)
```
![Onboarding Verification](images/ticket1.png)

---

 ## 🎫 Help Desk Ticket Simulation



### 🟢 Ticket 1: New Hire Provisioning & Security Onboarding

> **Scenario:** A new hire, **Sarah Jenkins (sjenkins)**, joined the IT department. My goal was to set up her account following the company’s security and folder structure.

#### **Technical Actions:**
* **Implementation:** Created the user object `sjenkins` within the **FCTS > Users > IT** Organizational Unit. This ensures she automatically receives the correct department settings based on her physical location in the directory.
* **Access Control:** Established a **Global Security Group** named `IT-Group` within the **Groups** OU to facilitate Role-Based Access Control (RBAC).
* **Security Policy:** Enforced the `User must change password at next logon` requirement. This aligns with industry-standard security protocols, ensuring the Administrator has zero knowledge of the user’s permanent credentials.
* **Verification:** Successfully authenticated as `sjenkins` on the **Windows 11 Client**. Executed `whoami /groups` to verify that the IT-Group Security Identifier (SID) was correctly added to the user's Kerberos access token.

#### **📸 Technical Evidence:**

**1. ADUC Provisioning: Defining the `sjenkins` User Object**
![User Creation](images/ticket2a.png)
*Defining account name and logon credentials within the targeted IT Organizational Unit.*

**2. Credential Security: Enforcing Mandatory Password Change**
![Security Flag](images/ticket2b.png)
*Configuring initial security flags to ensure user-owned credential privacy.*

**3. RBAC Setup: Creating the `IT-Group` Security Group**
![Group Creation](images/ticket2c.png)
*Setting up the departmental security group for streamlined permission management.*

**4. Policy Verification: Forced Password Change on Client**
![Client Prompt](images/ticket2d.png)
*Verifying that the server-side security policy propagated correctly to the Windows 11 workstation.*

**5. Technical Validation: Access Token Group Confirmation**
![whoami Command](images/ticket2e.png)
*Using CLI to confirm the account successfully inherited all IT-Group permissions.*

---

### 🟡 Ticket 2: Identity Recovery & Security Reset

> **Scenario:** **Michael Chen (mchen)** from HR reported a total loss of workstation access. His account was locked out after multiple failed login attempts, triggering the domain's security protection.

#### **Technical Actions:**
* **Security Policy:** Configured an **Account Lockout Threshold** of 3 attempts via the **Default Domain Policy** to protect the environment against brute-force attacks.
* **Diagnostic:** Identified the lockout status on the Domain Controller by navigating to the **Account** tab in ADUC, confirming the user was restricted from authenticating.
* **Resolution:** Performed a secure administrative password reset while clearing the `Unlock the user's account` flag to restore immediate access.
* **Security Maintenance:** Re-enforced the `User must change password at next logon` policy to ensure the new temporary credential was immediately replaced by a private one known only to the user.

#### **📸 Technical Evidence:**

**1. Security Enforcement: Account Lockout Message on Client**
![Client Lockout](images/ticket3a.png)
*Verifying the Active Directory security policy successfully blocked access on the Windows 11 workstation.*

**2. Admin Diagnostic: Identifying the Lockout in ADUC**
![ADUC Lockout Status](images/ticket3b.png)
*Locating the specific lockout flag within the user's account properties on the Domain Controller.*

**3. Technical Resolution: Resetting Password and Unlocking Account**
![Unlock Action](images/ticket3c.png)
*Executing the recovery process to restore user access while maintaining administrative zero-knowledge security.*

---

### 🔵 Ticket 3: Internal Transfer & Departmental Move

> **Scenario:** **Michael Chen** was promoted from HR to the IT Department. I was tasked with migrating his account to the new Organizational Unit and re-aligning his access permissions to match his new technical responsibilities.

#### **Technical Actions:**
* **OU Migration:** Physically migrated the `mchen` user object from the **HR OU** to the **IT OU**. This ensures the account inherits the correct Group Policy Objects (GPOs) and reflects the updated organizational hierarchy.
* **Permission Re-alignment:** Updated security group memberships by removing Michael from the legacy HR group and adding him to the **IT-Group**. This maintains the **Principle of Least Privilege** by revoking access to sensitive HR resources he no longer requires.
* **Communication Setup:** Created a new **Distribution Group** (`IT-Support-DL`) and enrolled Michael as a member. This facilitates departmental email communication without granting unnecessary administrative security rights.
* **Verification:** Validated that both IT team members are consolidated within the same security group and distribution list for streamlined management.

#### **📸 Technical Evidence:**

**1. Directory Migration: User Object in the HR OU (Pre-Move)**
![OU Pre-Move](images/ticket4a.png)
*Initial state showing the user object located within the HR Organizational Unit.*

**2. OU Migration: User Object Successfully Migrated to the IT OU**
![OU Post-Move](images/ticket4b.png)
*Final state showing the user object relocated to the IT OU alongside the IT team.*

**3. RBAC Update: Granting Access to the `IT-Group`**
![Group Membership Update](images/ticket4c.png)
*Updating the security group members list to include the transferred user.*

**4. Communication Infrastructure: Enrolling Members in the `IT-Support-DL`**
![Distribution Group Membership](images/ticket4d.png)
*Confirming the user is added to the departmental distribution list for internal communications.*

---
