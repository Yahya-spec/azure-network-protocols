<p align="center">
  <img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this home lab, I observed different networking activities and traffic using multiple ports such as ICMP, SSH, RDP, and DNS through the use of Wireshark and PowerShell on Windows and Linux VMs.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used</h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>

<h3>Part 1: Create Our Resources</h3>
1. Create a Resource Group.
2. Create a Windows 10 Virtual Machine (VM).
3. While creating the VM, select the previously created Resource Group:  
 
![r1](https://github.com/user-attachments/assets/8c12e457-4e9d-4c62-afea-0b566f0d4d4d)
4. Allow the creation of a new Virtual Network (Vnet) and Subnet:  
  
![r2](https://github.com/user-attachments/assets/f93823f1-e760-4828-b841-42e5e271f2bf)
5. Create a Linux (Ubuntu) VM:  
  
![r3](https://github.com/user-attachments/assets/44662103-2d2b-445e-91d1-a3be63c776de)
6. While creating the VM, select the previously created Resource Group and Vnet:  
 
![6a](https://github.com/user-attachments/assets/9276c3c4-011f-4f15-8306-523099565d5c)
  
![7a](https://github.com/user-attachments/assets/c56ca154-3ebf-402e-a14d-2b59611fc362)
7. Observe your Virtual Network within Network Watcher.

<h3>Part 2: Observe ICMP Traffic</h3>
1. Use Remote Desktop to connect to your Windows 10 Virtual Machine.
  
![8a](https://github.com/user-attachments/assets/4eaa7662-d0bb-4125-a1cc-3214b2a5ab80)
2. Install Wireshark on the Windows 10 VM.
 
   ![image](https://github.com/user-attachments/assets/76b18670-cb2d-4dfe-a955-3cf23ad2c764)
3. Open Wireshark and filter for ICMP traffic only.  
  
   ![image](https://github.com/user-attachments/assets/f694f4d6-4c6a-4072-ac9d-3f4745f7c0d6)  
  
   ![image](https://github.com/user-attachments/assets/3295c8d5-cf75-42d3-bc92-337971734a38)
4. Retrieve the private IP address of the Ubuntu VM and attempt to ping it from within the Windows 10 VM.
   - Observe ping requests and replies within Wireshark.  
 
   ![image](https://github.com/user-attachments/assets/04d699d2-65f8-4bad-8d69-075962fd372d)
5. From the Windows 10 VM, open Command Line or PowerShell and attempt to ping a public website (such as www.google.com) and observe the traffic in Wireshark.  
 
   ![image](https://github.com/user-attachments/assets/a75193f9-32c2-4a42-b0bb-a8018f1c5db5)
6. Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM.  

   ![image](https://github.com/user-attachments/assets/704346b0-8e9c-45d6-b3e6-aeb3858092b9)
   - Open the Network Security Group (NSG) associated with your Ubuntu VM and disable incoming (inbound) ICMP traffic.  
  
![57](https://github.com/user-attachments/assets/6e2e4886-7ad1-49cd-b1e6-5d48d80f0c81)
   - Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command-line ping activity.
   - Re-enable ICMP traffic for the Network Security Group your Ubuntu VM is using.
   - Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command-line ping activity (it should resume).
   - Stop the ping activity.

<h3>Part 3: Observe SSH Traffic</h3>
1. In Wireshark, filter for SSH traffic only.
2. From your Windows 10 VM, "SSH into" your Ubuntu VM (via its private IP address).
   - Type commands (username, password, etc.) into the Linux SSH connection and observe SSH traffic in Wireshark.
   - Exit the SSH connection by typing 'exit' and pressing [Enter].  
  
   ![image](https://github.com/user-attachments/assets/98506428-2d79-4886-b8b1-1cf7e171036a)

<h3>Part 4: Observe DHCP Traffic</h3>
1. In Wireshark, filter for DHCP traffic only.
2. From your Windows 10 VM, attempt to issue your VM a new IP address using the command `ipconfig /renew` and observe the DHCP traffic in Wireshark.  

   ![image](https://github.com/user-attachments/assets/9549dbae-fa51-4243-bb07-bc031f601cd5)
   
<h3>Part 5: Observe DNS Traffic</h3>
1. In Wireshark, filter for DNS traffic only.
2. From your Windows 10 VM, within the Command Line, use `nslookup` to resolve a website's IP address and observe the DNS traffic in Wireshark.  
  
   ![image](https://github.com/user-attachments/assets/0e32494b-3551-46d3-8b39-44f89a8f563d)

<h3>Part 6: Observe RDP Traffic</h3>
1. In Wireshark, filter for RDP traffic only (`tcp.port == 3389`).
2. Observe the immediate non-stop traffic. Why is it non-stop versus only showing traffic when an activity occurs?
   
   RDP traffic is constant because it streams the live display of one computer to another. Unlike SSH, where traffic is generated only when keystrokes are sent, RDP transmits a constant stream of data for the remote session.  

   ![image](https://github.com/user-attachments/assets/dcbdcecc-4847-4cdf-813c-a623185a4b8a)

<h2>Takeaways and Key Skills Developed</h2>
In this lab, I explored network security and traffic monitoring between Azure VMs using various protocols and tools such as Wireshark and PowerShell. I created both Windows and Ubuntu VMs, observing traffic for ICMP, SSH, DNS, RDP, and DHCP. By filtering traffic in Wireshark, I was able to understand how different protocols behave, such as viewing ICMP traffic during ping tests, SSH traffic during remote connections, and RDP traffic for remote desktop sessions. I also configured Network Security Groups (NSGs) to control traffic, such as blocking ICMP and monitoring its impact in real-time. This lab enhanced my understanding of network security practices, traffic analysis, and the configuration of security measures in a cloud environment.
