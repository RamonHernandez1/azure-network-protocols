<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we explore network traffic between Azure Virtual Machines using Wireshark, focusing on different protocols such as ICMP, SSH, DHCP, DNS, and RDP. We also experiment with Network Security Groups (NSGs) to control inbound and outbound traffic. This allows us to gain insight into how network traffic flows between virtual machines and how security rules can be used to restrict or permit specific types of traffic.



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 
- Ubuntu Server 

<h2>Part1: Create Resources</h2>

<p> 1. Create a Resource Group in Azure.
  <p> https://portal.azure.com/
<p> 2. Create a Windows 10 Virtual Machine (VM).<p>During the VM setup, select the Resource Group created earlier.
  <p>Allow the creation of a new Virtual Network (Vnet) and Subnet for the VM.<img width="1382" height="811" alt="Screenshot 2026-02-05 at 4 28 55 PM" src="https://github.com/user-attachments/assets/22fa9dc6-c832-4761-a324-314267f92f5f" />

<p>3. Create a Linux (Ubuntu) Virtual Machine (VM). <p>Select the previously created Resource Group and Vnet for this VM.
<p>4. Observe the topology and details of your Virtual Network using Azure Network Watcher.
<img width="1433" height="809" alt="image" src="https://github.com/user-attachments/assets/b3be0e7e-36d1-4d09-8dcc-aa20c898252d" />



<p> 
</p><h2>Part2: Observe ICMP Traffic</h2>
<p> 1. Connect to the Windows 10 VM using Remote Desktop.<p><img width="1161" height="708" alt="Screenshot 2026-02-05 at 4 30 02 PM" src="https://github.com/user-attachments/assets/76e3a95b-7078-4c51-9d40-e13f0db0ae8b" />

<p> 2. Install Wireshark within the Windows 10 VM.<p><img width="698" height="457" alt="Screenshot 2026-02-05 at 5 45 05 PM" src="https://github.com/user-attachments/assets/0cca6272-f6de-402d-b9d0-130e0df7a92f" />


<p>3. Open Wireshark and apply a filter to capture only ICMP traffic (used for ping commands).
<p>4. Retrieve the private IP address of the Ubuntu VM and attempt to ping it from the Windows 10 VM.
Observe the ping requests and replies in Wireshark.<p><img width="1386" height="811" alt="Screenshot 2026-02-05 at 5 50 55 PM" src="https://github.com/user-attachments/assets/ef7b3b99-b15f-4516-8511-3f8790051eb0" />

<p>5. From the Windows 10 VM, open Command Prompt or PowerShell and ping a public website (such as www.google.com).
Observe the public ping traffic in Wireshark.<p><img width="1386" height="817" alt="Screenshot 2026-02-05 at 5 53 40 PM" src="https://github.com/user-attachments/assets/f8869616-56e4-4027-ba89-5aea873e15ea" />

<p>6. Initiate a continuous ping from the Windows 10 VM to the Ubuntu VM.<p><img width="1369" height="838" alt="Screenshot 2026-02-05 at 5 57 05 PM" src="https://github.com/user-attachments/assets/7168c632-9b1a-4dc5-8b95-560f30aa6d3a" />

<p>7. In the Azure portal, access the Network Security Group (NSG) assigned to the Ubuntu VM and disable inbound ICMP traffic.
Observe how the ICMP traffic stops in Wireshark and the command line due to the blocked traffic.
<p>8. Re-enable ICMP traffic in the NSG for the Ubuntu VM.
Observe the ICMP traffic start working again in Wireshark and on the command line.
<p>9. Stop the continuous ping activity from the Windows 10 VM.

  <p>
<img src="https://i.imgur.com/IIUShxp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We can inspect each individual packet and see the actual data that is being sent in each ping. the picture below demonstrates just that. 
</p>
<br />
<p>
<img src="https://i.imgur.com/GLxSIG3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In the next portion of the lab we will perpetually ping the Linux machine with the command ping -t. This will continually ping the machine until we decide to stop it, while the Windows machine is pinging the Linux machine we will go to the Linux machine and block inbound ICMP traffic on it's firewall. Once we do that we will stop recieving echo replys from the Linux machine. We will block ICMP by creating a new Network Security Group on the Linux machine that will be set to block ICMP. We can allow the traffic by allowing ICMP on the Linux Network Security Groups page on Azure. 
</p>
<br />
<img src="https://i.imgur.com/5vXO75R.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/Asl80tN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
Next we will use our Windows machine to SSH to the Linux machine. SSH has no GUI it just gives the user access to the machines CLI. We will set the wireshark filter to capture SSH packets only. When we ssh into the Linux machine with the command prompt "ssh labuser@10.0.0.5" we can see that wireshark starts to immediately capture SSH packets.
</p>
<br />
<img src="https://i.imgur.com/zteR41r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we will use wireshark to filter for DHCP. DHCP is the Dynamic Host Configuration Protocol this works on ports 67/68. It is used to assign IP addresses to machines. We will request a new ip address with the command "ipconfig /renew". Once we enter the command wireshark will capture DHCP traffic.
</p>
<br />
<img src="https://i.imgur.com/vU8fpQf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Time to filter DNS traffic. We will set wireshark to filter DNS traffic. We will initiate DNS traffic by typing in the command "nslookup www.google.com" this command essentially asks our DNS server what is google's IP address.
</p>
<br />
<img src="https://i.imgur.com/VMcwmsO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly we will filter for RDP traffic. When we enter tcp.port==3389 traffic is spammed non stop because we are using Remote Desktop Protocol to connect to our Virtual Machine. 
</p>
<br />
<img src="https://i.imgur.com/VxXGv6X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<br />
