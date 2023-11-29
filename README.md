# RMIT-cybersecurity-networking
A few projects carried out on an observational task!

# LAB A: Plan and configure a basic network set-up

The topology

![image](https://github.com/Jasmine-108/RMIT-cybersecurity-networking/assets/151819725/cdbc6de7-ae90-4c51-848e-abf38facbef6)

| Device   | Device name (on Topology)    | Ports                                              | IP Adress                          | Subnet Mask                           | Default Gateway |
| :---:    | :---:                        | :---:                                              |    :---:                           | :---:                                 | :---:           |    
| PC       | PC 0 (Server)                | Ethernet                                           | 192.168.10.2                       | 255.255.255.0                         | 192.168.10.1    |
|          | PC1                          | Ethernet                                           | 192.168.20.2                       | 255.255.255.0                         | 192.168.20.1    |
| Switch   | Switch 0                     | F0/1 from PC0 <br> <br> G0/1 to Sydney's G0/0/0    | N/A                                | N/A                                   | N/A             |
|          | Switch 1                     | F0/1 from PC1 <br> <br> G0/1 to Melborune's G0/0/1 | N/A                                | N/A                                   | N/A             |
| Router   | Sydney                       | G0/0/0 <br> <br> S0/0/0                            | 192.168.10.1 <br> <br> 192.168.3.1 | 255.255.255.0 <br> <br> 255.255.255.0 | N/A             |
|          | Melbourne                    | G0/0/1 <br> <br> S0/0/1                            | 192.168.20.1 <br> <br> 192.168.3.2 | 255.255.255.0 <br> <br> 255.255.255.0 | N/A             |

**Setting up the router**

For Sydney Router:

```
enable
conf t
hostname Sydney
int g0/0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit
int s0/0/0
ip address 192.168.3.1 255.255.255.0
clock rate 64000
no shutdown
exit
```

For Melbourne Router:

```
hostname Melbourne
int g0/0/1
ip address 192.168.20.1 255.255.255.0
no shutdown
exit
int s0/0/1
ip address 192.168.3.2 255.255.255.0
no shutdown
exit
```

Setting passwords for both Routers

```
enable secret class
line vty 0 4
password cisco
transport input all
logging synchronous
login
no privilege level 15

line con 0
password cisco
logging synchronous
login
no privilege level 15
```
Set the IP's on the pc and type ipconfig to confirm the IP

**Routing protocol setup on routers**

Sydney router:
```
router rip
version 2
network 192.168.10.0
network 192.168.3.0
exit
```

Melbourne Router:
```
router rip
version 2
network 192.168.20.0
network 192.168.3.0
exit
```

**Test connectivity between PC’s
**Now we are ready to test the connectivity between the 2 PC’s.
On Sydney’s PC (PC0), go back into CMD and type the following:
ping 192.168.20.2
On Melbourne’s PC (PC0), go back into CMD and type the following:
ping 192.168.10.2

Before and after implementation of Router RIP protocol

![image](https://github.com/Jasmine-108/RMIT-cybersecurity-networking/assets/151819725/12f19905-a8a4-4727-b00b-711b91bd7f38)


SMB file sharing


![image](https://github.com/Jasmine-108/RMIT-cybersecurity-networking/assets/151819725/e90b761a-1bd7-48d8-ad31-37caea33a917)


![image](https://github.com/Jasmine-108/RMIT-cybersecurity-networking/assets/151819725/562ef9a8-5f5f-4f4f-a4de-f3aab67bba80)


# LAB C

Troubleshoot this logical topology to get PCA to ping PCB.


![image](https://github.com/Jasmine-108/RMIT-cybersecurity-networking/assets/151819725/280f2f12-c4fc-4b9d-b329-6c10303c753c)


Adressing table

![image](https://github.com/Jasmine-108/RMIT-cybersecurity-networking/assets/151819725/1709b3b7-5237-4fe3-9958-06f5a42ee8b0)

Red = Correct IP address assigned on router

- What are the routers’ hostname? How would you fix it to reflect what the diagram above say?
  - The routers did not have a set host name to fix this I entered hostname Router0 and hostname Router1 to the respective routers giving it a hostname

-  Are the IP addresses on the devices correct including the Gateway? What would you need to do to fix this?
  - The gateways for PC0 and PC1 were incorrect
  - PC0 had a gateway of 1.0.0.1 when the correct gate way is 1.0.0.3
  - PC1 had a gateway of 3.0.0.1 when the correct gate way is 2.0.0.1

- Are the links connected to the right ports? If not, how would you fix this?
  - The serial cables between router 0 and 1 are not correct this is fixed by unplugging the port and plugging it into the correct port s0/0/0
 

# LAB D: Set-up an IPv6 network between two computers

Topology

![image](https://github.com/Jasmine-108/RMIT-cybersecurity-networking/assets/151819725/cfc42c30-6192-43d0-8f70-2e864580b13d)

**Routing Table**

| Device   | Interface              | IPv6 Address                                          | Gateway            |
| :---:    | :---:                  | :---:                                                 | :---:              |
| R1       | G0/0/0 <br <br> G0/0/1 | 2001:def:cafe:1::1/64 <br> <br> 4044:abc:fed0:1::1/64 | N/A <br> <br> N/A  |
| PCA      | NIC                    | 2001:def:cafe:1::2/64                                 | 2001:def:cafe:1::1 |
| PCB      | NIC                    | 4044:abc:fed0:1::2/64                                 | 4044:abc:fed0:1::1 | 

1.	Assign an IP address to PCA.
2.	Assign an IP address to PCB.
3.	Verify that PCA can ping PCB by IPv6.

![image](https://github.com/Jasmine-108/RMIT-cybersecurity-networking/assets/151819725/8bae76f3-b79a-45da-9ae0-d9bac7a6de95)


# Lab H: Demonstrating ARP Poisoning

**METHOD**

1. Bridge the Network Adapter for both Virtual Machines.
2. On both virtual machines, set the IP address to automatic.
3. Ensure both virtual machines have Internet access.
4. Note down the IP address of your WiFi router and your Windows machine.
5. On Kali Linux, run Ettercap.
6. Turn on Ettercap. (Click the tick).
7. Click the Magnifying Glass in Ettercap.
8. Click the Host Lists button to locate the IP address of your WiFi router and Windows Machine.
9. Select the WiFi Router’s IP address and add it as Target 1.
10. Select the Windows Virtual Machine’s IP address and add it as Target 2.
11. Click the Globe button and select ARP Poisoning. Sniff for Remote Connections and click OK.
12. Go to the Windows Machine and open a Web Browser. Type http://testphp.vulnweb.com/login.php. Enter user name admin and password test then click Login.
13. Verify Kali Linux has intercepted the username and password.


![image](https://github.com/Jasmine-108/RMIT-cybersecurity-networking/assets/151819725/d4b514d7-ba9b-4013-bbdc-8b0655cb643c)

![image](https://github.com/Jasmine-108/RMIT-cybersecurity-networking/assets/151819725/e235f9fe-2c06-4ce1-a58b-b9daf733fca0)


# LAB I: Denial of Service Attack

**METHOD**

1. Assign IP address of 192.168.1.1/24 to the router’s port.
2. Set up Telnet access to the router.
3. Bridge Kali Linux to the network.
4. Assign IP address of 192.168.1.2/24 to Kali Linux attached to the router.
5. Perform a denial of service attack on a router using the hping command.
6. Attempt to Telnet to the router, it should fail.
7. Check to see how busy the processor of the router is.


Hping and failiure to telnet

![image](https://github.com/Jasmine-108/RMIT-cybersecurity-networking/assets/151819725/6b464a3b-12e1-4e06-b55d-ed1b30538edb)


CPU activity

![image](https://github.com/Jasmine-108/RMIT-cybersecurity-networking/assets/151819725/34b40fc2-092f-4754-916f-7c0dd128ac51)

