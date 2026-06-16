# Active Directory LAB Setup

Domain: evilcorp.lab

---

## Architecture

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

<img src="images/11.png" width="40%" height="40%"/>

- Set password and Confirm

<img src="images/12.png" width="40%" height="40%"/>

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

<img src="images/17.png" width="40%" height="40%"/>

- Add domain 'evilcorp.lab' and click OK

<img src="images/18.png" width="40%" height="40%"/>

- Authenticate with username and password of the Admin of our domain

<img src="images/19.png" width="30%" height="30%"/>

- Congratulations, the computer is now connected to evilcorp.lab domain

<img src="images/20.png" width="60%" height="60%"/>

### Verification

I can login to both cliet computers using evilcorp\Administrator 

<img src="images/21.png" width="50%" height="50%"/>

<img src="images/22.png" width="50%" height="50%"/>

I can also check on Domain Controller Server > 'Active Directory Users and Computers' > Computers 

<img src="images/23.png" width="60%" height="60%"/>

I can see Both my client PCs.