# Active Directory Domain Controller Setup - Phour Global Limited

A hands-on implementation of Windows Server Active Directory for a small cybersecurity services firm. This project demonstrates domain controller configuration, organizational unit design, user management, and group policy enforcement in a simulated enterprise environment.

## Project Overview

Phour Global Limited is a cybersecurity services company that needed centralized identity and access management. This project establishes a Windows Server-based Active Directory infrastructure to manage user accounts, computers, and security policies across three office locations.

## Infrastructure Design

### Network Topology

```
Internet
   ↓
Router (Gateway: 10.0.5.1)
   ↓
Switch
   ├── Windows Server 2012 (10.0.5.3) - Domain Controller
   ├── Windows 8 PC (10.0.5.5) - Sales Department
   └── Windows 8 PC (DHCP) - HR Department
```

### Environment Specifications

| Device | Hostname | IP Address | Role |
|--------|----------|------------|------|
| Windows Server 2012 | PHOURGLOBAL | 10.0.5.3 | Active Directory Domain Controller |
| Windows 8 Client | SALES-PC | 10.0.5.5 | Domain Member (Sales) |
| Windows 8 Client | (Default) | DHCP | Domain Member (HR) |

**Domain Configuration:**
- Domain Name: phourglobal.com
- Forest Functional Level: Windows Server 2012
- Roles Installed: Active Directory Domain Services, DNS

## Organizational Structure

The domain is organized by office location with department-specific groups:

```
phourglobal.com
├── ABUJA
│   ├── IT
│   │   ├── akeem_it@phourglobal.com
│   │   └── mutiat_it@phourglobal.com
│   ├── Sales
│   │   ├── hope_sales@phourglobal.com
│   │   └── rashidat_sales@phourglobal.com
│   └── Marketing
│       ├── cara_marketing@phourglobal.com
│       └── samson_marketing@phourglobal.com
├── JOS
│   ├── HR
│   │   └── ibrahim_hr@phourglobal.com
│   └── Digital Marketing
└── LAGOS
    ├── Consultation
    └── Finance
```

## Implementation Guide

### Phase 1: Domain Controller Setup

**1. Configure Static IP Address**
- Open Network Adapter settings
- Set IP: 10.0.5.3
- Subnet Mask: 255.255.255.0
- Default Gateway: 10.0.5.1
- DNS: 127.0.0.1 (points to itself after AD installation)

**2. Install Active Directory Domain Services**
```
Server Manager → Add Roles and Features → Role-based installation
→ Select server → Active Directory Domain Services → Add Features → Install
```

**3. Promote Server to Domain Controller**
- Click the notification flag in Server Manager
- Select "Promote this server to a domain controller"
- Choose "Add a new forest"
- Root domain name: phourglobal.com
- Set Directory Services Restore Mode password
- Accept default NetBIOS name: PHOURGLOBAL
- Review paths for database, log files, and SYSVOL
- Complete prerequisite check and install

**4. Post-Installation**
- Server automatically restarts
- Log in as: PHOURGLOBAL\Administrator
- Verify DNS role is installed and functioning

### Phase 2: Organizational Unit Design

**1. Open Active Directory Users and Computers**
```
Server Manager → Tools → Active Directory Users and Computers
```

**2. Create Location-Based OUs**
- Right-click phourglobal.com → New → Organizational Unit
- Create three OUs: ABUJA, JOS, LAGOS
- Uncheck "Protect container from accidental deletion" for lab environment

**3. Create Department Groups**

Within each OU, create security groups for departments:

**ABUJA:**
- IT (Security Group - Global)
- Sales (Security Group - Global)
- Marketing (Security Group - Global)

**JOS:**
- HR (Security Group - Global)
- Digital Marketing (Security Group - Global)

**LAGOS:**
- Consultation (Security Group - Global)
- Finance (Security Group - Global)

**4. Create User Accounts**

For each user:
- Right-click the appropriate group → New → User
- Fill in name and logon name (format: firstname_department)
- Set password and configure password policies
- Add user to their department group

### Phase 3: Client Computer Configuration

**1. Configure Network Settings on Windows 8 PC**
- IP Address: 10.0.5.3 (or obtain automatically)
- Subnet Mask: 255.255.255.0
- Default Gateway: 10.0.5.1
- Preferred DNS: 10.0.5.3 (points to domain controller)

**2. Join Computer to Domain**
```
System Properties → Computer Name → Change
→ Member of: Domain → phourglobal.com
→ Enter domain admin credentials
→ Restart computer
```

**3. Verify Domain Join**
- Log in with domain credentials: phourglobal\username
- Run: `gpresult /r` to verify group policy application

### Phase 4: Group Policy Implementation

**1. Install Group Policy Management Console**
```
Server Manager → Add Roles and Features → Features
→ Group Policy Management → Install
```

**2. Create Custom GPO**
- Open Group Policy Management (gpmc.msc)
- Right-click Group Policy Objects → New
- Name: Disable_ShutDown_Action

**3. Configure Policy Settings**
- Right-click Disable_ShutDown_Action → Edit
- Navigate to: User Configuration → Administrative Templates → Start Menu and Taskbar
- Enable: "Remove and prevent access to the Shut Down, Restart, Sleep, and Hibernate commands"

**4. Link GPO to Sales OU**
- Right-click ABUJA OU → Link an Existing GPO
- Select: Disable_ShutDown_Action

**5. Configure Security Filtering**
- Select the GPO → Delegation tab
- Remove "Authenticated Users"
- Add: hope_sales@phourglobal.com
- Grant: Read and Apply Group Policy permissions

**6. Enforce Policy**
- Right-click the GPO link → Enforced

**7. Apply Policy on Client**
```
On SALES-PC, run as administrator:
gpupdate /force
```

**8. Verify Policy Application**
- Log in as hope_sales@phourglobal.com on SALES-PC
- Check Start Menu → Power options should be hidden
- Press Ctrl+Alt+Delete → Shut down option should be unavailable

## Testing and Validation

### Connectivity Tests
```bash
# From Domain Controller
ping 10.0.5.5

# From Client PC
ping 10.0.5.3
nslookup phourglobal.com
```

### AD Verification
```bash
# Verify domain controller status
dcdiag

# Check replication (if multiple DCs exist)
repadmin /showrepl

# List domain computers
dsquery computer
```

### Group Policy Verification
```bash
# On client machine
gpresult /r
gpresult /h report.html
```

## Key Learnings

**Technical Skills Gained:**
- Windows Server administration and configuration
- Active Directory architecture and design principles
- DNS integration with Active Directory
- Group Policy creation and enforcement
- Security filtering and delegation
- Domain join procedures and troubleshooting

**IAM Concepts Applied:**
- Organizational unit hierarchy for access control
- Role-based access through security groups
- Centralized user management
- Policy-based security enforcement

## Security Considerations

This lab environment uses simplified settings for learning purposes. Production environments should implement:
- Complex password policies
- Account lockout thresholds
- Tiered administrative model
- Regular security audits
- Multi-factor authentication
- Restricted domain admin usage

## Future Enhancements

- Implement additional GPOs for password policies and account lockout
- Configure file server with shared folders and NTFS permissions
- Set up DHCP server for automatic IP assignment
- Create printer deployment policies
- Establish backup domain controller for redundancy
- Implement Windows Server Update Services (WSUS)

## Project Files

This repository includes:
- Complete project documentation with screenshots
- Step-by-step configuration guide
- Network topology diagrams
- Group policy settings reference

## Author

**Ibrahim Ayomide Fayomi**  
Cybersecurity Professional  
[LinkedIn](https://www.linkedin.com/in/fayomi31/)

## License

This project is for educational and portfolio purposes.

---

*Built with Windows Server 2012 and Windows 8 in a virtualized lab environment*
