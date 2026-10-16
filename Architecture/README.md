# Architecture

This folder contains diagrams and design documents for the SoloTech environment.
Project reports and technical documents.

solotech-enterprise-lab/
│
├── README.md
│
├── Architecture/
│   ├── Enterprise-Architecture.md
│   ├── Active-Directory-Design.md
│   ├── RBAC-Model.md
│   ├── File-Server-Architecture.md
│   └── Security-Monitoring.md
│
├── Infrastructure/
│   ├── DC01-Domain-Controller.md
│   ├── FS01-File-Server.md
│   ├── PC01-Windows-Workstation.md
│   ├── PC02-Linux-Workstation.md
│   └── Network-Configuration.md
│
├── Projects/
│   │
│   ├── Active-Directory-Deployment/
│   │   └── README.md
│   │
│   ├── Group-Policy-Management/
│   │   └── README.md
│   │
│   ├── Drive-Mapping/
│   │   └── README.md
│   │
│   ├── Printer-Deployment/
│   │   └── README.md
│   │
│   ├── Software-Deployment/
│   │   └── README.md
│   │
│   ├── User-Onboarding/
│   │   └── README.md
│   │
│   ├── User-Offboarding/
│   │   └── README.md
│   │
│   ├── Linux-Domain-Join/
│   │   └── README.md
│   │
│   ├── Event-Forwarding/
│   │   └── README.md
│   │
│   └── File-Server-Migration/
│       └── README.md
│
├── Permissions/
│   ├── Share-Permissions.md
│   ├── NTFS-Permissions.md
│   ├── RBAC-Implementation.md
│   └── Payroll-Security-Model.md
│
├── Helpdesk-Tickets/
│   ├── INC-001-Account-Lockout.md
│   ├── INC-002-Password-Reset.md
│   ├── INC-003-New-User-Onboarding.md
│   ├── INC-004-Account-Disabled.md
│   ├── INC-005-Wrong-Department.md
│   ├── INC-014-Employee-Offboarding.md
│   └── Ticket-Index.md
│
├── Troubleshooting/
│   ├── GPO-Security-Filtering.md
│   ├── Trust-Relationship-Failure.md
│   ├── DNS-Registration-Issue.md
│   ├── Printer-Deployment-Issues.md
│   ├── Share-vs-NTFS-Permissions.md
│   ├── HyperV-Installation-Issues.md
│   ├── Windows11-TPM-Issue.md
│   └── Firewall-ICMP-Blocking.md
│
├── Screenshots/
│   │
│   ├── ActiveDirectory/
│   ├── GPOs/
│   ├── DriveMappings/
│   ├── Printers/
│   ├── FileServer/
│   ├── Linux/
│   ├── EventForwarding/
│   └── Helpdesk/
│
└── Documentation/
    ├── Change-Log.md
    ├── Lessons-Learned.md
    ├── Future-Roadmap.md
    └── Skills-Demonstrated.md





# SoloTech Active Directory Lab

```mermaid
graph TD

    HOST["Host Windows 11 Pro Laptop"]

    HOST --> HV["Hyper-V"]

    HV --> DC01["DC01 Windows Server 2022"]
    HV --> PC01["PC01 Windows 11 Enterprise"]

    DC01 --> AD["Active Directory"]
    DC01 --> DNS["DNS"]
    DC01 --> FS["File Server"]
    DC01 --> PS["Print Server"]
    DC01 --> SW["Software Repository"]

    PC01 --> DOMAIN["solotech.local"]

    FS --> IT["IT_Data"]
    FS --> HR["HR_Data"]
    FS --> FIN["Finance_Data"]
    FS --> SALES["Sales_Data"]
    FS --> CS["CustomerService_Data"]
```

# Technical Architecture

```mermaid
graph LR

    USERS["Domain Users"]

    USERS --> PC01["PC01<br/>Windows 11 Enterprise"]

    PC01 --> DC01["DC01<br/>Windows Server 2022"]

    DC01 --> AD["Active Directory & DNS"]
    DC01 --> GPO["Group Policy"]
    DC01 --> FILE["File & Print Services"]
    DC01 --> SEC["Security Monitoring"]
```

---

# RBAC Model

This diagram illustrates role-based access control (RBAC) using Active Directory security groups.

```mermaid
graph LR

    USER["Employee/User"]

    USER -->|Member Of| GROUP["Department Security Group"]

    GROUP -->|Access Granted| DRIVE["Department Shared Drives"]
    GROUP -->|Print Rights| PRINTER["Department Printers"]
    GROUP -->|Policy Applied| POLICY["Department GPOs"]

    POLICY -->|Configures Access| DRIVE
    POLICY -->|Deploys Settings| PRINTER
```

## Access Flow

1. User is added to a Department Security Group.
2. The Security Group grants access to shared drives.
3. The Security Group grants access to department printers.
4. Group Policies are applied based on group membership.
5. GPOs configure resources and deploy settings automatically.

---

# Printer Architecture

This diagram shows printer deployment through Active Directory security groups and Group Policy.

```mermaid
graph LR

    USER["Finance User"]

    USER -->|Member Of| GROUP["Finance-Team Security Group"]

    GROUP -->|Security Filtering| GPO["Finance Printer Deployment GPO"]

    GPO -->|Deploys Printer Queue| PRINTSERVER["PRINT-SRV01"]

    PRINTSERVER -->|Publishes| PRINTER["FINANCE-PRINTER"]

    USER -->|Receives Mapped Printer| PRINTER
```

## User Provisioning Model

```mermaid
graph LR

    NEWUSER["New Employee"]

    NEWUSER --> OU["Department OU"]

    OU --> GROUP["Security Group"]

    GROUP --> DRIVE["Shared Drive"]
    GROUP --> PRINTER["Department Printer"]
    GROUP --> POLICY["Department GPO"]
```


## Network Diagram

The following diagram shows the infrastructure used during the Scheduled Task Authentication and Network Drive Troubleshooting Lab.

```mermaid
graph TD

    HOST["Host Windows 11 Pro"]
    
    HOST --> HV["Hyper-V"]

    HV --> DC01["DC01<br/>Windows Server 2022"]
    HV --> PC01["PC01<br/>Windows 11 Enterprise"]

    DC01 --> AD["Active Directory"]
    DC01 --> DNS["DNS"]
    DC01 --> FS["File Server"]

    FS --> FIN["Finance_Data Share"]

    PC01 --> USER["d.carter"]
    
    USER --> FDRIVE["Mapped Drive F:"]

    FDRIVE --> FIN

    USER --> TASK["Scheduled Task: Reports"]

    TASK -. Interactive Session .-> FDRIVE

    TASK -. Batch Logon Session .-> UNC["\\\\DC01\\Finance_Data\\Notes"]

    UNC --> FIN
```

### Network Resource Flow

#### Interactive User Session

When `d.carter` signs into Windows:

```text
d.carter
    ↓
Desktop Session
    ↓
Mapped Drive F:
    ↓
\\DC01\Finance_Data
```

The Finance department share is available through the mapped drive and can be accessed normally through File Explorer, PowerShell, and Command Prompt.

---

#### Scheduled Task Session

When Task Scheduler launches the task:

```text
Task Scheduler
    ↓
Batch Logon Session
    ↓
No Explorer Session
    ↓
No Mapped Drives
```

The scheduled task cannot see the `F:` drive because mapped drives only exist inside the interactive desktop session.

---

#### Successful Enterprise Solution

Instead of relying on a mapped drive letter:

```text
F:\Notes\report.txt
```

the task was updated to use the UNC path:

```text
\\DC01\Finance_Data\Notes\report.txt
```

This allows the task to access the file share regardless of whether the user is logged on interactively.

---

### Authentication Flow

```mermaid
sequenceDiagram

    participant TS as Task Scheduler
    participant PC as PC01
    participant DC as DC01
    participant FS as Finance Share

    TS->>DC: Authenticate SOLOTECH\d.carter
    DC-->>TS: Kerberos/NTLM Authentication

    TS->>FS: Access \\DC01\Finance_Data\Notes
    FS-->>TS: Write report.txt

    TS->>PC: Task completes successfully
```

### Key Architecture Lessons

- Interactive sign-in sessions and Task Scheduler batch sessions are separate security contexts
