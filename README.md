# Active Directory Domain Controller Setup - Phour Global Limited

A hands-on implementation of Windows Server Active Directory for a small cybersecurity services firm. This project demonstrates domain controller configuration, organizational unit design, user management, and group policy enforcement in a simulated enterprise environment.

---

## Project Overview

Phour Global Limited is a cybersecurity services company that needed centralized identity and access management. This project establishes a Windows Server-based Active Directory infrastructure to manage user accounts, computers, and security policies across three office locations.

---

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

### Organizational Structure

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

---

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

<img width="825" height="421" alt="image" src="https://github.com/user-attachments/assets/128deaa7-3429-4378-971d-34a1a7ba31c6" />
<img width="825" height="423" alt="image" src="https://github.com/user-attachments/assets/3ea80015-0c38-4dcf-9dfa-098770039dce" />
<img width="825" height="437" alt="image" src="https://github.com/user-attachments/assets/6548ec27-093d-4a76-b7e7-6826b37cb003" />

*Opening Server Manager to begin AD DS installation*

<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/eb7deed9-70fc-4dd9-ad0f-6537c8ca856a" />

*Selecting Active Directory Domain Services role from the server roles list*

**3. Promote Server to Domain Controller**
- Click the notification flag in Server Manager
- Select "Promote this server to a domain controller"
- Choose "Add a new forest"
- Root domain name: phourglobal.com
- Set Directory Services Restore Mode password
- Accept default NetBIOS name: PHOURGLOBAL
- Review paths for database, log files, and SYSVOL
- Complete prerequisite check and install

<img width="825" height="439" alt="image" src="https://github.com/user-attachments/assets/e43272a6-1b1c-4a0c-8789-dcc76d9d8f21" />
<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/0bed036b-5747-459c-8b25-19afa199c67d" />
<img width="825" height="439" alt="image" src="https://github.com/user-attachments/assets/69f41463-8343-4fa9-963e-1b825907e16f" />

*Creating new forest and setting root domain name to phourglobal.com*

<img width="825" height="439" alt="image" src="https://github.com/user-attachments/assets/d7a93cfb-b303-4dff-8f8d-0b758cdd3922" />

*All prerequisite checks passed, ready to install domain controller*

**4. Post-Installation**
- Server automatically restarts
- Log in as: PHOURGLOBAL\Administrator
- Verify DNS role is installed and functioning

---

### Phase 2: Organizational Unit Design

**1. Open Active Directory Users and Computers**
```
Server Manager → Tools → Active Directory Users and Computers
```

<img width="825" height="436" alt="image" src="https://github.com/user-attachments/assets/d1aa0a83-e1ab-409a-9dd5-93751ed08813" />
<img width="825" height="437" alt="image" src="https://github.com/user-attachments/assets/fd1da3ae-aab8-4f91-8b0e-32b151318295" />

*Active Directory Users and Computers management console*

**2. Create Location-Based OUs**
- Right-click phourglobal.com → New → Organizational Unit
- Create three OUs: ABUJA, JOS, LAGOS
- Uncheck "Protect container from accidental deletion" for lab environment

<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/f7d6aeec-fe25-4632-9b4b-216e508713c1" />
<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/15a0c105-7861-4a09-a63d-ac9a420359f4" />
<img width="825" height="436" alt="image" src="https://github.com/user-attachments/assets/80704277-d343-4719-8624-b339b179a94e" />

*Three location-based OUs created: ABUJA, JOS, and LAGOS*

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

<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/3d35f1fb-0035-4062-9bec-4920ac48dc7e" />
<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/6d584016-aead-42d3-b66c-8a0dd1d2a0fa" />

*Consultation and Finance groups created for LAGOS OU*

<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/526fc184-1e9e-4930-a0a3-38e4da0db9cd" />

*Marketing, IT, and Sales groups created for ABUJA OU*

---

### Phase 3: User Account Creation

**1. Create User Accounts**

For each user:
- Right-click the appropriate group → New → User
- Fill in name and logon name (format: firstname_department)
- Set password and configure password policies
- Add user to their department group

<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/fb5e25e8-a8e0-4a86-b38f-7f607f91d1b7" />
<img width="825" height="436" alt="image" src="https://github.com/user-attachments/assets/bffbe8b5-e115-42f1-b545-c44f13032224" />

*Creating and naming user under Sales group*

<img width="825" height="439" alt="image" src="https://github.com/user-attachments/assets/eae483dc-0c48-4145-9055-2e06bf1a3371" />

*Configuring password policies for the user*

**2. Add Users to Security Groups**

After creating users, add them to their respective security groups. Open the group properties, go to the Members tab, and add the appropriate users.

<img width="825" height="436" alt="image" src="https://github.com/user-attachments/assets/caf672e1-4b9d-469c-90f5-67f242cb3206" />

*Multiple users created across different department groups*

<img width="825" height="395" alt="image" src="https://github.com/user-attachments/assets/df59e7c6-4f8b-4aa4-87ce-9e94d92753e1" />
<img width="825" height="436" alt="image" src="https://github.com/user-attachments/assets/3f8c37ca-77cd-48e3-b82d-ebc2d8fd6327" />
<img width="825" height="439" alt="image" src="https://github.com/user-attachments/assets/cd797baf-9ab8-4541-8e24-6c1322374c5d" />
<img width="825" height="436" alt="image" src="https://github.com/user-attachments/assets/93abe7e0-1751-4ac3-afe6-9c1be6193f7b" />
<img width="825" height="394" alt="image" src="https://github.com/user-attachments/assets/2876bbbd-e716-4126-b173-3cfed3e8d1b6" />
<img width="825" height="386" alt="image" src="https://github.com/user-attachments/assets/c53ace5f-8042-448b-be1d-84fd71258ab5" />
<img width="825" height="385" alt="image" src="https://github.com/user-attachments/assets/4acc4b62-b97e-4e49-886d-46c4e810ef7c" />

*Adding users to the IT security group*

---

### Phase 4: Client Network Configuration

**1. Configure Network Settings on Windows 8 PC**
- IP Address: 10.0.5.5 (or obtain automatically)
- Subnet Mask: 255.255.255.0
- Default Gateway: 10.0.5.1
- Preferred DNS: 10.0.5.3 (points to domain controller)

<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/a0e191ee-3e24-472c-a014-bb17f7997046" />
<img width="675" height="539" alt="image" src="https://github.com/user-attachments/assets/7a82f309-169f-407a-bffc-9c7170df4c59" />

*Verifying the domain controller IP address configuration*

<img width="600" height="478" alt="image" src="https://github.com/user-attachments/assets/f1d788a2-d935-49e0-9924-86bb4dfa9580" />
<img width="600" height="480" alt="image" src="https://github.com/user-attachments/assets/ca1c1aed-b5ac-46ae-b488-4da6bdcbfc5f" />
<img width="600" height="476" alt="image" src="https://github.com/user-attachments/assets/55809e1b-86dc-4a04-83e7-5cf3dbfaa3c1" />
<img width="600" height="481" alt="image" src="https://github.com/user-attachments/assets/3b1bdfe8-a986-46c5-86b3-21db65195467" />

*Configuring IP address and DNS settings on Windows 8 client*

---

### Phase 5: Joining Computers to Domain

**1. Join Computer to Domain**
```
System Properties → Computer Name → Change
→ Member of: Domain → phourglobal.com
→ Enter domain admin credentials
→ Restart computer
```
<img width="600" height="480" alt="image" src="https://github.com/user-attachments/assets/6a4bcd8c-b5e2-465a-b79e-fb013c0c2660" />
<img width="600" height="483" alt="image" src="https://github.com/user-attachments/assets/5166cc4f-71a9-4d4c-b622-c1971a3af140" />
<img width="600" height="480" alt="image" src="https://github.com/user-attachments/assets/10bec18b-f03f-4965-aa11-9dbab4b3e19d" />
<img width="600" height="482" alt="image" src="https://github.com/user-attachments/assets/c332bf4c-08c1-4db4-828c-6b8afe42957f" />
<img width="600" height="479" alt="image" src="https://github.com/user-attachments/assets/5a88eeba-16d4-42db-a0a6-c857d3b508ad" />

*Entering the domain name: phourglobal.com*

<img width="600" height="481" alt="image" src="https://github.com/user-attachments/assets/aab5ff8f-de07-4983-88a6-66c3db1f96d1" />
*Welcome message confirming successful domain join*

**2. Verify Domain Join**
- Log in with domain credentials: phourglobal\username
- Run: `gpresult /r` to verify group policy application

---

### Phase 6: Group Policy Implementation

**1. Install Group Policy Management Console**
```
Server Manager → Add Roles and Features → Features
→ Group Policy Management → Install
```

**2. Create Custom GPO**
- Open Group Policy Management (gpmc.msc)
- Right-click Group Policy Objects → New
- Name: Disable_ShutDown_Action

<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/99b395c5-343e-4606-aa89-23cd0ecc3d3f" />
<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/452560f2-068e-458d-a68d-3fc8d4345d90" />
<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/f847c35b-8506-4475-8be5-df02263a2412" />

*Creating new GPO named Disable_ShutDown_Action*

**3. Configure Policy Settings**
- Right-click Disable_ShutDown_Action → Edit
- Navigate to: User Configuration → Administrative Templates → Start Menu and Taskbar
- Enable: "Remove and prevent access to the Shut Down, Restart, Sleep, and Hibernate commands"

<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/302c8b11-a167-42a4-b711-f0781334021f" />
<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/cf07d2fd-5919-476f-8f40-ac5ee35cce71" />

*Opening Start Menu and Taskbar settings to configure shutdown restriction*

<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/845cd7ce-d47f-4aa8-877f-069f9d2f78e9" />

*Confirming the policy is set to Enabled*

**4. Link GPO to Sales OU**
- Right-click ABUJA OU → Link an Existing GPO
- Select: Disable_ShutDown_Action

**5. Configure Security Filtering**
- Select the GPO → Delegation tab
- Remove "Authenticated Users"
- Add: hope_sales@phourglobal.com
- Grant: Read and Apply Group Policy permissions

<img width="825" height="440" alt="image" src="https://github.com/user-attachments/assets/d56cf806-bff3-4f67-84fb-fc95c724a8f6" />
<img width="825" height="439" alt="image" src="https://github.com/user-attachments/assets/ad47cfad-b354-4ee7-93b1-a5a1bf598636" />
<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/12e38295-4738-42b9-b245-c76c164ada60" />
<img width="825" height="438" alt="image" src="https://github.com/user-attachments/assets/dfa40606-691d-466e-b8da-4583e17ee8d2" />

*Adding hope_sales user to security filtering*

**6. Enforce Policy**
- Right-click the GPO link → Enforced

**7. Apply Policy on Client**
```
On SALES-PC, run as administrator:
gpupdate /force
```

<img width="675" height="508" alt="image" src="https://github.com/user-attachments/assets/8c322553-47f7-460f-ae81-f33fdeff050f" />

*Running gpupdate /force on Windows 8 client machine*

**8. Verify Policy Application**
- Log in as hope_sales@phourglobal.com on SALES-PC
- Check Start Menu → Power options should be hidden
- Press Ctrl+Alt+Delete → Shut down option should be unavailable

<img width="750" height="567" alt="image" src="https://github.com/user-attachments/assets/75ad8b0f-3943-4098-aa70-017528ad98f7" />

*Verification: Power options removed from Start Menu*

<img width="825" height="620" alt="image" src="https://github.com/user-attachments/assets/5c04c485-f21a-4551-a2ea-a0e4d74eb32b" />

*Verification: Shutdown option unavailable in Ctrl+Alt+Delete menu*

---

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

---

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

---

## Security Considerations

This lab environment uses simplified settings for learning purposes. Production environments should implement:
- Complex password policies
- Account lockout thresholds
- Tiered administrative model
- Regular security audits
- Multi-factor authentication
- Restricted domain admin usage

---

## Future Enhancements

- Implement additional GPOs for password policies and account lockout
- Configure file server with shared folders and NTFS permissions
- Set up DHCP server for automatic IP assignment
- Create printer deployment policies
- Establish backup domain controller for redundancy
- Implement Windows Server Update Services (WSUS)

---

## Project Files

This repository includes:
- Complete project documentation with screenshots
- Step-by-step configuration guide
- Network topology diagrams
- Group policy settings reference

---

## Author

**Ibrahim Ayomide Fayomi**  
Cybersecurity Professional  
[LinkedIn](#) | [Portfolio](#)

## License

This project is for educational and portfolio purposes.

---

*Built with Windows Server 2012 and Windows 8 in a virtualized lab environment*
