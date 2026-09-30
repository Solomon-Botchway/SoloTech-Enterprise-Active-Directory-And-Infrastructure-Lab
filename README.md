# SoloTech-Innovations---Enterprise-Active-Directory-Lab
Enterprise Active Directory Lab built using Windows Server 2022, Hyper-V, Group Policy and Windows 11.

## Infrastructure Overview

```
                            INTERNET
                                │
                     Home Router / WiFi
                                │
            ┌───────────────────┴───────────────────┐
            │                                       │
     Host Laptop                            HP DJ2700 Series Printer
   Windows 11 Pro
            │
            │
     Hyper-V External Switch
            │
 ┌──────────┴──────────┐
 │                     │
 ▼                     ▼

DC01                 PC01
Windows Server       Windows 11 Enterprise
2022

Active Directory
DNS
Print Server
File Server
Software Deployment

Domain:
solotech.local
```

## Domain Structure

solotech.local

└── SoloTech
    ├── IT
    ├── HR
    ├── Finance
    ├── Sales
    └── Customer Service
``
## Security Groups

IT-Team
HR-Team
Finance-Team
Sales-Team
CustomerService-Team

``
## Implemented Services

- Active Directory Domain Services
- DNS
- Shared Folders
- NTFS Permissions
- Group Policy
- Drive Mapping
- Printer Deployment
- Software Deployment
- Account Lockout Policies
- Password Policies

## Troubleshooting Experience

- Windows 11 TPM & Secure Boot Requirements
- Domain Trust Relationship Repair
- GPO Security Filtering Issues
- Drive Mapping Errors
- Printer Permission Issues
- Software Deployment Troubleshooting
- Password Lockout Investigations



2
# SoloTech Active Directory Lab

```mermaid
graph TD

HOST[Host Windows 11 Pro Laptop]

HOST --> HV[Hyper-V]

HV --> DC01[DC01 Windows Server 2022]
HV --> PC01[PC01 Windows 11 Enterprise]

DC01 --> AD[Active Directory]
DC01 --> DNS[DNS]
DC01 --> FS[File Server]
DC01 --> PS[Print Server]
DC01 --> SW[Software Repository]

PC01 --> DOMAIN[solotech.local]

FS --> IT[IT_Data]
FS --> HR[HR_Data]
FS --> FIN[Finance_Data]
FS --> SALES[Sales_Data]
FS --> CS[CustomerService_Data]
```



# Security Monitoring & Event Forwarding

## Overview

As part of the SoloTech Innovations Active Directory Lab, centralized Windows event monitoring was implemented to improve visibility, auditing, and security monitoring across domain-joined devices.

## Environment

| Device | Operating System | Role |
|----------|----------|----------|
| DC01 | Windows Server 2022 | Domain Controller / Event Collector |
| PC01 | Windows 11 Enterprise | Event Source |

Domain:

```text
solotech.local
```

## Implemented Features

### Windows Event Forwarding (WEF)

Successfully configured:

```text
PC01
   ↓
Windows Event Forwarding
   ↓
DC01 Event Collector
```

Events generated on PC01 are now collected and monitored centrally from DC01.

### Security Auditing

Enabled auditing through Group Policy for:

- Successful Logon Events
- Failed Logon Events
- Account Lockout Events
- Account Management Events
- User Authentication Events
- Security Log Monitoring

### Firewall Configuration

Configured Windows Firewall rules to allow:

```text
Windows Remote Event Log Management

Windows Event Collector Service

Windows Remote Management (WinRM)
```

ensuring successful communication between PC01 and DC01.

## Validation Tests Performed

### Logon Monitoring

Generated multiple successful and failed logon attempts on PC01 and confirmed events were received on DC01.

### Account Lockout Monitoring

Triggered account lockouts using the SoloTech Account Lockout Policy and verified collection of the resulting security events.

### Event Subscription Verification

Verified event subscriptions were:

```text
Active

Connected

Receiving Events
```

through Event Viewer on DC01.

## Key Event IDs Monitored

| Event ID | Description |
|-----------|-------------|
| 4624 | Successful Logon |
| 4625 | Failed Logon |
| 4720 | User Account Created |
| 4722 | Account Enabled |
| 4725 | Account Disabled |
| 4728 | Added to Security Group |
| 4729 | Removed from Security Group |
| 4740 | Account Locked Out |
| 1102 | Audit Log Cleared |

## Skills Demonstrated

- Windows Event Forwarding (WEF)
- Event Viewer Administration
- Security Auditing
- Active Directory Monitoring
- Windows Firewall Configuration
- Group Policy Administration
- Security Log Analysis
- Troubleshooting Event Collection
- Security Operations Fundamentals

## Cyber Security Relevance

This implementation introduces concepts commonly used in Security Operations Centres (SOC) and Security Information and Event Management (SIEM) platforms.

The architecture simulates the early stages of:

```text
Endpoint Monitoring
        ↓
Windows Event Forwarding
        ↓
Central Log Collection
        ↓
Threat Detection
        ↓
Security Investigation
```

# IT Service Management & User Onboarding

## Ticket Reference

INC-003 – New Employee Onboarding

## Business Request

The Finance Manager submitted a request for a new joiner:

```text
Name: Daniel Dokafor
Role: Finance Assistant
Department: Finance
```

Required access:

- Finance shared drive
- Finance department printer
- Standard domain access

Restrictions:

- No Payroll access
- No access to other departmental resources
- No access to other departmental printers

## Pre-Provisioning Verification

Before account creation, approval was verified from:

- Hiring Manager
- Human Resources

This follows standard onboarding and identity management procedures.

## Active Directory Provisioning

Account created in:

```text
solotech.local
└── SoloTech
    └── Finance
         └── dokafor
```

User configuration:

- Temporary password assigned
- Password change required at first logon
- No account expiration date
- Department information completed
- Job title completed
- Manager assigned
- Office location recorded

## Security Group Assignment

Assigned Groups:

```text
Domain Users

Finance-Team
```

Access granted through Finance-Team:

- FINANCE_Drive
- FINANCE-PRINTER
- General Finance documents

## Least Privilege Access Implementation

A sensitive Payroll folder was identified during the onboarding process.

A dedicated security group was created:

```text
Finance-Team-Payroll
```

NTFS permissions were modified so that:

```text
Finance-Team-Payroll
```

retains Payroll access.

```text
Finance-Team
```

was removed from Payroll permissions.

Daniel was intentionally excluded from:

```text
Finance-Team-Payroll
```

as part of least-privilege access control.

## Validation Testing

Testing was performed using the newly created account.

| Resource | Result |
|-----------|----------|
| Finance Shared Drive | ✅ Accessible |
| Finance Documents | ✅ Read/Write Access |
| Payroll Folder | ❌ Access Denied |
| Finance Printer | ✅ Accessible |
| Other Department Drives | ❌ Not Accessible |
| Other Department Printers | ❌ Not Accessible |

## Resolution

User account successfully provisioned according to business requirements.

Access was assigned using:

- Active Directory Security Groups
- NTFS Permissions
- Role Based Access Control (RBAC)
- Least Privilege Principles

## Skills Demonstrated

- New Employee Onboarding
- Active Directory Administration
- User Provisioning
- Identity & Access Management
- Security Group Administration
- NTFS Permissions Management
- Role Based Access Control (RBAC)
- Least Privilege Security
- Printer Deployment
- Shared Drive Administration
- Access Verification
- Service Desk Documentation

## Cyber Security Relevance

This onboarding process implemented:

```text
Identity Governance

Least Privilege Access

Separation of Duties

Access Control Validation

Role-Based Security
```

These are foundational security principles used in enterprise environments and Security Operations teams.

This project serves as a foundation for future work with:

- Microsoft Sentinel
- Splunk
- Elastic SIEM
- Security Operations (SOC)
- Incident Response
- Threat Hunting
