# Setting up gns3 for a networking lab environment

There were a few options to install gns3, but what I thought would be most convenient and work for me was to host the server on a VM in proxmox and use the same server as the gui. 

Install the server and gui. 

```
sudo add-apt-repository ppa:gns3/ppa
sudo apt update
sudo apt install gns3-gui gns3-server
```
Launch the server

![alt text](images/startserver.png)

I launched the gui and downloaded mikrotik routerOS because it is free and that is really the only reason that I chose mikrotik. Networking is networking no matter which vendor you choose and being able to implement networks on the cheap may be important depending on your organization.

Couldn't launch my device. 

![alt text](images/mikrotikqemu.png)

Recommended changes... 

![alt text](images/Qemukvm.png)

Now can launch mikrotik device 

Attempt to get into console of device. receive errors. 

```
Error constructing proxy for org-gnome.Terminal:/org/gnome/Terminal/Factory0: Failed to execute child process "dbus-launch" (No such file or directory)
```

download dbus-x11

```
apt install dbux-x11
```

Ah, finally able to console in. 

![alt text](images/mikrotikconsole.png)

First thing I did was add a user for myself and remove the default admin user.

```
user add name <name> password <password> group=full
user remove admin
```

What I would like to do is set up a fairly basic enterprise switch with a few vlans. First, we'll initialize the mikrotik device as a switch and then we'll initialize the vlans and ports with appropriate access vlans assigned. Last we enable vlan filtering.  

```
interface bridge add name=SW1

interface vlan add interface=SW1 name=PCI vlan-id=10
interface vlan add interface=SW1 name=Data vlan-id=20
interface vlan add interface=SW1 name=Voice vlan-id=30
interface vlan add interface=SW1 name=Management vlan-id=40

interface bridge port add bridge=SW1 interface=ether1 pvid=10
interface bridge port add bridge=SW1 interface=ether2 pvid=20
interface bridge port add bridge=SW1 interface=ether3 pvid=20

interface bridge add name=SW1 vlan-filtering=yes
```

Connect a couple of PCs, assign IPs and see if they can ping each other. 
![alt text](images/basictopology.png)

Assigned 10.1.1.1/8 for PC1, 10.1.1.2/8 for PC2 and 10.1.2.1/8 for PC3. Both using 10.1.0.1 as a default gateway, though there isn't one.

Some pings from PC2 showing it can reach PC3 because its in the same vlan, but not PC1 because it is not. 
![alt text](images/pings.png)

Expanded my topology a bit.
![alt text](images/expandedtop.png)

SW2 config, pretty similar to SW1. PC4 and 5 in vlan10 and PC6 in vlan20.
![alt text](images/sw2config.png)

IPs for PC 4, 5 and 6 are 10.1.1.4, 10.1.1.5 and 10.1.1.6 respectively. Here's PC5 sending pings to PC6 and PC4 to show vlans set up are working as intended.
![alt text](images/pc5pings.png)

So now to set up the trunk link between the two switches. 

