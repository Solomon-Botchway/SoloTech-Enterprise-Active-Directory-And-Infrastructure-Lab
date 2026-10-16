# Documentation

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







## Printer Deployment Flow

1. User belongs to the Finance-Team security group.
2. Group Policy security filtering evaluates membership.
3. The printer deployment GPO applies.
4. The printer queue is mapped from the print server.
5. The department printer becomes available to the user.

## 🛠️ Architecture Workflow Diagram


```text
[ Active Directory Users ]
           │
           ▼
┌────────────────────────────────────────────────────────┐
│  1. User Logs On / GPO Triggers                       │
└────────────────────────────────────────────────────────┘
           │
           ▼
┌────────────────────────────────────────────────────────┐
│  2. Registry Preference (Processes First)              │
│     Sets: LegacyDefaultPrinterMode = 1                 │
│     Blocks: "Allow Windows to manage my default..."    │
└────────────────────────────────────────────────────────┘
           │
           ▼
┌────────────────────────────────────────────────────────┐
│  3. GPP Printer 'Replace' Cycle Evaluated             │
└────────────────────────────────────────────────────────┘
           │
           ├─► [User in 'Print-Finance' Group?] ──► YES: Map Finance Printer & Set Default
           │                                    └──► NO:  [Remove Device if previously mapped]
           │
           └─► [User in 'Print-HR' Group?]      ──► YES: Map HR Printer & Set Default
                                                └──► NO:  [Remove Device if previously mapped]
```


---


## 🎯 Technical Challenges Addressed


1. **Departmental Isolation:** Restricting network printer visibility so users can only see, list, and interact with their specific department's hardware.
2. **Dynamic Cross-Department Transfers:** Automatically purging stale printer mappings when a user shifts to a new department or security group, mitigating profile clutter.
3. **The "Print to PDF" Hijack:** Preventing Windows 10/11 client machines from falling back to "Microsoft Print to PDF" as the system default during policy refresh cycles.
4. **Permissions Overhead:** Preserving standard print server Access Control Lists (ACLs) to ensure drivers pull reliably without throwing "Access Denied" errors.


---


## ⚙️ Core Implementation Details


### 1. Visibility Filter via Item-Level Targeting
Instead of deploying printers globally to an entire Organizational Unit (OU), mappings are controlled via targeted Active Directory Security Groups (e.g., `Print-Finance`, `Print-HR`).
* **GPO Location:** `User Configuration` > `Preferences` > `Control Panel Settings` > `Printers`
* **Filtering Gatekeeper:** Configured on the **Common Tab** using **Item-Level Targeting**, mapping the preference rule specifically to `User is a member of the Security Group`.
* **Print Server Security:** Permissions on the Print Server are left at their healthy default (**Everyone: Print**). Security is cleanly managed at the GPO presentation layer rather than the server's ACLs.


### 2. Automated Self-Cleaning Configuration
To ensure the environment stays entirely clean if an employee changes departments, the policy alters the default GPO behavior:
* Enabled **"Remove this item when it is no longer applied"** under the GPP Common Tab.
* This changes the internal mechanics from an *Update* action to a **Replace** action (symbolized by a **Red Icon** in the Group Policy Management Console).
* At every background policy check or login, Windows validates group membership. If membership has been stripped, the old device is instantly deleted from the client workstation.


### 3. Securing the Default Status
Because the *Replace* action quickly tears down and rebuilds network printers during background refreshes, Windows clients can temporarily drop the network connection for a split second, defaulting back to *Microsoft Print to PDF*.


This layout guarantees absolute default persistence by enforcing two rules in a precise **Order of Operations**:
* **Administrative Template:** Enforced `Turn off Windows default printer management` under *User Configuration > Policies > Administrative Templates > Control Panel > Printers*.
* **Registry Pre-Conditioning:** Deployed a GPP Registry update to push `LegacyDefaultPrinterMode = 1` (REG_DWORD) under `HKCU\Software\Microsoft\Windows NT\CurrentVersion\Windows`.

## Scheduled Task Authentication & Network Drive Troubleshooting

### Environment

This lab was performed in a virtualized Active Directory environment built using Hyper-V.

#### Infrastructure

| System | Role |
|----------|----------|
| DC01 | Windows Server 2022 Domain Controller |
| PC01 | Windows 11 Enterprise Client |
| SOLOTECH.LOCAL | Active Directory Domain |
| Finance_Data | Department File Share |
| d.carter | Standard Domain User Test Account |

---

### Active Directory Configuration

The test user account was configured as:

```text
Username: d.carter
Domain: SOLOTECH.LOCAL
```

The account had:

- Access to department file shares
- A mapped Finance Department Drive (F:)
- Permission to create files and folders within the assigned department share

---

### File Share Configuration

A Finance department file share was hosted on DC01 and mapped to the user as:

```text
F:
```

Mapped location:

```text
\\DC01\Finance_Data
```

A test folder was created for report output:

```text
\\DC01\Finance_Data\Notes
```

---

### Scheduled Task Configuration

Task Name:

```text
Reports
```

Security Context:

```text
SOLOTECH\d.carter
```

Task Configuration:

```text
Run whether user is logged on or not
```

Trigger:

```text
Repeat every 5 minutes
```

Initial Test Action:

```cmd
whoami > C:\Temp\report.txt
```

Network Share Test Action:

```cmd
whoami > F:\Notes\report.txt
```

Final Working Action:

```cmd
whoami > \\DC01\Finance_Data\Notes\report.txt
```

---

### Group Policy Requirements

To enable the task to run successfully under the user account, the following User Right Assignment was configured:

```text
Computer Configuration
 └─ Policies
     └─ Windows Settings
         └─ Security Settings
             └─ Local Policies
                 └─ User Rights Assignment
                     └─ Log on as a batch job
```

The domain user account was granted the required permission and Group Policy was refreshed using:

```cmd
gpupdate /force
```

---

### Validation Tools Used

Throughout the investigation, the following Windows administration tools were used:

- Active Directory Users and Computers (ADUC)
- Group Policy Management Console (GPMC)
- Task Scheduler
- Event Viewer
- Command Prompt
- PowerShell
- File Explorer
- Network Drive Mapping (`net use`)
- GPResult

---

### Lab Objectives

This exercise was designed to:

- Understand Task Scheduler security contexts
- Troubleshoot scheduled task failures
- Validate Group Policy user rights assignments
- Investigate mapped network drive behaviour
- Compare interactive logons and batch logons
- Develop enterprise troubleshooting methodology
- Prepare for later account lockout investigations involving stale credentials

