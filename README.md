# Virtual Network Lab

## Background

The purpose of this project was to provide hands-on experience with network configuration, troubleshooting, and virtual machine (VM) management. The goal was to develop skills relevant to the IT field by setting up and managing an isolated network for Windows and Linux virtual machines.

This project simualtes tasks including: 
- Configuring and managing network connectivity between multiple systems.
- Setting up and troubleshooting network services.
- Diagnosing connectivity issues, such as firewall restrictions and any network configuration errors.

## Goals

My goals for this project included:
- Setting up two different VM instances using Windows and Ubuntu.
- Segment the VM's network from the external internet.
- Configure the proper IP addresses and subnet mask so they would be on the same internal network.
- Use the ```ping``` command so each VM could communicate with the other.

## Steps/Process 

I chose VirtualBox as my VM emulator since it's known for being more user-friendly than QEMU, my usual emulator of choice due to its high performance. I also wanted to focus on acheiving my goals for the project with minimal troubleshooting before even booting up each VM.

First I downloaded two ISO's: one for Windows 10 and one for Ubuntu Server. I did not run into any issues installing the Windows VM. The only notable steps I took were changing the network settings to be an internal network in VirtualBox after installation and modifying the boot order instead of booting from the ISO every time. If I did not change the boot order, I would have gone through the Windows installation process everytime I booted up the VM.

Ubuntu Server's installation was a bit more involved because I had to configure the network during the installation process, unlike with Windows. I ran into a small configuration issue because when it asked for the subnet mask, I needed to input the CIDR notation of the IP address I wanted to use instead of the actual subnet mask. So instead of entering ```255.255.255.0```, I entered ```192.168.1.0/24```. For context, the IP address I used for the Ubuntu Server is ```192.168.1.1```. 

As an aside as to why I chose this subnet and IP address: When creating a local connection without any external internet connection, one should use private IP addresses. IPv4 addresses of ```192.168.x.x``` are apart of Class C private IP addresses which has the smallest set of available IP addresses compared to Class A and B. I also used the subnet mask of ```255.255.255.0```, even though the smallest subnet mask I could've used was ```255.255.255.254``` which would allow for 2 available IP addresses, giving a point-to-point (P2P) connection. I chose ```255.255.255.0``` for a couple of reasons: First if I expand the scope of the project in the future (within reason), I won't have to modify the subnet mask as it allows for 254 usable IP addresses, since ```192.168.1.0``` is the network address and ```192.168.1.255``` is the broadcast address. The other reason was because it's simpler conceptually, since ```192.168.1.255``` is the highest IP address in the subnet mask. 

Back to the steps taken to complete this lab: After installing Ubuntu, I reset the network settings through VirtualBox to use the internal network and confirmed that the network names were the same for both the Ubuntu and Windows installations. 

I booted up both VM's and could not access the internet as expected, this confirmed that I had successfully isolated the machines from the external internet. 

I ran ```ipconfig``` on my Windows VM and noticed that it was not on the same subnet as the Ubuntu VM, so I modified the IP address and subnet mask on Windows to ```192.168.1.2``` and ```255.255.255.0``` respectively. 

Now that Windows had the desired IP and subnet, I ran ```ping``` on my Windows machine to send packets my Ubuntu VM, and the output confirmed [I had successfully sent and received the 32 bytes with 0% Loss.](imgs/Windows-ipconfig-and-ping-to-ubuntu.png) 

I then attempted to ```ping``` from Ubuntu to Windows. However, the packets were not received by the Windows VM, resulting in a situation where Windows could successfully ```ping``` Ubuntu, but Ubuntu could not ```ping``` Windows. After some research, I found that by default, Windows Firewall does not allow inbound echo requests. [I was able to find out how to modify the Firewall rules to enable inbound echo requests on my Windows VM](Windows-firewall-rules.png), and afterward, [I could successfully ```ping``` Windows from my Ubuntu VM.](imgs/Ubuntu-ping-to-Windows.png)

## What I Learned

This was a very informative lab! I was able to solidify my understanding of several concepts, while picking up some new knowledge along the way. I reinforced my understanding of subnetting, private IP address ranges and their purpose, configuring static IP addresses, the role of network and broadcast addresses, and setting up VMs. In terms of new material, I learned about Windows Firewall rules, modifying IP addresses and subnet masks through VirtualBox and Windows, LAN configuration, and setting up an internal network from scratch.


## Next Steps

One of the great things about this lab is the opportunity to build off of it in the future. Here are some additional ideas I can work on with this project:
- Hosting a web server.
- Sharing files between VMs.
- Creating Powershell or Bash scripts for static IP addressing.

## TL;DR

I set up two VM's through VirtualBox, isolated them from external internet access, configured an internal network to allow them to communicate, and successfully got each VM to ```ping``` the other's IP address.

## Resources:

- [Where the Idea Came From](https://networkjourney.com/build-your-own-network-lab-a-step-by-step-guide/)
- [Private IP Address Ranges](https://ipcisco.com/lesson/private-ip-address-ranges/)
- [A Refresher on Private IP Addresses](https://www.geeksforgeeks.org/private-ip-addresses-in-networking/)
- [Subnetting Refrehser](https://www.freecodecamp.org/news/subnet-cheat-sheet-24-subnet-mask-30-26-27-29-and-other-ip-address-cidr-network-references/)
- [Ubuntu Network Configuration Issue](https://askubuntu.com/questions/1188147/installation-of-server-does-not-accept-subnet-cidr)
- [Modifying Windows IP Addresses](https://support.microsoft.com/en-us/windows/change-tcp-ip-settings-bd0a07af-15f5-cd6a-363f-ca2b6f391ace#WindowsVersion=Windows_10)
- [Windows Firewall Rules](https://www.howtogeek.com/112564/how-to-create-advanced-firewall-rules-in-the-windows-firewall/)
- [Allow Inbound Ping Requests Through Windows Defender](https://activedirectorypro.com/allow-ping-windows-firewall/)

