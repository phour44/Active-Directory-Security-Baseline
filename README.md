# Active Directory Domain Controller Setup - Phour Global Limited

A hands-on implementation of Windows Server Active Directory for a small cybersecurity services firm. This project demonstrates domain controller configuration, organizational unit design, user management, and group policy enforcement in a simulated enterprise environment.

---

## Project Overview

Phour Global Limited is a cybersecurity services company that needed centralized identity and access management. This project establishes a Windows Server-based Active Directory infrastructure to manage user accounts, computers, and security policies across three office locations.

This project successfully implemented a complete Active Directory infrastructure. The environment includes a fully functional domain controller managing user authentication, DNS resolution, and centralized policy enforcement across multiple client computers.

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

### Network Diagrams

![Network Diagram 1](screenshots/image1.jpeg)
*Initial network topology and planning*

![Network Diagram 2](screenshots/image2.jpeg)
*Network infrastructure layout*

![Network Diagram 3](screenshots/image3.png)
*Server and client configuration*

![Network Diagram 4](screenshots/image4.png)
*Complete network architecture*

![Network Setup 1](screenshots/image5.png)
*Network configuration overview*

![Network Setup 2](screenshots/image6.png)
*Infrastructure components*

---

## Implementation Guide

## Phase 1: Domain Controller Setup

### Step 1: Configure Static IP Address

Before installing Active Directory, assign a static IP address to the server.

- Open Network Adapter settings
- Set IP: 10.0.5.3
- Subnet Mask: 255.255.255.0
- Default Gateway: 10.0.5.1
- DNS: 127.0.0.1 (points to itself after AD installation)

### Step 2: Install Active Directory Domain Services

![Setting up Active Directory](screenshots/image7.jpeg)
*Opening Server Manager to begin AD DS installation*

Launch Server Manager and begin the installation process:

```
Server Manager → Add Roles and Features → Role-based installation
→ Select server → Active Directory Domain Services → Add Features → Install
```

![Adding Roles and Features](screenshots/image8.jpeg)
*Add Roles and Features wizard initial screen*

Begin by opening Server Manager and selecting Add Roles and Features. This launches the wizard that will guide you through installing Active Directory Domain Services.

### Step 3: Role-Based Installation

![Role-Based Installation](screenshots/image9.jpeg)
*Selecting role-based or feature-based installation*

Choose Role-based or feature-based installation and select the local server from the server pool.

![Machine Name Configuration](screenshots/image10.jpeg)
*Confirming the server selection and machine name*

### Step 4: Installing Active Directory Domain Services

![Installing AD DS](screenshots/image11.jpeg)
*Selecting Active Directory Domain Services role*

Select Active Directory Domain Services from the server roles list. The wizard will automatically include the required management tools.

![Adding AD DS](screenshots/image12.jpeg)
*Adding required features for AD DS*

![Installing AD DS Progress](screenshots/image13.jpeg)
*Installation progress screen*

![Installing AD DS Confirmation](screenshots/image14.png)
*Confirming AD DS installation settings*

![AD DS Installation Complete](screenshots/image15.jpeg)
*AD DS installation completed successfully*

![AD DS Installed Verification](screenshots/image16.jpeg)
*Verification that AD DS is now installed on the server*

### Step 5: Promote Server to Domain Controller

After the AD DS installation completes, click the notification flag in Server Manager and select "Promote this server to a domain controller". This begins the domain controller configuration wizard.

![Promoting Server to Domain Controller](screenshots/image17.jpeg)
*Post-deployment configuration notification*

![Promotion Wizard](screenshots/image18.jpeg)
*Domain Controller promotion wizard starts*

### Step 6: Create New Forest and Domain

![Adding New Forest](screenshots/image19.jpeg)
*Creating new forest and setting root domain name to phourglobal.com*

- Click the notification flag in Server Manager
- Select "Promote this server to a domain controller"
- Choose "Add a new forest"
- Root domain name: phourglobal.com
- Set Directory Services Restore Mode password
- Accept default NetBIOS name: PHOURGLOBAL
- Review paths for database, log files, and SYSVOL
- Complete prerequisite check and install

![Setting DSRM Password](screenshots/image20.jpeg)
*Setting the Directory Services Restore Mode password*

![NetBIOS Domain Name](screenshots/image21.jpeg)
*Accepting the default NetBIOS name (PHOURGLOBAL)*

![Database and Log Folders](screenshots/image22.jpeg)
*Specifying paths for AD database, log files, and SYSVOL*

### Step 7: Prerequisites Check and Installation

![Prerequisites Check](screenshots/image23.jpeg)
*All prerequisite checks passed, ready to install domain controller*

The wizard verifies all prerequisites before installation. Once all checks pass, proceed with the installation. The server will automatically restart after completion.

**Post-Installation:**
- Server automatically restarts
- Log in as: PHOURGLOBAL\Administrator
- Verify DNS role is installed and functioning

After installation is completed, the computer will be signed out and restarted. You need to log back in with the password you created as the new admin of the domain (phourglobal.com).

---

## Phase 2: Organizational Unit Design

After logging back in as the domain administrator, open Active Directory Users and Computers from Server Manager tools. Organizational Units provide a way to organize users, groups, and computers by department or location.

### Step 1: Open Active Directory Users and Computers

```
Server Manager → Tools → Active Directory Users and Computers
```

![Adding OU](screenshots/image24.jpeg)
*Accessing Active Directory Users and Computers*

![AD Users and Computers Interface](screenshots/image25.png)
*Active Directory Users and Computers management console*

### Step 2: Create Location-Based OUs

Right-click the domain name and select New, then Organizational Unit. Create three OUs representing the company locations: ABUJA, JOS, and LAGOS.

![Adding Organizational Units](screenshots/image26.jpeg)
*Creating new Organizational Unit dialog*

- Right-click phourglobal.com → New → Organizational Unit
- Create three OUs: ABUJA, JOS, LAGOS
- Uncheck "Protect container from accidental deletion" for lab environment

![Creating OU](screenshots/image27.jpeg)
*Naming the organizational unit*

![Three OUs Created](screenshots/image28.jpeg)
*Three location-based OUs created: ABUJA, JOS, and LAGOS*

### Step 3: Create Department Groups

Within each OU, create security groups for different departments. Right-click the OU, select New, then Group. Set the group type to Security and scope to Global.

![Creating Groups](screenshots/image29.jpeg)
*Creating new security group*

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

![Creating Finance Group](screenshots/image30.jpeg)
*Creating Finance group under LAGOS OU*

![LAGOS Groups Created](screenshots/image31.png)
*Consultation and Finance groups created for LAGOS OU*

![JOS Groups Created](screenshots/image32.jpeg)
*HR and Digital Marketing groups created for JOS OU*

![ABUJA Groups Created](screenshots/image33.jpeg)
*Marketing, IT, and Sales groups created for ABUJA OU*

---

## Phase 3: User Account Creation

User accounts are created within the appropriate department groups. Each user is assigned a unique username following the format firstname_department and added as a member of their department security group.

### Step 1: Create User Accounts

For each user:
- Right-click the appropriate group → New → User
- Fill in name and logon name (format: firstname_department)
- Set password and configure password policies
- Add user to their department group

![Creating Users](screenshots/image34.jpeg)
*Starting the new user creation process*

![Naming User Under Sales](screenshots/image35.jpeg)
*Creating and naming user under Sales group*

![Setting Password Rules](screenshots/image36.jpeg)
*Configuring password policies for the user*

![User Created Under Sales](screenshots/image37.jpeg)
*User successfully created under Sales group*

![Users Created Under Each Group](screenshots/image38.jpeg)
*Multiple users created across different department groups*

### Step 2: Add Users to Security Groups

After creating users, add them to their respective security groups. Open the group properties, go to the Members tab, and add the appropriate users.

![Checking IT Properties](screenshots/image39.jpeg)
*Checking IT group properties to view and add members*

![Adding Users to IT Group](screenshots/image40.jpeg)
*Adding users to the IT security group*

![Adding Users to IT Group 2](screenshots/image41.jpeg)
*Continuing to add users to IT group*

![Finding Users for IT Group](screenshots/image42.jpeg)
*Searching for users to add to IT group*

![Finding Users 1](screenshots/image43.jpeg)
*Locating specific users in Active Directory*

![Finding Users 2](screenshots/image44.jpeg)
*User search results*

![User Added to IT Group](screenshots/image45.jpeg)
*Confirmation that users have been added to IT group*

---

## Phase 4: Client Network Configuration

Before joining client computers to the domain, configure their network settings to use the domain controller as the DNS server. This allows the clients to resolve the domain name and locate domain resources.

### Step 1: Verify Server Connectivity

![Checking Server IP Address](screenshots/image46.jpeg)
*Verifying the domain controller IP address configuration*

![Server Ping Response](screenshots/image47.jpeg)
*Server successfully responding to ping requests*

![Windows 8 Ping Response](screenshots/image48.jpeg)
*Windows 8 client successfully pinging the domain controller*

### Step 2: Configure Network Settings on Windows 8 PC

Open Network and Sharing Center, click on the active connection, select Properties, then configure IPv4 settings. Set the preferred DNS server to 10.0.5.3 (the domain controller IP address).

- IP Address: 10.0.5.5 (or obtain automatically)
- Subnet Mask: 255.255.255.0
- Default Gateway: 10.0.5.1
- Preferred DNS: 10.0.5.3 (points to domain controller)

![Configuring Network Settings](screenshots/image49.jpeg)
*Opening network adapter settings*

![Network Settings 2](screenshots/image50.jpeg)
*Accessing network connection properties*

![Network Settings 3](screenshots/image51.jpeg)
*Selecting Internet Protocol Version 4 (TCP/IPv4)*

![Network Settings 4](screenshots/image52.jpeg)
*Configuring IP address and DNS settings*

![Network Settings 5](screenshots/image53.jpeg)
*Entering domain controller IP as DNS server*

![Network Settings 6](screenshots/image54.jpeg)
*Confirming network configuration changes*

---

## Phase 5: Joining Computers to Domain

With network settings configured, join the Windows 8 client computer to the phourglobal.com domain. This process establishes trust between the client and the domain controller.

### Step 1: Access System Properties

```
System Properties → Computer Name → Change
→ Member of: Domain → phourglobal.com
→ Enter domain admin credentials
→ Restart computer
```

![Configuring PC to Join Domain](screenshots/image55.jpeg)
*Accessing system properties to change computer name and domain*

![Configuring PC 1](screenshots/image56.jpeg)
*Opening Computer Name/Domain Changes dialog*

![Configuring PC 2](screenshots/image57.jpeg)
*Selecting domain membership option*

![Configuring PC 3 Domain](screenshots/image58.jpeg)
*Entering the domain name: phourglobal.com*

![Configuring PC 4 Domain](screenshots/image59.jpeg)
*Providing domain administrator credentials*

![Configuring PC 5 Domain](screenshots/image60.jpeg)
*Welcome message confirming successful domain join*

![PC Configuration Done](screenshots/image61.jpeg)
*Restart required to complete domain join process*

### Step 2: Verify Domain Join

- Log in with domain credentials: phourglobal\username
- Run: `gpresult /r` to verify group policy application

---

## Phase 6: Group Policy Management

Group Policy allows centralized management of user and computer settings. This section demonstrates creating and applying a policy to restrict shutdown options for specific users.

### Step 1: Install Group Policy Management Console

```
Server Manager → Add Roles and Features → Features
→ Group Policy Management → Install
```

![Setting Up Group Policy Management](screenshots/image62.jpeg)
*Opening Group Policy Management Console*

![Group Policy Management 1](screenshots/image63.jpeg)
*Group Policy Management interface*

![Group Policy Management 2](screenshots/image64.jpeg)
*Navigating Group Policy Objects*

![Group Policy Management 3](screenshots/image65.jpeg)
*Preparing to create new Group Policy Object*

### Step 2: Create Custom GPO

- Open Group Policy Management (gpmc.msc)
- Right-click Group Policy Objects → New
- Name: Disable_ShutDown_Action

![Creating Disable Shutdown GPO](screenshots/image66.jpeg)
*Creating new GPO named Disable_ShutDown_Action*

### Step 3: Configure Policy Settings

- Right-click Disable_ShutDown_Action → Edit
- Navigate to: User Configuration → Administrative Templates → Start Menu and Taskbar
- Enable: "Remove and prevent access to the Shut Down, Restart, Sleep, and Hibernate commands"

![Editing Disable Shutdown Policy](screenshots/image67.jpeg)
*Opening Group Policy Editor*

![Editing Policy 2](screenshots/image68.jpeg)
*Navigating to User Configuration settings*

![Editing Policy 3](screenshots/image69.jpeg)
*Accessing Administrative Templates*

![Editing Policy 4](screenshots/image70.jpeg)
*Opening Start Menu and Taskbar settings*

![Editing Policy 5](screenshots/image71.jpeg)
*Locating the shutdown restriction policy*

![Editing Policy 6](screenshots/image72.jpeg)
*Configuring the policy for specific users*

![Policy Enabled Confirmation](screenshots/image73.jpeg)
*Confirming the policy is set to Enabled*

### Step 4: Link GPO to Sales OU

- Right-click ABUJA OU → Link an Existing GPO
- Select: Disable_ShutDown_Action

![Linking GPO to Domain](screenshots/image74.jpeg)
*Linking the GPO to the ABUJA OU*

![Linking GPO 2](screenshots/image75.jpeg)
*Selecting the Disable_ShutDown_Action GPO*

### Step 5: Configure Security Filtering

- Select the GPO → Delegation tab
- Remove "Authenticated Users"
- Add: hope_sales@phourglobal.com
- Grant: Read and Apply Group Policy permissions

![Assigning GPO to Users and Computers](screenshots/image76.jpeg)
*Configuring security filtering for the GPO*

![Assigning GPO 2](screenshots/image77.jpeg)
*Managing GPO delegation and permissions*

![Assigning to hope_sales](screenshots/image78.jpeg)
*Adding hope_sales user to security filtering*

![Assigning to hope_sales 2](screenshots/image79.jpeg)
*Confirming user assignment to GPO*

### Step 6: Enforce Policy

- Right-click the GPO link → Enforced

![Enforcing GPO](screenshots/image80.jpeg)
*Enabling GPO enforcement to prevent override*

### Step 7: Apply Policy on Client

```
On SALES-PC, run as administrator:
gpupdate /force
```

![Policy Updated on Windows 8](screenshots/image81.jpeg)
*Running gpupdate /force on Windows 8 client machine*

![Policy Updated on Server](screenshots/image82.jpeg)
*Running gpupdate /force on Windows Server*

### Step 8: Verify Policy Application

- Log in as hope_sales@phourglobal.com on SALES-PC
- Check Start Menu → Power options should be hidden
- Press Ctrl+Alt+Delete → Shut down option should be unavailable

![Power Options Not Available](screenshots/image83.jpeg)
*Verification: Power options removed from Start Menu*

![No Power Options in Ctrl+Alt+Del](screenshots/image84.jpeg)
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

### Technical Skills Gained

- Windows Server administration and configuration
- Active Directory architecture and design principles
- DNS integration with Active Directory
- Group Policy creation and enforcement
- Security filtering and delegation
- Domain join procedures and troubleshooting

### IAM Concepts Applied

- Organizational unit hierarchy for access control
- Role-based access through security groups
- Centralized user management
- Policy-based security enforcement

### Skills Demonstrated

This project demonstrates proficiency in Windows Server administration, Active Directory design and implementation, DNS configuration, group policy management, and identity and access management principles. The implementation follows industry best practices for domain controller deployment and organizational unit design.

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

Additional improvements could include:

- Implement additional GPOs for password policies and account lockout
- Configure file server with shared folders and NTFS permissions
- Set up DHCP server for automatic IP assignment
- Create printer deployment policies
- Establish backup domain controller for redundancy
- Implement Windows Server Update Services (WSUS)

---

## Project Summary

### Key Accomplishments

Configured Windows Server 2012 as the primary domain controller for phourglobal.com with integrated DNS services. Established a three-tier organizational unit structure representing company locations in Abuja, Jos, and Lagos. Created department-specific security groups and user accounts following standard naming conventions. Successfully joined Windows 8 client computers to the domain. Implemented and tested group policy for user-specific access control.

---

## Author

**Ibrahim Ayomide Fayomi**  
Cybersecurity Professional  
[LinkedIn](#) | [Portfolio](#)

## License

This project is for educational and portfolio purposes.

---

*Built with Windows Server 2012 and Windows 8 in a virtualized lab environment*
*January 2026*
