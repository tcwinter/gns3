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

[Mikrotik switch set upl](https://github.com/tcwinter/mikrotikswitching.git)
