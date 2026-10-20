## INTRODUCTION

In order to demonstrate the SSH brute force end-to-end on victim machine, first we need to install the special software called virtual machine which help to emulates a physical computer, running its own operating system and applications in an isolated environment on shared hardware that helps to install multiple machines like ubuntu server, victim machine and attacker machine on single computer.
To run a mini security lab locally via VMware Workstation containing a Wazuh server, an Ubuntu server, a Victim machine, and an Attacker machine simultaneously, your hardware needs to handle running 4 virtual machines concurrently alongside host operating system. We need the following laptop/computer specification.
| Component / Machine | Operating System / Function | Minimum RAM | Recommended RAM | Storage Space | CPU Cores |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Host System** | Windows / Linux (Host OS) | **4 GB** | **8 GB** | — | Native |
| **Wazuh Server** | Ubuntu Server / All-in-One Manager + Indexer + Dashboard | **4 GB** | **6 GB – 8 GB** | **30 GB** | 2 Cores |
| **Ubuntu Server** | Ubuntu Server (Web/App Target or Agent Node) | **1 GB** | **2 GB** | **15 GB** | 1 Core |
| **Victim Machine** | Windows 10 / 11 *(or secondary Linux target)* | **2 GB** *(Linux)* / **4 GB** *(Windows)* | **4 GB** | **30 GB – 40 GB** | 1 – 2 Cores |
| **Attacker Machine** | Kali Linux | **2 GB** | **4 GB** | **25 GB** | 2 Cores |
| **TOTALS** | **All 4 VMs + Host System Running** | **13 GB – 15 GB** | **24 GB – 26 GB** | **~110 GB SSD** | **4+ Cores** |


# lets install and setup VMware Workstation Pro 25H2
In this project iam using a vmware workstation pro version 25H2. 
Download [VMware Workstation Pro 25H2](https://access.broadcom.com/) from the official Broadcom Support Portal.

## Step 1: Create or Sign In to Your Broadcom Account
VMware products are hosted and managed through the Broadcom Support Portal.  
* Go to the Broadcom Support Portal Login Page.
+ If you don't have an account, click Register and fill out your details (Email, password, name, and country).
- Verify your email with the confirmation code sent by Broadcom, then sign in

## Step 2: Download VMware Workstation Pro 25H2
* Once logged into your Broadcom dashboard, look at the left sidebar or quick links and click on My Downloads. 
* On the My Downloads page, look for the link or text that says “Free Software Downloads available HERE” and click it.  
* In the product search bar on the new page, type:
   - Workstation Pro (if you are on Windows or Linux)
   - (Mac users should search for VMware Fusion instead)   
* From the results list, click on VMware Workstation Pro.   
* Select the latest version: Workstation Pro 25H2. 
* Scroll down to accept the Terms and Conditions (you may need to click the terms link first to unlock the checkbox), fill out any required trade-compliance profile fields if prompted, and click the Download button to save the installer.

## Step 3: Prepare Your System (Pre-requisites)
Before running the installer, ensure your system meets the requirements:
* Hardware Virtualization: Ensure Intel VT-x or AMD-V is enabled in your computer's BIOS/UEFI settings. 
* RAM: At least 8 GB of RAM (16 GB or higher recommended if you plan to run multiple virtual machines for a project/homelab).
  
## Step 4: Install VMware Workstation Pro 25H2
* Double-click the downloaded .exe installer file to launch the setup wizard.
* Click Next, accept the license terms and conditions, and click Next again.
* Choose whether you want enhanced keyboard driver features or to add the console path to your system environment variables, then proceed.
*  The installer will check if Windows Hyper-V features conflict or integrate; keep the recommended settings and click Install.
*  Click Finish when the installation completes and restart your computer if prompted.   

## Step 5: Launch and Verify 
* Open your application launcher or double-click the desktop shortcut to open VMware Workstation Pro 25H2.  
* Select the option for Personal Use when prompted upon first launch.

## ADDING VICTIM MACHINE & ATTACKER MACHINE
To add your Attacker Machine (Kali Linux) and Victim Machine into VMware Workstation Pro, you will need their respective operating system installation files (ISO files) downloaded beforehand.

Here is the step-by-step process to build and configure both machines in your lab.

## Phase 1: Download Required ISO Files

- **Kali Linux (Attacker):** Download the official installer image (.iso) from [Kali Linux](https://www.kali.org/get-kali/).
- **Victim Machine:** Download an ISO for [Windows 10](https://www.microsoft.com/software-download/windows10) / [Windows 11](https://www.microsoft.com/software-download/windows11) (from Microsoft) or an alternative light Linux distro like [Ubuntu Desktop](https://ubuntu.com/download/desktop) or [Alpine Linux](https://alpinelinux.org/downloads/) depending on what you prefer to attack/defend.

## Phase 2: Add the Attacker Machine (Kali Linux)
* Open VMware Workstation Pro.
* Click on Create a New Virtual Machine (or go to File > New Virtual Machine).  
* Select Typical (recommended) and click Next. 
* Choose Installer disc image file (iso), browse to select your downloaded Kali Linux ISO file, and click Next.
* Select Linux as the guest operating system and choose Ubuntu 64-bit (or Debian 12 64-bit, as Kali is Debian-based) from the version dropdown, then click Next.Name your virtual machine (e.g., Kali-Attacker) and * choose a storage folder location on your SSD, then click Next.   
* Specify the disk capacity based on your lab design (e.g., 25 GB), select Store virtual disk as a single file, and click Next. 
* Click on Customize Hardware... before finishing:
  * Memory (RAM): Set to 2048 MB (2 GB) or 4096 MB (4 GB).
  * Processors: Set to 2 Processors with 2 Cores (or what fits your host CPU).
  * Network Adapter: Set to NAT or Host-Only (depending on how you want it to isolate or talk to your Wazuh/Victim machines).
* NAT is easiest for initial internet access.  
* Click Close and then Finish.   
* Select your new Kali VM from the left panel and click Power on this virtual machine to run through the Kali Linux text/graphical installation.

## Phase 3: Add the Victim Machine (Windows or Ubuntu Server)
* Click File > New Virtual Machine again to start the wizard for your target/victim.
*  Choose Typical and click Next.
*  Select Installer disc image file (iso) and browse to your Windows 10/11 or ubuntu server or target OS ISO file, then click Next.
*  Enter your Windows product key (or leave it blank if you are using an evaluation/trial ISO), select the version, and fill out username/password credentials for Easy Install. Click Next.
* Name the machine (e.g., Windows-Victim) and select its storage path, then click Next.
* Allocate disk space (e.g., 40 GB for Windows) and choose Store virtual disk as a single file, then click Next.
* Click Customize Hardware...:
   * Memory (RAM): Allocate 4096 MB (4 GB) for Windows (or 2048 MB if it's a lightweight Linux victim).
   * Processors: Assign 2 Cores.
   * Network Adapter: Ensure it matches the same network connection type (e.g., NAT or a custom LAN Segment) as your Attacker and Wazuh server so they can communicate with each other.
* Click Close and Finish to initialize the VM installation.

## Phase 4: Essential Next Step (Take Snapshots)
Once both machines are successfully installed, updated, and configured:
* Right-click each virtual machine tab on the left sidebar.
* Go to Snapshot > Take Snapshot...
* Name it Clean Install State
