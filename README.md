# SoloTech Innovations Enterprise Active Directory Lab

## Overview

SoloTech Innovations is a hands-on enterprise infrastructure lab designed to simulate a real-world Microsoft environment using Hyper-V, Windows Server 2022, and Windows 11 Enterprise.

The project focuses on building practical experience in:
- Active Directory Domain Services (AD DS)
- DNS Administration
- Group Policy Management
- Identity & Access Management (IAM)
- File and Print Services
- Software Deployment
- Windows Event Forwarding (WEF)
- Scheduled Task Automation
- Security Monitoring
- IT Service Management
- Enterprise Troubleshooting

The environment was designed, deployed, administered, and continuously improved through real-world support scenarios and change-management style projects.

---
<img width="946" height="506" alt="Screenshot 2026-10-18 233802" src="https://github.com/user-attachments/assets/8f4d318e-520a-4f29-8279-33189b405a5d" />

# Infrastructure Overview

```text
                                            INTERNET
                                                │
                                       Home Router / WiFi
                                                │
                            ┌───────────────────┴───────────────────┐
                            │                                       │
                          Host Laptop                         HP DJ2700 Series Printer
                     HP Elitebook Windows 11 Pro                  (On-Site)
                                │
                   Hyper-V Internal LAB Switch 
                      + External Switch 
                                │
    ┌───────────────────────────┼───────────────────────┼────────────────────────────┐
    │                           │                       │                            │
  DC01                        FS01                     PC01/03                         PC02
Windows Server              Windows Server          Windows 11                 Linux Ubuntu 26.04
2022 Enterprise             2022 Enterprise          Workstation                   Workstation
(Administrators)            (Domain Admins)           (All Users)                 (IT-Team Only)
       │                         │
       ▼                         ▼
  Active Directory           Department Shares/Drives Mapping
  DNS                        User Home Folders
  DHCP                       Archive Storage
  GPO                        Software Repository
  Printer Deployment         Wallpaper Repository
  Event Log Collector        Screensaver Repository

        │                          │                              │                              │
        └──────────────────────────────── solotech.local ────────────────────────────────────────┘
                                    Centralized Authentication

```
## Active Directory Structure

```
solotech.local

└── SoloTech
    ├── IT
    ├── HR
    ├── Finance
    ├── Sales
    └── Customer Service
```

### Security Groups
```
IT-Team
HR-Team
Finance-Team
Sales-Team
CustomerService-Team
Finance-Team-Payroll
Automation-Robotics
```
---
# Infrastructure Components

## DC01 & FS01– Windows Server 2022

Configured as the central infrastructure server providing:

- Active Directory Domain Services (AD DS)
- DNS
- DHCP
- Group Policy Management
- File Services
- Storage Services
- Print Services
- Software Deployment Repository
- Windows Event Collector

## PC01 & PC03 – Windows 11 Enterprise

Configured as the primary domain-joined workstation for:

- User authentication testing
- Group Policy validation
- Software deployment testing
- Printer deployment testing
- Security monitoring
- Event forwarding

## PC02 – LINUX UBUNTU 26.04

 Domain-joined workstation for:

- RBAC authentication testing
- Multi-Platform testing

---

# Core Technologies Implemented

## 1. Active Directory & Identity Management

- User account provisioning
- Password management
- Account lockout policies
- Department-based Organizational Units
- Security Group administration
- Role-Based Access Control (RBAC)
- Least Privilege implementation

## 2. DNS (Domain Name System)

Implemented and managed Microsoft DNS as part of the Active Directory infrastructure on DC01.

### Key Responsibilities

- Installed and configured the DNS Server role
- Integrated DNS with Active Directory Domain Services (AD DS)
- Managed the internal DNS zone:

```text
solotech.local
```

- Verified domain name resolution between servers and clients
- Supported Active Directory authentication and Group Policy processing
- Configured DNS records for domain-joined devices
- Performed DNS troubleshooting and record management
- Implemented DNS forwarding to external ISP DNS servers for internet name resolution

### Enterprise Features Demonstrated

- Active Directory Integrated DNS
- Forward and Reverse Lookup Zones
- Dynamic DNS Registration
- DNS Forwarders
- Name Resolution Troubleshooting
- Client and Server DNS Configuration

### Example Environment

```text
DC01
192.168.10.10
DNS Server

FS01
192.168.10.20

PC01
192.168.10.110

PC02
Linux Workstation

PC03
192.168.10.130
```

All systems use:

```text
192.168.10.10
```

as their primary DNS server, ensuring consistent domain authentication, Group Policy application, drive mapping, file server access, and resource discovery throughout the environment.

### Skills Demonstrated

- Microsoft DNS Administration
- Active Directory Integration
- Name Resolution
- DNS Forwarders
- DNS Troubleshooting
- Enterprise Network Services
---

## 3. DHCP Implementation

A centralized DHCP service was deployed on DC01 to automate IP address management.

### Scope Configuration

```text
Scope:
192.168.10.100 - 192.168.10.200

Domain:
solotech.local

DNS:
192.168.10.10

Router:
192.168.0.1
```

### Reservations

```text
PC01
192.168.10.110

PC03
192.168.10.130

FS01
192.168.10.20
```

### Benefits

- Centralized endpoint management
- Consistent IP allocation
- Automatic DNS registration
- Reduced manual configuration
- Enterprise-style network administration

### Skills Demonstrated

- Windows DHCP Administration
- Scope Management
- DHCP Reservations
- DNS Integration
- Network Infrastructure Management
---

## 4. Group Policy Management

Successfully implemented:

- Password Complexity Policies
- Account Lockout Policies
- Department Drive Mapping
- Department Printer Deployment
- HR Wallpaper Deployment
- Customer Service Restrictions
- Software Deployment
- Security Auditing Policies
- Finance only Software
- Finance Desktop Policies

## 5. File Services

Departmental file shares secured through:

- Active Directory Security Groups
- Share Permissions
- NTFS Permissions
- Least Privilege Design

```
Examples:

IT_Drive

HR_Drive

FINANCE_Drive

SALES_Drive

CUST-SERV_Drive
```

A separate Payroll security model was implemented using:

```text
Finance-Team-Payroll
```

to protect sensitive financial records.

---
# Featured Projects

## 1. New Employee Onboarding

### Ticket Reference

INC-003 – Finance Assistant Onboarding

### Business Requirement

Provision a new Finance employee with:

- Finance drive access
- Finance printer access
- Standard domain access

Restrictions:

- No Payroll access
- No access to other departments
- No access to other departmental printers

### Provisioning Activities

Created and configured:

```text
solotech.local
└── SoloTech
    └── Finance
         └── david.carter
```

Configured:

- Temporary password
- Mandatory password change at first logon
- Department information
- Job title
- Manager
- Office location

Assigned Groups:

```text
Domain Users
Finance-Team
```

### Security Implementation

Created:

```text
Finance-Team-Payroll
```

to isolate Payroll data.

The new employee was deliberately excluded from Payroll access using:

- NTFS permissions
- Security groups
- RBAC principles

### Validation Results

| Resource | Result |
|-----------|---------|
| Finance Drive | ✅ Accessible |
| Finance Documents | ✅ Read & Write |
| Payroll Folder | ❌ Access Denied |
| Finance Printer | ✅ Accessible |
| Other Departments | ❌ Access Denied |

### Skills Demonstrated

- User Provisioning
- RBAC
- Active Directory Administration
- NTFS Permissions
- Least Privilege
- Service Desk Documentation

---

## 2. Security Monitoring & Event Forwarding

### Objective

Implement centralized event collection and security auditing.

### Architecture

```text
PC01
    ↓
Windows Event Forwarding (WEF)
    ↓
DC01 Event Collector
```

### Events Monitored

- Successful Logons (4624)
- Failed Logons (4625)
- Account Lockouts (4740)
- User Account Changes
- Security Group Changes

### Security Controls Implemented

- Windows Event Forwarding
- Subscription Management
- Firewall Configuration
- Security Auditing via GPO
- Centralized Event Collection

### Security Benefit

Provides visibility into:

- Authentication activity
- Account management events
- Security policy enforcement
- User behaviour monitoring

---

# Troubleshooting Experience

Throughout the project, multiple enterprise-style incidents were investigated and resolved.

### Resolved Issues

- Windows 11 TPM and Secure Boot deployment failures
- Active Directory trust relationship failures
- GPO Security Filtering issues
- Drive mapping failures
- Printer deployment failures
- Default printer assignment issues
- Account lockout investigations
- Group Policy delegation issues
- Software deployment failures
- Windows Event Forwarding configuration issues
- DNS-related authentication issues
- Hyper-V Enhanced Session behaviour

Each issue was documented using:

- Investigation
- Root Cause Analysis
- Resolution
- Validation
- Lessons Learned

---

# Technical Skills Demonstrated

## Infrastructure

- Windows Server 2022
- Windows 11 Enterprise
- Hyper-V
- DNS
- Active Directory Domain Services

## Identity & Access Management

- User Provisioning
- Security Groups
- Role-Based Access Control (RBAC)
- Least Privilege
- User Lifecycle Management

## Group Policy

- GPO Design
- Security Filtering
- Delegation
- Drive Mapping
- Printer Deployment
- Software Deployment

## Security

- Windows Event Forwarding
- Security Auditing
- Account Lockout Monitoring
- Event Analysis
- Access Control

## IT Operations

- Incident Management
- Troubleshooting
- Service Desk Documentation
- Root Cause Analysis
- Change Validation

---

# Key Outcomes

This project evolved from a simple Active Directory deployment into a miniature enterprise infrastructure environment incorporating:

✅ Identity Management

✅ Access Control

✅ Group Policy Automation

✅ File & Print Services

✅ Software Distribution

✅ Security Monitoring

✅ User Lifecycle Management

✅ IT Service Management

✅ Cyber Security Foundations

---

## Hands-On Lab: 
## Scheduled Task Authentication & Network Drive Troubleshooting

### Overview

As part of my Active Directory Helpdesk Lab, I created and tested a Scheduled Task running under a domain user account (`SOLOTECH\d.carter`) to simulate a real-world account lockout scenario caused by stale credentials.

During implementation, I encountered and resolved several issues involving Group Policy, Windows authentication, Task Scheduler security contexts, and mapped network drives. This exercise provided practical experience troubleshooting Windows administration issues commonly seen in enterprise environments.

---

### Objective

Create an automated reporting task that runs under a domain account and writes report data to a department file share.

---

### Issue 1: Scheduled Task Would Not Start

#### Symptoms

The task failed to execute and displayed:

```text
This task requires that the user account specified has
Log on as a batch job rights.
```

#### Root Cause

Although the user account could log on interactively, Task Scheduler runs under a separate **batch logon** security context requiring the **Log on as a batch job** user right.

#### Resolution

Configured the required User Rights Assignment through Group Policy and applied updates using:

```cmd
gpupdate /force
```

#### Skills Demonstrated

- Group Policy Management
- User Rights Assignment
- Windows Authentication
- Task Scheduler Administration

---

### Issue 2: Verifying Task Execution

#### Test

```cmd
whoami > C:\Temp\report.txt
```

#### Result

The Scheduled Task completed successfully and generated:

```text
solotech\d.carter
```

confirming that the task was executing under the intended domain account.

#### Skills Demonstrated

- Task Validation
- Windows Command Line
- Security Context Verification

---

### Issue 3: Scheduled Task Failed When Writing to Mapped Network Drive

#### Test

```text
F:\Notes\report.txt
```

#### Result

Task Scheduler returned:

```text
0x1
```

The same command executed successfully when run manually from PowerShell or Command Prompt.

---

### Investigation

To identify the difference, I compared the output of:

```cmd
net use
```

#### Interactive User Session

```text
F: mapped to Finance Department Drive
```

#### Scheduled Task Session

```text
There are no entries in the list.
```

---

### Root Cause

The mapped drive letter (`F:`) existed only within the user's interactive logon session.

When Task Scheduler executed under a batch logon session, mapped network drives were unavailable, causing the task to fail when attempting to access the share using the drive letter.

---

### Resolution

Replaced the mapped drive path:

```text
F:\Notes\report.txt
```

with the UNC path:

```text
\\DC01\Finance_Data\Notes\report.txt
```

The task immediately executed successfully and wrote the report to the shared 

## Future Enhancements

Planned expansion areas include:

- Microsoft 365 Administration
- Microsoft Entra ID
- Microsoft Intune
- Microsoft Sentinel
- PowerShell Automation
- Vulnerability Management
- Security Operations Centre (SOC) Workflows
- Incident Response Scenarios
