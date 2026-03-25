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



