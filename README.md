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
- Security Monitoring
- IT Service Management
- Enterprise Troubleshooting

The environment was designed, deployed, administered, and continuously improved through real-world support scenarios and change-management style projects.

---

# Infrastructure Overview

```text
                                            INTERNET
                                                │
                                       Home Router / WiFi
                                                │
                            ┌───────────────────┴───────────────────┐
                            │                                       │
                         Host Laptop                         HP DJ2700 Printer
                       Windows 11 Pro
                            │
                Hyper-V  External Switch +
                   Internal LAB Switch
                            │
    ┌───────────────────────┼───────────────────────┐
    │                       │                       │
  DC01                    PC01                   PC02
Windows Server        Windows 11             Linux Ubuntu 26.04
2022 Enterprise       Workstation            Workstation  
   │                  (All Users)              (IT-Team Only)
   ▼
  Active Directory DS
  Domain Name Service
  Group Policy Objects
  File Services / Drive Mapping
  Print Services / Auto-Deployment
  Software Repository/Deployment
  Centralize Event Log Collector
        │                       │                      │
        └────────────── solotech.local ────────────────┘
                Centralized Authentication
                            │
                            ▼
             Auto Deployment and Self Updating Printer Allocation
                            │
                            ▼
                    HP DJ2700 Series
                     Network Printer
                      ├── IT-PRINTER
                      ├── HR-PRINTER
                      ├── FINANCE-PRINTER
                      ├── SALES-PRINTER
                      └── CUST-SERV-PRINTER

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
IT-Team
HR-Team
Finance-Team
Sales-Team
CustomerService-Team
Finance-Team-Payroll
```

---
```
# Infrastructure Components

## DC01 – Windows Server 2022

Configured as the central infrastructure server providing:

- Active Directory Domain Services (AD DS)
- DNS
- Group Policy Management
- File Services
- Print Services
- Software Deployment Repository
- Windows Event Collector

## PC01 – Windows 11 Enterprise

Configured as the primary domain-joined workstation for:

- User authentication testing
- Group Policy validation
- Software deployment testing
- Printer deployment testing
- Security monitoring
- Event forwarding

---

# Core Technologies Implemented

## Active Directory & Identity Management

- User account provisioning
- Password management
- Account lockout policies
- Department-based Organizational Units
- Security Group administration
- Role-Based Access Control (RBAC)
- Least Privilege implementation

## Group Policy Management

Successfully implemented:

- Password Complexity Policies
- Account Lockout Policies
- Department Drive Mapping
- Department Printer Deployment
- HR Wallpaper Deployment
- Customer Service Restrictions
- Software Deployment
- Security Auditing Policies

## File Services

Departmental file shares secured through:

- Active Directory Security Groups
- Share Permissions
- NTFS Permissions
- Least Privilege Design

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

## Automated Departmental Printer Deployment

### Business Objective

Deliver a fully automated printer deployment solution that:

- Assigns printers based on department membership
- Automatically sets default printers
- Removes obsolete printers during department transfers
- Requires zero manual intervention after onboarding

### Challenges Encountered

Initial deployment required users to manually browse:

```text
\\DC01
```

to install printers.

Windows also repeatedly reverted default printers to:

```text
Microsoft Print to PDF
```

during policy refresh cycles.

### Solution

Implemented:

- Group Policy Preferences
- Item-Level Targeting
- Active Directory Security Groups
- Registry-based printer management
- Automated default printer enforcement

### Outcome

Users automatically receive:

- Department-specific printer access
- Correct default printer assignment
- Automatic printer replacement if department membership changes

Example:

```text
Finance-Team
        ↓
FINANCE-PRINTER
        ↓
Default Printer Assigned
```

If the user moves to HR:

```text
FINANCE-PRINTER removed
HR-PRINTER deployed
HR-PRINTER becomes default
```

### Skills Demonstrated

- Print Management
- Group Policy Preferences
- Printer Automation
- Registry Configuration
- RBAC
- User Lifecycle Management

---

## New Employee Onboarding

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
         └── dokafor
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

## Security Monitoring & Event Forwarding

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

# Linux Integration with Active Directory

## Overview

To extend the enterprise environment beyond Windows systems, a Linux workstation (PC02) was integrated into the Active Directory domain.

This project demonstrates centralized identity management across multiple operating systems using Active Directory as the single authentication source.

---

## Environment

| Device | Operating System | Role |
|----------|----------|----------|
| DC01 | Windows Server 2022 | Domain Controller |
| PC01 | Windows 11 Enterprise | Domain Workstation |
| PC02 | Linux Desktop | Linux Domain Workstation |

Domain:

```text
solotech.local
```

---

## Project Objectives

- Join Linux to Active Directory
- Centralize authentication through AD
- Restrict workstation access using AD security groups
- Eliminate local account dependency
- Implement role-based access control

---

## Linux Domain Join Process

### 1. Configure DNS

Linux was configured to use the domain controller as its primary DNS server.

```text
DNS Server:
192.168.10.10
```

This enabled Linux to locate:

- Active Directory
- Kerberos Services
- LDAP Services

---

### 2. Install Required Components

Installed:

```text
realmd
sssd
adcli
krb5
samba-common
oddjob
```

These components provide:

- Kerberos authentication
- Active Directory integration
- User account resolution
- Domain enrollment

---

### 3. Discover Domain

Verified Linux could locate:

```text
solotech.local
```

and communicate with the domain controller.

---

### 4. Join Active Directory

Joined Linux workstation to:

```text
solotech.local
```

using domain administrator credentials.

Result:

```text
PC02
```

was automatically created within:

```text
Active Directory Users and Computers
→ Computers
```

---

### 5. Configure Centralized Authentication

Enabled:

- Active Directory login
- Domain user authentication
- User directory creation
- Identity resolution

Users can now authenticate using:

```text
SOLTECH\username
```

equivalent Linux login formats.

---

### 6. Restrict Access Using Security Groups

Login restrictions were configured so that only:

```text
IT-Team
```

members can access the Linux workstation.

Denied Groups:

```text
HR-Team

Finance-Team

Sales-Team

CustomerService-Team
```

---

## Access Control Model

```text
IT-Team
    ↓
Access Granted

All Other Department Groups
    ↓
Access Denied
```

This provides:

- Least Privilege
- RBAC
- Centralized Identity Governance

---

## Enhanced Session Investigation

A parallel investigation was conducted into improving Linux and Hyper-V integration.

### Objective

Enable:

- Clipboard Redirection
- Host to Guest Copy & Paste
- Improved GUI Experience

### Technologies Evaluated

- xRDP
- Xorg
- Xvnc

### Outcome

Successfully established enhanced functionality through:

```text
xRDP
+
Xvnc
```

before simplifying the final deployment configuration.

This provided valuable experience in:

- Linux Remote Desktop Services
- Virtualization Integration
- Linux Desktop Administration

---

## Validation

Successfully verified:

| Test | Result |
|---------|---------|
| Domain Discovery | ✅ |
| Domain Join | ✅ |
| AD Computer Object Created | ✅ |
| Domain Authentication | ✅ |
| IT-Team Login | ✅ |
| Non-IT Login Blocked | ✅ |

---

## Skills Demonstrated

### Active Directory

- Computer Object Management
- Domain Join Procedures
- Security Group Administration
- Identity Management

### Linux Administration

- Linux Domain Integration
- Kerberos Authentication
- SSSD Configuration
- Realmd Administration
- Linux Access Control

### Security

- RBAC
- Least Privilege
- Directory-Based Authentication
- Cross-Platform Identity Management

### Infrastructure

- Hyper-V
- Linux Virtualization
- DNS Integration
- Enterprise Authentication Services

---

## Business Outcome

The SoloTech lab evolved from a Windows-only Active Directory environment into a hybrid Windows/Linux infrastructure with centralized authentication, access control, and identity management.

This implementation reflects the type of environment commonly found in enterprise organizations where Linux and Windows systems coexist and rely on a shared identity platform.









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
