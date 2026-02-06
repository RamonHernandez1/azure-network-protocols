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
Observe how the ICMP traffic stops in Wireshark and the command line due to the blocked traffic.<p><img width="1340" height="805" alt="Screenshot 2026-02-05 at 6 05 59 PM" src="https://github.com/user-attachments/assets/a63df506-de89-434b-9772-9033a3835295" /><p><img width="1378" height="811" alt="Screenshot 2026-02-05 at 6 07 10 PM" src="https://github.com/user-attachments/assets/4af1c7c4-6a3f-41fe-9810-2f13d646fdb0" />


<p>8. Re-enable ICMP traffic in the NSG for the Ubuntu VM.
Observe the ICMP traffic start working again in Wireshark and on the command line.<p><img width="1341" height="749" alt="Screenshot 2026-02-05 at 6 12 41 PM" src="https://github.com/user-attachments/assets/1db95a73-6af0-4c5c-9a4e-ab08bb2a681a" /><p><img width="1364" height="841" alt="Screenshot 2026-02-05 at 6 27 25 PM" src="https://github.com/user-attachments/assets/14919a00-0ce4-4c4d-9372-68e12ab46c58" />


<p>9. Stop the continuous ping activity from the Windows 10 VM.

  <p><h2>Part3: Observe SSH Traffic</h2><p>1. In Wireshark, apply a filter to capture SSH traffic.
<p>2. From the Windows 10 VM, establish an SSH connection to the Ubuntu VM using its private IP address.
<p>Type commands (e.g., username, password) into the SSH session and observe the SSH traffic in Wireshark.<p><img width="1411" height="820" alt="Screenshot 2026-02-05 at 7 10 21 PM" src="https://github.com/user-attachments/assets/abac163b-5b4b-4e8f-8382-ebe3a1188f5f" />

<p>Exit the SSH session by typing exit and pressing Enter.<img width="1407" height="840" alt="Screenshot 2026-02-05 at 7 31 17 PM" src="https://github.com/user-attachments/assets/5cd03fc9-3e2b-4f79-ae38-6352f9f849e7" />

</p><h2>Part4: Observe DHCP Traffic</h2><p>1. In Wireshark, apply a filter to capture DHCP traffic.
<p>2. From the Windows 10 VM, attempt to request a new IP address using the command ipconfig /renew.
Observe the DHCP traffic in Wireshark as the VM communicates with the DHCP server to renew its IP address.<p><img width="1413" height="838" alt="Screenshot 2026-02-05 at 7 40 13 PM" src="https://github.com/user-attachments/assets/76a2dd95-94a6-43c6-a972-30f654af24ef" /> <p></p><h2>Part5: Observe DNS Traffic</h2><p><p>1. In Wireshark, apply a filter to capture DNS traffic.
<p>2. From the Windows 10 VM, use the nslookup command to resolve the IP addresses of websites like google.com and disney.com.
<p>Observe the DNS query and response traffic in Wireshark.<p><img width="1394" height="776" alt="Screenshot 2026-02-05 at 7 57 06 PM" src="https://github.com/user-attachments/assets/ef5df1aa-52c7-4659-8076-324ed1b37722" /><p></p><h2>Part6: Observe RDP Traffic</h2> <p><p>1. In Wireshark, apply a filter to capture RDP traffic (using the filter tcp.port == 3389).
<p>2.Observe the continuous stream of RDP traffic between your local machine and the Windows 10 VM.
<p>-This constant stream is because RDP continuously transmits data to keep the live remote session active.
<p>-The protocol constantly sends data, even when there is no specific user activity, to maintain the connection and update the screen in real time.<p><img width="1392" height="775" alt="Screenshot 2026-02-05 at 8 09 30 PM" src="https://github.com/user-attachments/assets/e682809d-2f5b-46a8-b8ae-381af8ab23a1" />





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
