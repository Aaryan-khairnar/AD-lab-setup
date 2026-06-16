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

![ipv4 config](images/1.png)


### Configuring Client IP addresses
- Windows 10
- Go to network and Internet Settings > Ethernet > Change Setting to manual > Set Ipv4 to ON > Put IP address

![ipv4 config](images/1-1.png)


<img src="images/1-1.png" alt="drawing" width="40%" height="40%"/>
  

For WIN10U1
  
![ipv4 config](images/2.png)


For WIN10A1

![ipv4 config](images/3.png)

---

# Configuring Names of all Computers

Settings > System > About > Rename this PC

For DC:

![name config](images/4.png)

For Client PCs:

![name config](images/5.png)
  
![name config](images/6.png)

---

#  
