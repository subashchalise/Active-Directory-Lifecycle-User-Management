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

### 🎯 Core Skills Demonstrated
* **Directory Services:** OU Design, Object Nesting, and Attribute Management.
* **Account Security:** Password Resets, Account Unlocks, and Force-Password-Change policies.
* **Access Control:** Security Group management and membership verification.
* **Troubleshooting:** Resolving authentication issues and Access Token refresh latencies.

---

## 📂 Active Directory Design (OU Structure)
I avoided the default "Users" container to implement a professional, scalable hierarchy under the **FCTS** root OU:

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



---



### 🟢 Ticket 1: New Hire Provisioning & Security Onboarding
> Scenario: A new hire, Sarah Jenkins, requires an account in the IT department. The goal is to provision access following the company's security and OU hierarchy.

#### **Technical Actions:**
* **Provisioning:** Created the user object `sjenkins` within the **FCTS > Users > IT** Organizational Unit.
* **Access Control:** Established the `IT-Group` Security Group in the **Groups** OU and assigned Sarah as a primary member.
* **Verification:** Logged into the **Windows 11 Client** workstation as `sjenkins`. Executed the `whoami /groups` command to verify that the Kerberos Security Token was correctly issued with the new group SID.

#### **📸 Technical Evidence:**

**1. ADUC Provisioning: Creating the `sjenkins` User Object** ![User Creation](images/ticket2a.png)

**2. Group Assignment: Adding Sarah to the `IT-Group` Security Group** ![Group Member](images/ticket2c.png)

**3. Client Verification: `whoami /groups` Command Execution** ![whoami Command](images/ticket2d.png)

---

## 🎫 Ticket 2: Identity Recovery & Security Reset

**Scenario:** Michael Chen (**mchen**) from the **HR Department** reported a total loss of workstation access. After several incorrect login attempts, his account was automatically locked by the domain’s security policy to prevent unauthorized access.

---

### 🔍 Diagnostic: Identifying the Lockout
To confirm the issue, I accessed the **Active Directory Users and Computers (ADUC)** console and performed the following checks:

* **Account Status:** Navigated to the **Account** tab in Michael’s properties. A system message confirmed: *"This account is currently locked out."*
* **Deep Dive (Attribute Editor):** Enabled **Advanced Features** to inspect the `badPwdCount` attribute. This confirmed that login attempts had exceeded the domain's threshold, triggering the security lockout.

---

### 🛠️ Technical Implementation
I performed a secure administrative reset to restore access while maintaining credential privacy:

1.  **Account Unlock:** Used the **Reset Password** tool in ADUC to clear the `ADS_UF_LOCKOUT` flag.
2.  **Security Flags:** Enabled the following mandatory flags:
    * **Unlock the user's account:** To restore immediate access.
    * **User must change password at next logon:** To ensure "Administrative Zero-Knowledge" and uphold the principle of non-repudiation.

---

### ✅ Verification & Recovery
I confirmed the resolution on the **Windows 11 Client** workstation:

* **Authentication Test:** Successfully logged in using a temporary password.
* **Password Challenge:** The system immediately prompted the user to create a new, private password.
* **Result:** Access was fully restored, and the account status in Active Directory returned to "Normal."

---

### 📸 Technical Evidence
| Evidence Type | Description |
| :--- | :--- |
| **1. Identifying the Lockout** | ![ADUC Account Tab Lockout Message](link-to-your-image-here) |
| **2. Admin Reset & Unlock** | ![Password Reset Security Flags](link-to-your-image-here) |
| **3. Client-Side Recovery** | ![Windows 11 Password Change Confirmation](link-to-your-image-here) |

## 🎫 Ticket 3: Security-Focused User Offboarding

**Scenario:** An employee, **James Wilson (jwilson)**, has resigned from the Marketing department. My task was to quickly and securely disable his account to ensure he no longer has access to company files or the network.

---

### 🛠️ 1. How I Secured the Account
In the **Active Directory Users and Computers (ADUC)** console, I followed standard security procedures to lock down the account:

* **Account Disablement:** I right-clicked James’s account and selected **Disable Account**. This immediately stops any new login attempts.
* **Credential Reset:** I reset his password to a random string. This acts as a "double-lock" to make sure his old password cannot be used for any background services or cached logins.
* **Group Cleanup:** I removed his account from the **Marketing-Group**. This ensures that even if the account were re-enabled by mistake, he would no longer have access to private department folders.

---

### 📂 2. Organizing the Directory
To keep the "Active" users separate from people who have left the company, I moved the account to a secure location:

* **OU Migration:** I moved the `jwilson` user object from the **Marketing OU** into a dedicated **Disabled Users** (or "Leavers") OU.
* **Why this matters:** This keeps the Active Directory clean and prevents the system from applying new company policies (GPOs) to an account that is no longer in use.

---

### ✅ 3. Verification & Testing
I confirmed the account was fully locked by performing the following checks:

* **Visual Check:** In ADUC, James’s account icon now shows a **small black downward arrow**, confirming the account is officially disabled.
* **Login Test:** I attempted to log into a **Windows 11 workstation** using the `jwilson` credentials.
* **Result:** The computer blocked the login and displayed the error: *"Your account has been disabled. Please see your system administrator."* This proves the security steps were effective.

---

### 📸 Technical Evidence
| Evidence Type | Description |
| :--- | :--- |
| **1. Account Status** | ![ADUC Screenshot showing jwilson with Disabled Arrow](link-to-your-image-here) |
| **2. OU Migration** | ![jwilson moved to Disabled Users OU](link-to-your-image-here) |
| **3. Security Success** | ![Windows 11 "Account Disabled" error message](link-to-your-image-here) |
