# Documentation

Project reports and technical documents.

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

## Printer Deployment Flow

1. User belongs to the Finance-Team security group.
2. Group Policy security filtering evaluates membership.
3. The printer deployment GPO applies.
4. The printer queue is mapped from the print server.
5. The department printer becomes available to the user.
