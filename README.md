# Active Directory LAB Setup

This repository documents the steps I followed to set up a Windows Active Directory lab on VMware

> Domain: evilcorp.lab

## Index

* [Architecture](#architecture)
* [Configuring IP addresses](#configuring-ip-addresses)
* [Configuring Names of all Computers](#configuring-names-of-all-computers)
* [Downloading Active Directory Domain Services](#downloading-active-directory-domain-services)
* [Promoting server to Active Directory](#promoting-server-to-active-directory)
* [Connect Client Computers to the Active Directory Domain](#connect-client-computers-to-the-active-directory-domain)
* [Adding Users to the Active Directory Domain](#adding-users-to-the-active-directory-domain)
* [Making and deploying Policies](#making-and-deploying-policies)

---

# Architecture

```
Host Linux (Arch)
192.168.148.1
        |
        |
   VMnet1 (Host-only)
    192.168.148.0/24
------------------------------------------------
|                    |                         |
|                    |                         |
DC                  WIN10-U1                WIN10-A1
Server 2019        Windows 10               Windows 10              
192.168.148.10     192.168.148.20          192.168.148.30
DNS -> self        DNS -> 148.10           DNS -> 148.10
|
|
Domain: evilcorp.lab
|
|-- Users
|    |-- john.smith
|    |-- alice.dev
|    |-- bob.hr
|    |-- helpdesk1
|    |-- itadmin
|    |-- svc_backup
|    |-- svc_sql
|    ....Other users
|
|-- Computers
     |-- DC01
     |-- WIN10-U1
     |-- WIN10-A1
```

---

# Configuring IP addresses

- I am using Default VMNet Provided by VMware, 192.168.148.0/24
- The first IP address 192.168.148.1 is used by my linux device

### Configuring DC IP address

- Windows 2019 Server 
- Go to network and Internet Settings > Change adapter Options > Click on the only adapter available (Ethernet 0 in my case)
- Right Click on Ethernet0 > Properties
- Click on Internet Protocol Version 4
- Set the following IP address, Subnet Mask and DNS

<img src="images/1.png" width="50%" height="50%"/>



### Configuring Client IP addresses
- Windows 10
- Go to network and Internet Settings > Ethernet > Change Setting to manual > Set Ipv4 to ON > Put IP address
- [IMP]: Make sure to keep the server IP address as the DNS

<img src="images/1-1.png" width="30%" height="30%"/>
  

For WIN10U1
  
<img src="images/2.png" width="30%" height="30%"/>



For WIN10A1

<img src="images/3.png" width="30%" height="30%"/>

---

# Configuring Names of all Computers

Settings > System > About > Rename this PC

For DC:

<img src="images/4.png" width="60%" height="60%"/>

For Client PCs:

<img src="images/5.png" width="60%" height="60%"/>

<img src="images/6.png" width="60%" height="60%"/>

---

# Downloading Active Directory Domain Services

- Server Manager > Add Roles and Features

<img src="images/7.png" width="60%" height="60%"/>

- Select AD Domain Services 
- This installs DNS and AD Domain Services Both

<img src="images/8.png" width="30%" height="30%"/>

- Install AD Domain Services

<img src="images/9.png" width="50%" height="50%"/>

---

# Promoting server to Active Directory

- Go to Server Manager > Notifications > Click 'Promote'

<img src="images/10.png" width="30%" height="30%"/>

- Add a new forest > Put root domain name 'evilcorp.lab' in my case

<img src="images/11.png" width="50%" height="50%"/>

- Set password and Confirm

<img src="images/12.png" width="50%" height="50%"/>

- Install and Restart 

<img src="images/13.png" width="60%" height="60%"/>

- In the server you can login using the domain now

<img src="images/14.png" width="50%" height="50%"/>

- On Server Manager you can see AD DS and DNS services are up and running

<img src="images/15.png" width="50%" height="50%"/>

---

# Connect Client Computers to the Active Directory Domain 

In Both the Client Computers: 
- Settings > About > Click on 'Rename this PC (Advanced)' on the far right

<img src="images/16.png" width="70%" height="70%"/>

- Click on Change Button, here we are changing the domain of the PC

<img src="images/17.png" width="50%" height="50%"/>

- Add domain 'evilcorp.lab' and click OK

<img src="images/18.png" width="50%" height="50%"/>

- Authenticate with username and password of the Admin of our domain

<img src="images/19.png" width="30%" height="30%"/>

- Congratulations, the computer is now connected to evilcorp.lab domain

<img src="images/20.png" width="60%" height="60%"/>

- Repeat the same for other computer too

### Verification

I can login to both cliet computers using evilcorp\Administrator 

<img src="images/21.png" width="50%" height="50%"/>

<img src="images/22.png" width="50%" height="50%"/>

I can also check on Domain Controller Server > 'Active Directory Users and Computers' > Computers 

<img src="images/23.png" width="60%" height="60%"/>

I can see Both my client PCs.

---

# Adding Users to the Active Directory Domain

I will have 12 users in this lab

Regular Employees(OU = Dev, HR, Finance, Marketing): 

| Name          | Username      | Department |
| ------------- | ------------- | ---------- |
| Emma Brown    | emma.brown    | Dev        |
| Kevin White   | kevin.white   | Dev        |
| Bob Miller    | bob.miller    | HR         |
| Daniel Clark  | daniel.clark  | HR         |
| Sarah Lee     | sarah.lee     | Finance    |
| Mia Walker    | mia.walker    | Finance    |
| John Smith    | john.smith    | Marketing  |
| Ethan Hall    | ethan.hall    | Marketing  |

Admin Users(OU = IT):

| Name         | Username    |
| ------------ | ----------- |
| Helpdesk     | helpdesk    |
| IT Admin     | itadmin     |

Service Accounts(OU = Services):

| Name            | Username    |
| --------------- | ----------- |
| SQL Service     | svc_sql     |
| Backup Service  | svc_backup  |

### Adding users

- Go to 'Active Directory Users and Computers' > evilcorp.lab > Right click > Create a New Organizational Unit
- Name it 'EVILCORP'

<img src="images/24.png" width="50%" height="50%"/>


- Go to EVILCORP OU
- Right Click > Create new User
- Fill in details, repeat for all other users

<img src="images/25.png" width="50%" height="50%"/>

- Just for the lab I will keep a simple password, and set Password never expires option

<img src="images/26.png" width="50%" height="50%"/>

- Once I add all the users and divide them in OUs, this is how the structure looks like:

<img src="images/27.png" width="50%" height="50%"/>

### Verifying

I try signing into a client computer using credentials of Daniel Clark

<img src="images/28.png" width="50%" height="50%"/>

I can sign in

<img src="images/29.png" width="50%" height="50%"/>

---

# Making and deploying Policies

- For this, I will block the control panel access to all users of HR, Finance and Marketing

- Go to 'Group Policy Management' software on DC Server
- Go to Group Policy Objects

<img src="images/30.png" width="50%" height="50%"/>

- Right Click > New 
- Make New group Policy Object
- Calling it 'Contol Panel Access Block'

<img src="images/31.png" width="30%" height="30%"/>

- On the newly created GPO > Right Click > Edit
- Go to Control Panel under User Configuration

<img src="images/32.png" width="60%" height="60%"/>

- Right click on 'Prohibit Access to Control Panel and PC Settings'
- Set to enabled and Click on Apply

<img src="images/33.png" width="50%" height="50%"/>

- Close the window > Head back to Group Policy Objects
- Click on the settings tab, you can see summary of the rule applied

<img src="images/34.png" width="70%" height="70%"/>

- Drag and drop GPO to the desired OUs

<img src="images/35.png" width="50%" height="50%"/>

### Verifying

- I login to Daniel Clark's account (who is in HR department) and start Control Panel from a client PC

<img src="images/36.png" width="70%" height="70%"/>

As you can see, I cannot start Control Panel.

---

Next Steps:
I will be exploiting Active Directory, will create another repository or add in this repository.  
Thank you.
