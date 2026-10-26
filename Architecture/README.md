# Architecture

This folder contains diagrams and design documents for the SoloTech environment.
Project reports and technical documents.

## Enterprise Architecture

```mermaid
graph TD

    INTERNET[Internet]

    INTERNET --> ROUTER[Router<br/>192.168.0.1]

    ROUTER --> SWITCH[Hyper-V External Switch]

    SWITCH --> DC01[DC01<br/>Windows Server 2022]

    SWITCH --> FS01[FS01<br/>Windows Server 2022]

    SWITCH --> PRINTER[HP DJ2700<br/>Network Printer]

    SWITCH --> PC01[PC01<br/>Windows 11]

    SWITCH --> PC03[PC03<br/>Windows 11]

    SWITCH --> PC02[PC02<br/>Linux]

    DC01 --> AD[Active Directory]
    DC01 --> DNS[DNS]
    DC01 --> DHCP[DHCP]
    DC01 --> GPO[Group Policy]
    DC01 --> WEF[Event Collection]

    FS01 --> FILES[Department Drives]
    FS01 --> HOMES[User Home Folders]
    FS01 --> ARCHIVE[Archives]
    FS01 --> SOFTWARE[Software Repository]
    FS01 --> WALLPAPER[Wallpapers]
    FS01 --> SCREENSAVER[Screensavers]

```

















































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
