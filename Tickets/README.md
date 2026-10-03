# Service Desk Tickets

Collection of incident investigations and resolutions.
``
# Active Directory Account Lockout Troubleshooting Lab

A hands-on **Windows Server / Active Directory home lab** project focused on investigating and resolving recurring user account lockouts.

This project simulates realistic IT Service Desk and Desktop Support incidents where users are unable to sign in or experience repeated Active Directory account lockouts after changing their passwords.

The lab demonstrates a structured troubleshooting approach, progressing from basic user error and account administration to identifying **cached credentials on mobile devices and Windows Scheduled Tasks**.

---

## 🎯 Project Overview

The lab consists of three simulated support tickets involving Active Directory authentication and account lockouts.

Each scenario required a different level of investigation:

| Ticket  | Scenario                                       | Root Cause                                                 | Key Skill                                              |
| ------- | ---------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------ |
| **001** | User unable to sign in after password change   | User entered an old password and triggered account lockout | AD account administration                              |
| **002** | Account repeatedly locking every 10–15 minutes | Mobile device retained old credentials                     | Authentication troubleshooting & fault isolation       |
| **003** | Account repeatedly locking every 30 minutes    | Scheduled Task stored an old password                      | Advanced Windows troubleshooting & root-cause analysis |

The objective was not simply to unlock the accounts, but to **identify why the lockouts were occurring and prevent them from recurring**.

---

# 🖥️ Lab Environment

The project was designed to simulate a small business Windows environment.

### Core Technologies

* Windows Server
* Active Directory Domain Services (AD DS)
* Windows client workstation
* Microsoft 365 applications
* Windows Task Scheduler
* Windows Credential Manager
* Network authentication
* Domain user accounts
* Account lockout policies

### Concepts Practised

* Active Directory user administration
* Authentication troubleshooting
* Account lockout investigation
* Cached credentials
* Credential persistence
* Scheduled Tasks
* Controlled fault isolation
* Root-cause analysis
* Incident documentation

---

# 🎫 Ticket 001 — User Unable to Sign In After Password Change

### User

**Sarah Mitchell (`smitchell`)**

### Issue

The user was unable to sign in to her domain-joined workstation after recently changing her password.

### Initial Troubleshooting

Before making any changes to the account, I worked through common authentication checks:

* Verified Caps Lock status
* Checked network connectivity
* Confirmed the correct sign-in method was being used
* Confirmed the exact error message
* Asked whether the password had recently been changed

The user confirmed that she had changed her password the previous afternoon and had successfully signed in afterwards.

Further investigation revealed that she had attempted to sign in using her **previous password**.

### Root Cause

The user had forgotten that her password had changed and entered the previous password multiple times.

This triggered the configured **Active Directory account lockout policy**.

### Resolution

1. Verified the user's identity.
2. Confirmed the account was enabled.
3. Unlocked the `smitchell` account in Active Directory.
4. Instructed the user to authenticate using the current password.

### Verification

The user successfully signed in using the new password.

The account remained active with no further lockout.

### Skills Demonstrated

* Active Directory user administration
* Account lockout policy awareness
* Identity verification
* User-focused troubleshooting
* Authentication troubleshooting
* Incident documentation

---

# 🎫 Ticket 002 — Repeated Account Lockout Caused by Saved Credentials

### User

**Sarah Mitchell (`smitchell`)**

### Issue

The user could successfully sign in to her office workstation with her new password, but her account continued to become locked approximately every **10–15 minutes**.

### Investigation

The user's password had recently been changed.

The office workstation accepted the new password, confirming that the current domain credentials were valid.

I then investigated other devices and services that could potentially be authenticating with the old credentials.

The user confirmed she was using:

* Outlook on her mobile phone
* Teams on her mobile phone
* A home laptop
* OneDrive
* Microsoft Edge signed in with her work account
* Office printer scan-to-email services

### Fault Isolation

Rather than changing multiple systems simultaneously, I used a controlled isolation test.

The user placed her mobile phone into **airplane mode** while the home laptop remained connected.

I then monitored the account for approximately 20 minutes.

The account remained unlocked during this period.

This strongly indicated that the mobile device was the source of the repeated authentication attempts.

### Root Cause

Work applications on the mobile phone had retained the user's **old password**.

Applications such as Outlook, Teams, Edge, or other signed-in services were repeatedly attempting authentication using the outdated credentials.

These failed authentication attempts caused Active Directory to repeatedly lock the account.

### Resolution

1. Placed the mobile device offline.
2. Unlocked the Active Directory account.
3. Signed the user out of work applications on the mobile device.
4. Updated the credentials.
5. Reconnected the device.
6. Signed back into the required applications using the new password.

### Verification

The account remained unlocked for approximately 20 minutes after reconnecting the phone.

The user was able to continue working from both the office workstation and mobile device without further lockouts.

### Skills Demonstrated

* Active Directory account administration
* Account lockout troubleshooting
* Cached credential awareness
* Mobile-device troubleshooting
* Microsoft 365 application support
* Controlled fault isolation
* Authentication troubleshooting
* Incident documentation

---

# 🎫 Ticket 003 — Repeated Account Lockout Caused by Scheduled Task

### User

**David Carter (`dcarter`)**

### Issue

The user's Active Directory account continued to become locked throughout the day despite successfully:

* Signing in with the new password
* Updating Outlook
* Updating Teams
* Updating OneDrive
* Updating mobile devices
* Updating the home laptop

The account appeared to lock approximately **every 30 minutes**.

### Initial Assessment

I confirmed:

* The account was enabled
* The account was not expired
* The user could successfully authenticate with the current password
* No additional password-reset requests had been submitted

Since the user could authenticate successfully, I suspected another device, application, service, or automated process was attempting to use outdated credentials.

### Investigation

I reviewed common locations where Windows credentials can be stored or used, including:

* Windows Credential Manager
* Mapped network drives
* Windows Services
* Scheduled Tasks
* Remote Desktop saved credentials

### Finding

During the investigation of **Windows Task Scheduler**, I discovered a task configured to execute every **30 minutes** using the user's domain account.

The task had originally been created to automate a report export to a departmental file share.

The task still contained the user's **old password**.

Every time the task executed, it attempted authentication against Active Directory using the outdated credentials.

### Root Cause

A Windows Scheduled Task was configured to run using cached domain credentials.

After the user changed their password, the Scheduled Task continued attempting authentication with the old password.

The recurring failed authentication attempts eventually triggered the Active Directory account lockout threshold.

### Resolution

1. Unlocked the user's Active Directory account.
2. Opened Windows Task Scheduler.
3. Identified the affected scheduled task.
4. Updated the stored credentials.
5. Manually executed the task.
6. Confirmed successful authentication.
7. Monitored the account for one hour.

### Verification

The scheduled task completed successfully using the updated credentials.

No further failed authentication events were generated.

The user's account remained unlocked throughout the monitoring period and normal operations resumed.

### Skills Demonstrated

* Active Directory administration
* Advanced account lockout troubleshooting
* Windows Task Scheduler administration
* Cached credential investigation
* Root-cause analysis
* Authentication troubleshooting
* Windows endpoint administration
* Incident documentation
* User support and communication

---

# 🔎 Troubleshooting Methodology

A key objective of this lab was to demonstrate that **unlocking an account is not necessarily the resolution**.

When an account repeatedly locks, the underlying source of the failed authentication attempts must be identified.

My troubleshooting approach followed this progression:

```text
User reports authentication problem
             │
             ▼
      Verify user identity
             │
             ▼
   Check AD account status
             │
             ▼
   Confirm current password
             │
             ▼
   Is the account locking again?
        /            \
      No              Yes
      │                │
   Resolved            ▼
              Identify authentication sources
                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼
       Devices       Apps/Services   Automation
          │             │             │
          └─────────────┼─────────────┘
                        ▼
                 Isolate the source
                        │
                        ▼
                 Correct credentials
                        │
                        ▼
                    Monitor
                        │
                        ▼
                  Verify resolution
```

This approach helped distinguish between:

**User error → cached credentials → automated authentication**

rather than treating every account lockout as the same problem.

---

# 🧠 Key Lessons Learned

### 1. Unlocking an account does not fix the root cause

An account may immediately lock again if another device, application, service, or scheduled process continues attempting authentication with an outdated password.

### 2. Password changes can have wider consequences

Changing an Active Directory password can affect multiple authentication sources, including:

* Mobile applications
* Microsoft 365 applications
* Mapped drives
* Services
* Scheduled Tasks
* Saved credentials

### 3. Controlled isolation is powerful

The second ticket demonstrated how temporarily disconnecting one device can help determine whether it is responsible for repeated authentication failures.

### 4. Automated processes can be easy to overlook

The third ticket demonstrated that a user's workstation can contain automated processes that continue running without the user's interaction.

### 5. Good documentation matters

Each ticket records:

**Issue → Investigation → Findings → Root Cause → Resolution → Verification**

This mirrors the structured documentation expected in an IT support environment.

---

# 🛠️ Technologies & Tools

`Windows Server` `Active Directory` `AD DS` `Windows 10/11` `Microsoft 365` `Outlook` `Teams` `OneDrive` `Microsoft Edge` `Task Scheduler` `Credential Manager`

---

# 📚 Skills Demonstrated

* Active Directory administration
* User account management
* Account unlocks
* Password troubleshooting
* Account lockout investigation
* Authentication troubleshooting
* Windows endpoint support
* Microsoft 365 support
* Credential management
* Scheduled Task administration
* Fault isolation
* Root-cause analysis
* Incident management
* Technical documentation
* User communication

---

# 📸 Lab Evidence

Screenshots and supporting evidence can be added below to demonstrate the practical implementation of the lab.

### Active Directory

```text
/screenshots/active-directory-account.png
```

### Account Lockout Investigation

```text
/screenshots/account-lockout.png
```

### Windows Task Scheduler

```text
/screenshots/scheduled-task.png
```

### Successful Resolution

```text
/screenshots/resolution-verification.png
```

> **Note:** Screenshots should be sanitised before publishing. Do not expose real passwords, personal information, domain names, IP addresses, email addresses, or other sensitive information.

---

# 🚀 What This Project Demonstrates

This lab demonstrates a practical approach to **IT support and Windows infrastructure troubleshooting** rather than simply performing administrative actions.

The three incidents show increasing troubleshooting complexity:

**Ticket 001**
Basic authentication troubleshooting and Active Directory account administration.

↓

**Ticket 002**
Multi-device investigation and controlled fault isolation.

↓

**Ticket 003**
Advanced endpoint investigation involving Windows Task Scheduler and cached domain credentials.

The progression demonstrates the ability to move from **symptom → investigation → isolation → root cause → resolution → verification**.

---

## 📌 Project Summary

**Project Type:** Home Lab / IT Support Simulation
**Focus:** Active Directory & Windows Authentication
**Environment:** Windows / Active Directory
**Tickets:** 3
**Primary Skills:** AD Administration, Account Lockout Troubleshooting, Windows Endpoint Support, Root-Cause Analysis

---

### ⭐ Key Takeaway

> **Don't just unlock the account — find out what is locking it.**

This project reinforced the importance of systematic troubleshooting when dealing with Active Directory authentication issues and demonstrated how seemingly simple account lockouts can originate from users, devices, applications, or automated Windows processes.
