---
layout: post
title: "Building a Starter HomeLab"
date: 2023-03-22 12:00:00 -500
categories: homelab
tags: homelab splunk pfsense securityonion windows linux
pin: true
image:
    path: /assets/img/headers/StarterVirtualHomeLab.webp
---


---
In Cybersecurity, it could be a daunting task to apply and implement security concepts if there is an unavailability of practical and safe infrastructure to carry out these activities. The quickest and simplest way to fix this issue is by creating a Home Lab to safely conduct or practice operations outside of a business environment.

## **What is a HomeLab?**

A home lab is a personal computing environment used for experimentation, learning, and testing new technologies. It typically consists of a collection of computer hardware and software that is used by enthusiasts, hobbyists, students, and professionals to develop and test new software applications, network configurations, virtualization setups, and other types of technology projects.

Home labs are often used by individuals who are interested in pursuing a career in the technology industry or who want to gain hands-on experience with various technologies outside of a formal educational setting. They can range from a simple setup with a single computer to complex networks with multiple servers and virtual machines.

Home labs can be useful for a variety of purposes, including testing new software, practicing system administration skills, learning about cybersecurity, experimenting with virtualization, and exploring different networking configurations. They can also be used to run personal projects, such as web servers, media servers, or game servers.

This is a project that keeps a simple approach while also containing key components necessary for network detection and monitoring practice. This environment is meant to provide a safe place to improve field-related skills while simulating a enterprise style architecture. In this repository, there will be quick and easy-to-understand walkthroughs of how to set up the components of this home lab. 

---

## **What does this Starter HomeLab include?**

- Using VMware Workstation Pro 17 (or VirtualBox) as the "Hypervisor"
- Configuring a pfSense firewall for Network Segmentation & Security
- Configuring Security Onion as an all-in-one IDS, Security Monitoring, and Log Management solution
- Configuring Kali Linux as an attack machine
- Configuring a Windows Server as a Domain Controller
- Configuring Windows desktops
- Configuring Splunk
- Additional Ubuntu/CentOS/Metasploitable/DVWA/Vulnhub machines (Exploitable Network Machines)

---

## **Walkthroughs**

**These walkthroughs assume you already know how to set up and create a virtual machine within the virtualization application of your choice (VirutalBox or VMware Workstation).*

---

### **Kali Linux Setup**

Kali Linux ISO File Download Link : 

<https://www.kali.org/get-kali/#kali-virtual-machines>

Recommended VM Settings are :

-  Disk Size : 80 GB (Store Virtual Disk as a Single File)
-  Memory : 4 GB or 4,096 MB
-  Processors : 4

Additional Network Settings :

-  Network Adapter 1 : NAT
-  Network Adapter 2 : VMnet2

*All other settings can be left at default.*

---

### **PfSense Setup**

PfSense ISO File Download Link : 

<https://www.pfsense.org/download/>

Recommended VM Settings are :

-  Disk Size : 20 GB (Split Virtual Disk into Multiple Files)
-  Memory : 2 GB or 2,048 MB

Additional Network Settings :

-  Network Adapter 1 : NAT
-  Network Adapter 2 : VMnet2
-  Network Adapter 3 : VMnet3
-  Network Adapter 4 : VMnet4
-  Network Adapter 5 : VMnet5
-  Network Adapter 6 : VMnet6

*All other settings can be left at default.*

#### **Software Configurations**

Accept all defaults and allow pfSense reboot. More specifically this means clicking Enter through the options until pfSense auto reboots.

Once the pfSense reboots with the default configured settings, you should come to a screen similar to this :

![pfSenseInterfaces](/assets/img/posts/starter-homelab/pfSenseInterfaces.webp)

From here, type `1` to select 1) Assign Interfaces after the Enter an option: prompt.

It will ask you if VLANs should be set up now, type `N` for no.

Next,

-  Type `em0` to assign it to the WAN interface
-  Type `em1` to assign it to the LAN interface
-  Type `em2` to assign it to the Optional 1 interface
-  Type `em3` to assign it to the Optional 2 interface
-  Type `em4` to assign it to the Optional 3 interface
-  Type `em5` to assign it to the Optional 4 interface

Then type `Y` to proceed. 

> These changes will be made and the menu from the screenshot above should pop back up. 
{: .prompt-info }

**LAN Interface**

From here, type `2` to select 2) Set interface(s) IP address after the Enter an option: prompt.

Start with picking the LAN interface which is number 2.

-  Assign `192.168.1.1` as the LAN IPv4 Address.
-  Assign `24` as the new LAN IPv4 Subnet Bit Count.
-  Click `Enter` for both of the next options (IPv4 Upstream Gateway and IPv6).
-  Type `Y` to enable the DHCP server on LAN.
-  Set the start address of the IPv4 range as `192.168.1.11`.
-  Set the end address of the IPv4 range as `192.168.1.200`.
-  Type `N` to not revert to HTTP as the webConfigurator protocol.

**OPT1 Interface**

From here, type `2` again to select 2) Set interface(s) IP address after the Enter an option: prompt.

Start with picking the OPT1 interface which is number 3.

-  Assign `192.168.2.1` as the LAN IPv4 Address.
-  Assign `24` as the new LAN IPv4 Subnet Bit Count.
-  Click `Enter` for both of the next options (IPv4 Upstream Gateway and IPv6).
-  Type `N` to enable the DHCP server on LAN.
-  Type `N` to not revert to HTTP as the webConfigurator protocol.

**OPT2 Interface**

From here, type `2` again to select 2) Set interface(s) IP address after the Enter an option: prompt.

Start with picking the OPT2 interface which is number 4.

-  Assign `192.168.3.1` as the LAN IPv4 Address.
-  Assign `24` as the new LAN IPv4 Subnet Bit Count.
-  Click `Enter` for both of the next options (IPv4 Upstream Gateway and IPv6).
-  Type `N` to enable the DHCP server on LAN.
-  Type `N` to not revert to HTTP as the webConfigurator protocol.

**OPT3 Interface**

Leave **OPT3 Interface** without an IP address as it is going to have traffic that will be monitored by Security Onion.

**OPT4 Interface**

From here, type `2` again to select 2) Set interface(s) IP address after the Enter an option: prompt.

Start with picking the OPT4 interface which is number 6.

-  Assign `192.168.4.1` as the LAN IPv4 Address.
-  Assign `24` as the new LAN IPv4 Subnet Bit Count.
-  Click `Enter` for both of the next options (IPv4 Upstream Gateway and IPv6).
-  Type `N` to enable the DHCP server on LAN.
-  Type `N` to not revert to HTTP as the webConfigurator protocol.

After all those changes to each interface, the interface list at the main menu should look like this :

![pfSenseInterfaceList](/assets/img/posts/starter-homelab/pfSenseInterfaceList.webp)

The WAN Interface IP address will most likely be different so don't panic :)

PfSense is safe to shut down from here by typing `6` to Halt System.

This marks the end of the configurations on the PfSense virutal machine. Additional changes will come through the Kali Linux Virtual Machine during the section of steps below. 

#### **Web Configurations**

Start up your Kali virtual machine for this next part.

Once the Kali Linux machine is started, you can use the default credentials (if not yet changed).

-  Username : `kali`
-  Password : `kali`

Navigate to the web browser and search <https://192.168.1.1>

If successfully connected to the pfSense firewall, a screen like the one below should show up :

![pfSenseWebWarning](/assets/img/posts/starter-homelab/pfSenseWebWarning.webp)

Click the Advanced... button and a new screen confirming to accept the risk will pop up.

Click the Accept the Risk and Continue button. 

The pfSense screen matching the one below will appear :

![pfSenseWebLogin](/assets/img/posts/starter-homelab/pfSenseWebLogin.webp)

Sign into this pfSense login page with the default credentials.

-  Username : `admin`
-  Password : `pfsense`

Click Next on the initial screen to start the pfSense web setup and a bar saying Step 1 of 9 should appear at the top.

Click Next again to proceed to Step 2 of 9.

-  Leave the Hostname and Domain boxes empty.
-  Set `8.8.8.8` as the Primary DNS Server.
-  Set `4.4.4.4` as the Secondary DNS Server.
-  Leave the Override DNS checkbox checked.

Click Next to proceed to Step 3 of 9.

-  Leave the Time Server Hostname box empty.
-  Set Timezone to your specific timezone.

Click Next to proceed to Step 4 of 9.

-  Leave all options listed as default except the last two checked boxes.
-  Uncheck the last two boxes.

![pfSenseUntickOptions](/assets/img/posts/starter-homelab/pfSenseUntickOptions.webp)

Click Next to proceed to Step 5 of 9.

-  Leave all options default.

Click Next to proceed to Step 6 of 9.

-  Set a new Admin Password. **(Make sure to label and write this down!)**

Click Next to proceed to Step 7 of 9.

Click Reload to proceed to Step 8 of 9.

Click Finish to complete the web configuration process.

After the web setup process is complete, the pfSense dashboard will now be available.

![pfSenseDashboard](/assets/img/posts/starter-homelab/pfSenseDashboard.webp)

Next, configurations need to be made to the interfaces.

Click on the Interfaces tab and select LAN on the dropdown menu.

For interface LAN, change the description from LAN to `Kali`. (This is to label the interfaces of what devices are running on what)

Click Save at the bottom.

Repeat this process for all other interfaces (excluding em0/WAN interface).

-  Keep WAN as `WAN`.
-  Change LAN to `Kali`.
-  Change OPT1 to `VictimNetwork`.
-  Change OPT2 to `SecOnion`.
-  Change OPT3 to `SpanPort`.
-  Ensure that OPT3 or now SpanPort interface is enabled.
-  Change OPT4 to `Splunk`.

To confirm these changes, the interface list should match the screenshot below :

![pfSenseWebInterfaceList](/assets/img/posts/starter-homelab/pfSenseWebInterfaceList.webp)

While in the Interfaces Assignment list, switch to the Bridges tab.

Click Add.

Select VictimNetwork.

![pfSenseDisplayAdvanced](/assets/img/posts/starter-homelab/pfSenseDisplayAdvanced.webp)

Click Display Advanced.

Scroll down to the Advanced Configuration Section and select SPANPORT for Span Port.

Then click Save at the bottom of the page.

Now click the Rules option in the dropdown menu of the Firewall tab.

Click the Add button with the arrow that points downward.

Under the Edit Firewall Rule section, select Any for the Protocol rule and then click Save at the buttom of the page.

This concludes all of the setups and configurations made to the pfSense firewall for this home lab walkthrough.

---

### **Security Onion Setup PT.1**

Security Onion ISO File Download Link : 

<https://github.com/Security-Onion-Solutions/securityonion/blob/master/VERIFY_ISO.md>

Recommended VM Settings are :

-  Version : Linux (CentOS 7 64-bit)
-  Disk Size : 200 GB (Store Virutal Disk as a Single File)
-  Memory : 15 GB or 16,384 MB
-  Processors : 4

Additional Network Settings :

-  Network Adapter 1 : NAT
-  Network Adapter 2 : VMnet4
-  Network Adapter 3 : VMnet5

*All other settings can be left at default.*

#### **Software Configurations**

Once the initial stages of loading are complete, type "yes" when prompted with this screen :

![SecOnionInstallPrompt](/assets/img/posts/starter-homelab/SecOnionInstallPrompt.webp)

Set a `username` and `password` when prompted and wait until Security Onion reboots.

When Security Onion reboots, login and a setup screen like the one below will pop up. Click `<Yes>` to this prompt and proceed.

![SecOnionSetupPrompt](/assets/img/posts/starter-homelab/SecOnionSetupPrompt.webp)

Click `Enter` while highlighting the `Install` option.

Select the `EVAL` option by using spacebar and then click `Enter`.

Type "AGREE" and accept the Elastic License to continue.

Set the `Hostname` to "SecOnion" or whatever is best to identify this machine and click `Enter`.

Select `ens32` displayed in the screenshot below using the spacebar again and click `<Ok>`.

![SecOnionENS1](/assets/img/posts/starter-homelab/SecOnionENSManagement.webp)

Select `DHCP`, then `<Ok>`.

Click `Enter` to confirm and `Enter` again to continue.

Select `Standard` on the prompt seen below and click `Enter`.

![SecOnionStandard](/assets/img/posts/starter-homelab/SecOnionStandard.webp)

Select `Direct`, click `Enter`, and let the configurations load.

Select `ens34` displayed in the screenshot below using the spacebar and click `<Ok>`.

![SecOnionENS2](/assets/img/posts/starter-homelab/SecOnionENSMonitor.webp)

Select `Automatic` for the OS patch schedule and click `<Ok>`.

Leave the home networks listed in the text box as default and click `<Ok>`.

Select all options in the screenshot below using the spacebar and click `<Ok>`.

![SecOnionServices](/assets/img/posts/starter-homelab/SecOnionServices.webp)

Click `<Yes>` to keeping the default Docker IP range.

Enter an email address and password for your administrator account and click `<Ok>`.

Select `IP` for web interface access and click `<Ok>`.

Select `<Yes>` to configure NTP servers.

Click `<Ok>` to defualt input for NTP servers to use.

Select `<No>` to running `so-allow` for now.

I recommend noting or screenshotting the options set screen seen below for future reference before clicking `<Yes>`.

![SecOnionSetupFinal](/assets/img/posts/starter-homelab/SecOnionSetupFinal.webp)

Proceed and wait for Security Onion to initialize all the configuration steps. (This may take a while)

---

### **Ubuntu Setup**

Ubuntu ISO File Download Link : 

<https://ubuntu.com/download/desktop>

Recommended VM Settings are :

-  Disk Size : 20 GB (Split Virtual Disk into Multiple Files)
-  Memory : 15 GB or 16,384 MB
-  Processors : 4

Additional Network Settings :

-  Network Adapter 1 : NAT

*All other settings can be left at default.*

#### **Configurations**

Start up the Ubuntu virtual machine. Once logged in, the home screen should appear :

![UbuntuDesktop](/assets/img/posts/starter-homelab/UbuntuDesktop.webp)

Open up the `Terminal` in Ubuntu and run "ifconfig" so that the information below displays :

![UbuntuTerminal](/assets/img/posts/starter-homelab/UbuntuTerminal.webp)

*If an error appears referring to `net-tools` run this command :

``` Terminal
sudo apt install net-tools 
```

**Once the information appears, take note of the IP address listed.**

### **Security Onion Setup PT.2**

Click back onto the security onion host and log back in if not already.

Type this command :

``` Terminal
sudo so-allow
```

Confirm your login password to proceed.

Next type "a" to select the first option.

Enter in the IP address of your Ubuntu machine that was noted in the previous screenshot and wait for it to update. This tells security onion to allow web interface access from that Ubuntu machine.

If everything was done right, your screen should look like the one below :

![SecOnionSoAllow](/assets/img/posts/starter-homelab/SecOnionSoAllow.webp)

Click back on the Ubuntu machine and open the web browser.

Type the security onion host IP address into the address bar. This can be found in the options screenshot recommendation from the last step of the security onion setup section.

Once requested, the website with a risk warning should appear like the screenshot below :

![SecOnionWebWarning](/assets/img/posts/starter-homelab/SecOnionWebWarning.webp)

Click the `Advanced...` button and a new screen confirming to accept the risk will pop up.

Click the `Accept the Risk and Continue` button. The security onion login screen matching the one below will appear :

![SecOnionWebLogin](/assets/img/posts/starter-homelab/SecOnionWebLogin.webp)

Log in using the security onion login created previously and the dashboard containing security alerts can now be accessed like shown below :

![SecOnionWebDashboard](/assets/img/posts/starter-homelab/SecOnionWebDashboard.webp)

This concludes the security onion setup.

---

### **Windows Server 2019 with Active Directory Setup**

Windows Server 2019 ISO File Download Link : 

<https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019>
    
Recommended VM Settings are :

-  Disk Size : 60 GB (Default)
-  Memory : 2 GB or 2,048 MB (Default)

Additional Network Settings :

-  Network Adapter 1 : VMnet3

*All other settings can be left at default.*

## Windows Server Setup and Domain Configuration

Power on the virtual machine and immediately click any key to boot from drive.

Click `Next`.

Click `Install Now`.

A screen like the one below should pop up listed with 4 options :

-- Insert Picture --

Select the `Windows Server 2019 Standard Evaluation (Desktop Experience)` and click `Next`.

Accept the License Terms.

Click `Next`.

Select `Custom: Install Windows only (advanced)`.

Click `New` with the yellow sun symbol and then `Apply`. The screen should then look like the visual below :

-- Insert Picture --

Click `Ok`.

Click `Next` and a window with installation status should appear like the one below :

-- Insert Picture --

When the installation is complete, create a password for the Administration account and the machine should reboot.

Log in using the Administrator account details in the screen below :

-- Insert Picture --

After logging in, the dashboard of the server manager should automatically pop up as displayed below :

-- Insert Picture --

Next the new server needs to be renamed. Navigate to `Settings` using the windows search bar.

Search "pc name" in the settings search bar.

The about page in settings will appear. Click the `Rename this PC` button.

Type a name to identify this PC as and click `Next`. The screen should've look similar to the one below :

-- Insert Picture --

After the server automatically reboots, the server manager will pop back up.

Up in the top right corner of the server manager, Click `Manage` for a dropdown menu so your screen looks like the screen below :

-- Insert Picture --

Select `Add Roles and Features` so a new windows pops up.

Click `Next` until you get to the `Server Roles` section of the wizard.

Check the `Active Directory Domain Services` box in the long list and click `Add Features` like in the screenshot below :

-- Insert Picture --

Click `Next` until you get to the `Confirmation` section of the wizard.

Now click `Install`

An installation progress screen will show the status of the installation until completetion like the one below :

-- Insert Picture --

After the install, click `Close`.

In the server manager, click the flag buton with the yellow caution triangle in the top right corner.

Click `Promote this server to a domain controller`

An Active Directory Domain Services Configuration Wizard should appear.

In the `Deployment Configuration` section, select `Add a new forest`.

Choose a domain name like the screenshot below :

-- Insert Picture --

Click `Next` and set a domain forest password.

Click the `Next` button until you get to the `Prerequisites Check` section.

Wait for the check to register which will display some yellow cautions in the results box like the display below :

-- Insert Picture --

Click `Install` and wait for the reboot.

After the reboot, log back in.

Select the `Manage` button in the top right again and select `Add Roles and Features`.

Click `Next` until you get to the `Server Roles` section.

Check the `Active Directory Certificate Services` box in the long list and click `Add Features` like in the screenshot below :

-- Insert Picture --

Click `Next` until you get to the `Confirmation` section of the wizard.

Check the `Restart the destination server automatically if required` box and click `Yes`.

Click `Install`.

After the installation, click `Close` seen in the screenshot below :

-- Insert Picture --

In the server manager, click the flag buton with the yellow caution triangle in the top right corner.

Click `Configure Active Directory Certificate Services on the destination server` under the `Post-deployment Configuration` alert.

An AC CS Configuration Wizard should appear.

Click `Next` through the `Credentials` section.

Within the `Roles Services` section, check the `Certification Authority` box.

Click `Next` until you get to the `Validity Period` section. Change the number to `99 Years` like the screenshot below :

-- Insert Picture --

Click `Next` until the `Confirmation` section. Click the `Configure` button, then `Close`.

Manually reboot the server using the Windows button for all changes to take effect.

Log back into the server.

In the top right of the server manager, click the `Tools` tab to view the dropdown menu.

Select `Active Directory Users and Computers` to open manager.

Click the arrow next to the domain name on the directory list on the left side of the manager.

Right click on the `Users` folder, hover over `New`, and select `User`.

Your screen should match the screenshot below before clicking :

-- Insert Picture --

Enter a First, Last, and User Logon name for the new user like the window below :

-- Insert Picture --

(In `User logon name:`, type the "-WIN10" after the initials of the user)

Click `Next` when finished.

Assign a password to the new account and check the `Password never expires` box. Click `Next`.

Click `Finish`.

Right click the new user created and click copy like the screenshot below :

-- Insert Picture --

Created another user with the same method as the first. Instead of adding "-WIN10", add "-WIN7" after the initials.

Now, go to the Windows search bar and search "Windows Defender Firewall".

Click `Turn Windows Defender Firewall on or off`. Turn the firewall off for all networks to match the screenshot below :

-- Insert Picture --

Click `OK`.

The last configuration to the domain is to set the default gateway to the PfSense firewall.

Go to the Windows search bar and search "Control Panel".

Once the windows opens, click `Network and Internet`.

Click `View Network Connections`.

Right click on `Ethernet0` and select `Propterties`.

Double click on `Internet Protocol Version 4 (TCP/IPv4)`.

Enter the following and match the screenshot below :

-- Insert Picture --

Click `OK` twice and reboot the server to update the changes.

---

### **Vulnerable Windows 10 Hosts**

Filler

---

### **Splunk Walkthrough**

Filler

---