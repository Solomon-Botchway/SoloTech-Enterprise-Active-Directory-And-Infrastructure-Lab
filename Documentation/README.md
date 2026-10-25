

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

