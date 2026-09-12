# 🔐 Cybersecurity Virtual Lab Environment Setup

Building an isolated virtual environment for cybersecurity, penetration-testing, and security experimentation.

---
## ⚠️ LIABILITY DISCLAIMER

> **Read before proceeding**

These materials are for **education and research purposes only**. Although this course includes practicals and labs, they are meant for *learning purposes only*. The aim is to show you how attacks work in real-world systems so you can defend systems better.

**Do not use anything from here to break the law.**

I am not responsible for what you do with this knowledge. Every action you take is your own responsibility.

> 🚨 Misuse can lead to criminal charges, heavy fines, loss of your job, and a permanent record. In most countries, unauthorized access is a crime **even when nothing is damaged**.

---

## ✅ Hacking is only legal when:

- 🖥️ You test a device or network that **you own**, or your **lab environment**
- 📝 You have **written and documented permission** from the owner
- 🤝 You are working as a **security professional** under a **signed agreement** with an agreed scope

---

### 🔒 Everything outside these cases is **illegal**.

---

**Environment:**  
`( ) Virtual lab   ( ) Your own devices`

☑️ *By continuing, you confirm that you have read this disclaimer and accept full responsibility for your actions.*


## 📌 Project Overview

This project focuses on building a controlled cybersecurity home lab using **Oracle VirtualBox** and multiple operating systems.

The lab consists of a dedicated virtual network containing:

* 🐉 Kali Linux — used primarily for Security testing and acts as an attacker VM
* 🪟 Windows 11 — Victim Target VM used for testing
* 🪟 Windows 10 — Victim Target VM used for testing
* 🪟 Windows 7 — Legacy Victim Target VM used for testing
* 🤖 Android — Mobile security testing VM

The purpose of the environment is to provide a controlled and repeatable platform for learning:

* Network reconnaissance
* Port scanning
* Vulnerability assessment
* Packet analysis
* Web security testing
* Operating-system security
* Exploitation practice
* Security-tool experimentation
* Network segmentation and isolation

All testing within this laboratory is performed only against systems that are owned by the lab operator or explicitly authorized for testing.

---

# 🎯 Objectives

The main objectives of this project are to:

* Install and configure **7-Zip**, where required.
* Install and configure **VirtualBox**.
* Obtain operating-system installation/virtual-machine images from legitimate sources.
* Install/import Kali Linux.
* Create an isolated **NAT Network**.
* Configure multiple virtual machines on the same virtual network.
* Configure Kali Linux as the primary security-testing machine.
* Configure Windows 11 as a modern Windows target.
* Configure Windows 10 as an additional Windows target.
* Configure Windows Server 2016 as a server target.
* Configure Windows 7 as a legacy operating-system target.
* Configure Android as a mobile-security testing target.
* Verify communication between the virtual machines.
* Verify controlled outbound connectivity where required.
* Document VM-specific configuration and edge cases.
* Create clean VM snapshots/baselines.
* Prepare the environment for future cybersecurity exercises.

---

# 🛡️ Purpose of the Lab

The laboratory provides an isolated environment for cybersecurity education and authorized security testing.

Potential activities include:

* Network reconnaissance
* Port scanning
* Vulnerability assessment
* Packet capture and analysis
* Web security testing
* Operating-system security testing
* Exploitation practice
* Malware-analysis fundamentals in a controlled environment
* Security-tool experimentation
* Network configuration and troubleshooting

> ⚠️ **Important:** This laboratory must only be used against systems that you own or have explicit authorization to test. Do not use the tools or techniques learned in this laboratory against unauthorized systems.

---

# 🏗️ Lab Architecture

The planned architecture consists of one host computer running multiple virtual machines connected to a dedicated VirtualBox NAT Network.

```text
                         HOST COMPUTER
                              │
                       Oracle VirtualBox
                              │
                    ┌─────────┴─────────┐
                    │                   │
              NAT Network           Host System
             10.0.0.0/24
                    │
       ┌────────────┼────────────┬────────────┬─────────────┬────────────────────┐
       │            │            │            │             │                    │               
       ▼            ▼            ▼            ▼             ▼                    ▼
   Kali Linux   Windows 11   Windows 10   Windows 7      Windows Server 2016   Android VM 
   Attacker       Target       Target      Legacy Target   Server Target      Mobile Target
       
```

### 📷 Lab Architecture

 ![Lab Architecture](images/1-screenshot-title-image.png)

---

# ⚙️ Lab Configuration

| 🧩 Component        | ⚙️ Configuration      |
| ------------------- | --------------------- |
| 🖥️ Host OS          | Windows 10            |
| 🧠 Host RAM         | 8 GB                  |
| ⚡ Processor        | Intel Core i7         |
| 🧰 Hypervisor       | VirtualBox 7.2        |
| 🌐 Virtual Network  | NAT Network           |
| 📡 Network Address  | 10.0.0.0/24           |
| 🚪 Default Gateway  | 10.0.0.1              |
| 🌍 DNS              | 8.8.8.8               |
| 📦 Network DHCP     | Enabled               |
| 📶 IPv6             | Optional              |
| 🐉 Kali Linux       | Kali Linux 2026.2     |
| 🪟 Windows Target 1 | Windows 11            |
| 🪟 Windows Target 2 | Windows 10            |
| 🪟 Windows Target 3 | Windows 7             |
| 🪟 Windows Target 4 | Windows Server 2016   |
| 🤖 Mobile Target    | Android               |
| 📸 VM Snapshots     | Clean baseline per VM |

> **Note:** Exact RAM, CPU, disk, IP addresses, and VM configuration may vary depending on the final laboratory setup.

---

# 🌐 Why a /24 Network?

The laboratory uses:

```text
10.0.0.0/24
```

A `/24` network corresponds to the subnet mask:

```text
255.255.255.0
```

This provides a simple and convenient private network for a small virtual laboratory.

The address range can be represented as:

```text
Network:       10.0.0.0
Usable range:  10.0.0.1 – 10.0.0.254
Broadcast:     10.0.0.255
```

A `/24` provides enough addresses for the current virtual machines while leaving substantial room for additional systems that may be added to the laboratory later.

For example:

```text
10.0.0.2   → Kali Linux
10.0.0.11   → Windows 11
10.0.0.10   → Windows 10
10.0.0.7   → Windows 7
10.0.0.16   → Windows Server 2016
10.0.0.9   → Android
```

The actual addresses may differ if DHCP is enabled.

---

# 🪜 Lab Setup Procedure

## Step 1 — Install 7-Zip

7-Zip was installed where required to extract compressed virtual-machine packages such as `.7z` archives. However, sometimes you can simply use the extract feature in Windows itself

### Tool

**7-Zip**

Official source:

https://7-zip.org/download.html


---

# Step 2 — Install VirtualBox

Oracle VirtualBox was installed as the hypervisor used to create and manage the virtual machines.

### Tool

**Oracle VirtualBox**

Official source:

https://virtualbox.org/wiki/Downloads


![VirtualBox Installation](images/11-virtualbox.png)

---

# Step 3 — Enable Hardware Virtualization

Before creating the virtual machines, hardware virtualization support was verified.

Depending on the processor and system firmware, this may appear as:

* Intel VT-x
* AMD-V
* SVM
* Hardware Virtualization

![Hardware Virtualization](images/maxresdefault.jpg)

---

# Step 4 — Create the NAT Network

A dedicated NAT Network was created in VirtualBox for the cybersecurity laboratory.

### Configuration

```text
Network Name:   NatNetwork
IPv4 Prefix:    10.0.0.0/24
DHCP:           Enabled
```

### Why NAT Network?

A NAT Network allows multiple virtual machines connected to the same virtual network to communicate with one another while providing NAT-based outbound connectivity.

This makes it suitable for a multi-machine cybersecurity laboratory. This helps students and professionals to conduct security testing among various devices and applications.

The virtual machines can therefore operate within the same controlled environment without placing the testing network directly onto the physical LAN.

![NAT Network Configuration](images/12-networknav.png)

![NAT Network Configuration](images/13-natnetwork.png)

---

# Step 5 — Configure Kali Linux

Kali Linux was installed/imported into VirtualBox.

Official source:

https://kali.org/get-kali

Make sure to select the Kali Virtual Box installation option.

The Kali VM was connected to:

```text
Adapter 1
Attached to: NAT Network
Network:     NatNetwork
```

### Configuration

```text
Operating System: Kali Linux
RAM:              2048 MB
Processors:       2
Network:          NatNetwork
```


![Kali VM Configuration](images/14-addVM.png)

![Kali Network Adapter](images/15-networkset.png)

This is how Kali Linux looks

![Kali](images/16-kali.png)

---
# Step 5.1 — Configure Kali Linux Network
Right click on the Kali taskbar and open connections and look for the 'Wired Connection 1'. It can differ depending on the VM.
Configure using the below details for the network.

![IP Set](images/17-ipaddr.png)

Confirm the change of the IP address using the below command.
# Restart `Wired connection 1`

Run the following commands separately:

```bash
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"

ifconfig
```

![IP confirm](images/21-ipset.png)

Now the IP address should be the manually configured IP address.

Finally set up a snapshot of this clean VM as a restore point if things go wrong. 

![Snapshot](images/22-snapshot.png)

# Step 6 — Configure Windows 11 VM

Windows 11 was installed/configured as a modern Windows target system.

The VM was connected to the same laboratory NAT Network.


The installation and configuration process included checking:

* Virtual hardware compatibility
* Network connectivity
* Display configuration
* Storage
* VM resource allocation
* Network adapter configuration
* Snapshot creation

First, download the proper Windows 11 ISO file from Microsoft.
![Windows 11](images/5-windows.png)

Once the ISO is downloaded, set the VM for Windows 11 by the following.
![Windows 11 VM](images/23-vmset-win11.png)

Remember to select the downloaded Windows 11 ISO image and disable unattended installation

Set the hardware configurations as below.

![Windows 11 Hardware 1](images/24-hardware1.png)
![Windows 11 Hardware 2](images/25-hardware2.png)

Remember to set the network also for the Windows VM as follows.
![Windows 11 Network](images/26-netsetwin.png)

Finally, start the VM and boot into the Windows installation tool and continue on with the installation similar to a normal Windows installation.
Make sure to select "Windows 11 Professional" as preferred edition.

![Windows 11 Install](images/28-win11install.png)

# Step 6.1 — Configure Windows 11 Network
It is important to configure the network connection of Window 11 VM manually to be part of the isolated NatNetwork. 
Once the VM is ready, open start menu and search for "network" and open "View Network Connections".
Select the Ethernet connection, select properties, select IPv4, and click properties. 

Follow the steps below.
![Windows 11 Network](images/37-networkConnwin11.png)
![Windows 11 Network](images/38-check_properties.png)
![Windows 11 Network](images/39-protocolselect.png)

Set the following configuration
![Windows 11 Network](images/40-Win11Ipconfig.png)

Finally, confirm the connection by opening CMD and checking the connection as below.

![Windows 11 Network](images/41-verifyconnWin11.png)

---

# Step 7 — Configure Windows 10 VM

Windows 10 was configured as an additional Windows target.
Windows 10 can be installed the same way as the steps followed for Windows 10. 

Follow the below steps to download the ISO officially from Microsoft.
![Windows 10 Install](images/6-Windows10.png)
![Windows 10 Install](images/7-Windows10.png)
![Windows 10 Install](images/8-Windows10.png)
![Windows 10 Install](images/9-windows10iso.png)

Virtual Box setup for Windows 10 can be followed similarly to Windows 11 as below.
![Windows 10 VBox](images/29-win10vbox.png)
![Windows 10 VBox](images/30-win10natset.png)
![Windows 10 VBox](images/31-win10setup.png)

Follow the installation tool once inside the VM, and install Windows cleanly similar to normal Windows installation.
Make sure to select "Windows 10 Professional" as preferred edition.

![Windows 10 VBox](images/32-win10desktop.png)


The VM was configured and connected to the same isolated virtual network.

# Step 7.1 — Configure Windows 10 Network
It is important to configure the network connection of Window 10 VM manually to be part of the isolated NatNetwork. 
Once the VM is ready, open start menu and search for "Control Panel" and open "Network and Internet" in Control Panel then move to Network and Sharing Center.
Select the Ethernet connection, select properties, select IPv4, and click properties. 

![Windows 10 Network](images/46-win10netset.png)

Set the following configuration
![Windows 10 Network](images/47-win10config.png)

Finally, confirm the connection by opening CMD and checking the connection as below.

![Windows 10 Network](images/48-networkcheckwin10.png)

---
# Step 8 — Configure Windows 7 VM

Windows 7 was configured as a legacy Windows target.

Because Windows 7 is an older operating system, additional compatibility and configuration issues may occur compared with modern Windows versions.

Areas checked included:

* VM compatibility
* Boot configuration
* Network adapter
* Driver support
* Display configuration
* Network connectivity
* Snapshot/recovery state

Similar to the Setup of Windows 11 and Windows 10, use Windows 7 ISO file to setup Windows 7 in Virtual Box as a Legacy VM.
Make sure to use default settings and disable unattended installation. Make sure to select "Windows 7 Ultimate" as preferred edition.

![Windows 7 Setup](images/35-win7set.png)

### ⚠️ Security Consideration

Windows 7 is an outdated operating system and should **not** be exposed directly to untrusted networks.

It is being used here strictly as an isolated laboratory target.

# Step 8.1 — Configure Windows 7 Network

Similar to Windows 10, open start menu and search "network", Open Network and Sharing Center and select Local Area Connection in Connections and set the manual IP configurations as follow.

![Windows 7 Network](images/49-win7net.png)
![Windows 7 Network](images/50-lanconnwin7.png)
![Windows 7 Network](images/51-netconfigwin7.png)

Use the following configuration for IP address.
![Windows 7 Network](images/52-netcheckwin7.png)

---
# Step 9 — Configure Windows Server 2016 VM

A server VM was added to the lab to provide a legacy server target.

Similar to Windows 10, acquire the ISO file for Windows Server 2016 and use the ISO file to setup the Server VM on Virtual Box as follows.

![Windows Server 2016](images/33-server2016.png)

After starting the VM, follow the installation steps in the VM. Make sure to select "Standard Evaluation (Desktop Experience)" as preferred edition.

The below shows the Server Manager in Server 2016.

![Windows Server 2016](images/34-servermanager.png)

---

The Server VM was configured and tested for:

* Boot functionality
* Network adapter functionality
* IP address assignment
* Connectivity to the laboratory network
* Communication with other authorized laboratory systems


---
# Step 9.1 — Configure Windows Server 2016 Network
Go to Step 7.1, and follow the exact same steps as Windows 10 because Windows Server 2016 is based on Windows 10. 
Make sure to use the following configurations for IPv4.

![Windows Server 2016 Network](images/54-servernetconfig.png)

# Step 10 — Configure Android VM

An Android virtual machine was added to the laboratory to provide a mobile operating-system target.

The Android image was obtained from an appropriate source and configured within the virtualization environment.

```

The Android VM was configured and tested for:

* Boot functionality
* Network adapter functionality
* IP address assignment
* Connectivity to the laboratory network
* Communication with other authorized laboratory systems


---
# Step 10.1 — Configure Android VM Network

# 🖥️ Virtual Machine Network Configuration

Each laboratory VM was configured to use the dedicated NAT Network.

Example:

```text
Adapter 1
Attached to: NAT Network
Network:     NatNetwork
```

The resulting environment allows the virtual machines to communicate through the same private network.

### Example Addressing

| VM                   | Example IP  | Role             |
| -------------------- | ----------- | ---------------- |
| Kali Linux           | 10.0.0.2    | Security Testing |
| Windows 11           | 10.0.0.11   | Target           |
| Windows 10           | 10.0.0.10   | Target           |
| Windows 7            | 10.0.0.7    | Legacy Target    |
| Windows Server 2016  | 10.0.0.16   | Legacy Target    |
| Android              | 10.0.0.9    | Mobile Target    |

> **Note:** These addresses are manually configured instead of using a DHCP Assignment. DHCP can be used, but it will not be convenient for a Home Lab.

---

# 🔎 Lab Verification

After configuring the virtual machines, network connectivity was verified.

## Kali Linux

### Check IP address

```bash
ip addr
```

Expected result:

```text
10.0.0.x/24
```

### Test gateway

```bash
ping 10.0.0.1
```

### Test Internet connectivity

```bash
ping 8.8.8.8
```

### Test DNS resolution

```bash
nslookup networkwalks.com
```

### Verify Nmap

```bash
nmap --version
```

---

# 🔍 Inter-VM Connectivity Testing

The laboratory network was tested to verify communication between authorized virtual machines.

For example:

```text
Kali → Windows 11
Kali → Windows 10
Kali → Windows 7
Kali → Android
```

The objective is to confirm that the virtual machines are located on the intended private network.

### Example

From Kali:

```bash
ping <target-ip>
```

The IP address of a target can also be identified using appropriate network-discovery tools within the laboratory.

### 📷 Screenshot

> **Image Placeholder:**
> `![Inter-VM Connectivity](images/15-inter-vm-connectivity.png)`

---

# 🔎 Network Discovery

Once the laboratory machines were connected to the same virtual network, basic network discovery was performed from Kali Linux.

Example:

```bash
nmap -sn 10.0.0.0/24
```

This can be used to identify active hosts within the authorized laboratory network.



---

# 📸 VM Snapshots

After completing the initial configuration of each VM, a clean baseline snapshot was created.

Example snapshot names:

```text
Kali - Clean Baseline
Windows 11 - Clean Baseline
Windows 10 - Clean Baseline
Windows 7 - Clean Baseline
Android - Clean Baseline
```

Snapshots provide a recovery point before performing experiments that may modify or damage the VM.

### 📷 Screenshot

> **Image Placeholder:**
> `![VM Snapshots](images/17-vm-snapshots.png)`

---

# 🐞 Problems Encountered & Solutions

Documenting configuration problems and their solutions is part of the laboratory process.

## Problem 1 — Hardware Virtualization Disabled

### Symptom

VirtualBox failed to start a virtual machine because hardware virtualization was unavailable.

### Solution

1. Restarted the computer.
2. Entered BIOS/UEFI settings.
3. Enabled hardware virtualization.
4. Saved the configuration.
5. Restarted the computer.
6. Started the VM again.

---

## Problem 2 — Network Connectivity After Static Configuration

After manually configuring IPv4 settings, network connectivity may fail depending on the Kali Linux NetworkManager configuration.

One workaround used during the laboratory was:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```

The connection was then restarted/rebooted and connectivity was tested again.

> ⚠️ Connection names may differ between systems. Always identify the actual NetworkManager connection name before modifying it.

---

## Problem 3 — Windows 7 Compatibility

Older operating systems may encounter compatibility issues with modern virtualization environments.

Potential areas requiring troubleshooting include:

* Boot configuration
* Storage controller
* Network adapter
* Display adapter
* Guest drivers
* Hardware virtualization

The configuration was adjusted as required to successfully boot and network the VM.

---

## Problem 4 — Android Virtualization Compatibility

Android virtualization can differ depending on the selected Android image and virtualization platform.

Potential issues include:

* Boot failure
* Network adapter compatibility
* Missing drivers
* Display problems
* Incorrect network configuration

The Android VM was tested and adjusted accordingly.

---

# 💡 What I Learned

Through this project, I gained practical experience with:

### 1. Virtualization

Understanding how a hypervisor can be used to create multiple isolated operating-system environments on a single physical machine.

### 2. Virtual Networking

Understanding the difference between standard NAT and NAT Network configurations and why a shared private network is useful for a multi-machine security laboratory.

### 3. IPv4 Addressing

Working with:

```text
10.0.0.0/24
255.255.255.0
```

and understanding network addresses, host addresses, gateways, and DNS.

### 4. Multi-OS Laboratory Design

Building a security-testing environment containing modern, legacy, and mobile operating systems.

### 5. Network Isolation

Understanding the importance of keeping cybersecurity experiments inside a controlled and authorized environment.

### 6. Troubleshooting

Identifying and resolving virtualization, networking, operating-system, and compatibility issues.

### 7. VM Snapshots

Creating known-good recovery points before conducting experiments.

### 8. Documentation

Learning to document configurations, commands, screenshots, errors, solutions, and observations as part of a cybersecurity project.

---

# 🔐 Security & Ethical Use

This laboratory is intended strictly for **educational and authorized security-testing purposes**.

All penetration-testing, scanning, exploitation, and security experiments performed using this environment must be limited to:

* Systems owned by the laboratory operator, or
* Systems for which explicit authorization has been provided.

The inclusion of intentionally vulnerable or legacy systems does not imply authorization to test similar systems outside this laboratory.

---

# 🔗 Tools & Resources

### 7-Zip

https://7-zip.org/download.html

### Oracle VirtualBox

https://virtualbox.org/wiki/Downloads

### Kali Linux

https://kali.org/get-kali

### Windows

Windows installation media/images should be obtained through Microsoft's official channels or other legitimate authorized sources.

### Android

Android images should be obtained through the appropriate official or legitimate project/distribution sources.


---

# 👤 Author

**[Safwan Abdurahiman Kavil]**

Cybersecurity Intern — Networkwalks

LinkedIn: **[www.linkedin.com/in/safwan-abdurahiman-kavil-sak03]**

GitHub: **[https://github.com/safwan-sk2110900]**

---

# 📌 Project Information

| Field       | Details                                            |
| ----------- | -------------------------------------------------- |
| Program     | Cybersecurity Internship — Networkwalks            |
| Week        | 01                                                 |
| Project     | Cybersecurity Virtual Lab Environment Setup        |
| Focus       | Virtualization, Networking & Multi-OS Security Lab |
| Platform    | Oracle VirtualBox                                  |
| Network     | 10.0.0.0/24 NAT Network                            |
| Security VM | Kali Linux                                         |
| Target VMs  | Windows 11, Windows 10, Windows 7, Android         |
| Server VMs  | Windows Server 2016                                |


---

## 📚 Week 01 Summary

The first stage of the internship involved building the foundation for a repeatable cybersecurity testing environment.

The completed laboratory provides a controlled virtual network containing multiple operating systems that can be used for future authorized security-testing exercises.

**Next stages:** The laboratory can be expanded with additional vulnerable applications, services, network configurations, and security-testing scenarios as future projects require.

