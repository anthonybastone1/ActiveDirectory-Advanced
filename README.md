<h1>Active Directory Home Lab - Brute Force Attack Simulation</h1>

<h2>Description</h2>
In this project, I setup an Active Directory home lab that includes Splunk, Kali Linux, and Atomic Red Team. In doing so, I learned how a domain environment works, increased my knowledge of how to ingest events to a SIEM, and generated telemetry related to common attacks. This project has allowed me to gain hands-on experience and enable myself to better detect threats in a live environment.

<h2>Key Objectives</h2>

-
-
- Install and configure Active Directory onto Windows Server, promote it to DC (domain controller), configure the target machine to join the domain.
- Kali Linux, brute force attack, view telemetry via splunk, setup and install Atomic Red Team to run tests.

<h2>Languages and Utilities Used</h2>

- <b>Linux</b> 
- <b>Sysmon</b>
- <b>Splunk Enterprise</b>
- <b>Splunk Forwarder</b>
- <b>Atomic Red Team</b>

<h2>Environments Used </h2>

- <b>Oracle VirtualBox</b>
- <b>Windows 10 VM (Target Machine)</b>
- <b>Kali Linux VM (Attacker Machine)</b>
- <b>Windows Server 2019 (Active Directory)</b>
- <b>Ubuntu Server (Splunk)</b>

<h2>Project Diagram:</h2>
<br />
<img src="https://imgur.com/3AKf5JX.png"height="80%" width="80%"/>
<br />
<h2>Splunk Server Configuration:</h2>

<p align="center">
<br />
First course of action was to create a NAT Network for the Active Directory project and assign all 4 machines to it so they could be on the same network and still have internet access. During the configuration, I made sure to assign the network IP address that I had designated for it in the diagram.
<br />
<br />
<img src="https://imgur.com/hNhyXMv.png" height="80%" width="80%"/>
<br />
<br />
<img src="https://imgur.com/0xjS3zZ.png"height="80%" width="80%"/>
<br />
<br />
<br /> 
<br />
<br />
Next, I navigated to the Splunk machine and typed in, ip a, to check the assigned IP address. It was 192.168.10.5/24, which if you go back to the diagram, needs to be assigned a static IP address of 192.168.10.10/24.
<br/>
<br/>
<img src="https://imgur.com/wce5nck.png" height="80%" width="80%"/>
<br />
<br />
In order to assign a static IP address to my Splunk server, I ran the following line of code: sudo nano /etc/netplan/00-installer-config.yaml
<br />
<br />
Then made the following changes and applied them in the screen captures below: 
<br />
<br />
<img src="https://imgur.com/OZcmFrj.png"height="80%" width="80%"/>
<br />
<br />
<img src="https://imgur.com/UdPp3IC.png"height="80%" width="80%"/>
<br />
<br />
<br /> 
<br />
<br /> 
I ran ip a again to verify the changes were successfully applied, then pinged Google to check for connectivity.  
<br/>
<br/>
<img src="https://imgur.com/pE1BDkr.png" height="80%" width="80%"/>
<br />
<br />
<br /> 
<br />
<br />  
After using the sudo command to install the virtualbox-guest-additions-iso and virtualbox-guest-utils followed by rebooting the server. I also created a shared folder using the folder that the splunk installer was in.
<br/>
<br/>
<img src="https://imgur.com/8ELDd13.png" height="80%" width="80%"/>
<br/>
<br/>
<img src="https://imgur.com/YkFNLH9.png" height="80%" width="80%"/>
<br/>
<br/>
<br/>
<br/>
<br/>
Here I added a user to the group vboxsf using the command: sudo adduser ab vboxsf
<br/>
<br/>
I then created a new directory named, share, by using the command: mkdir share
<br/>
<br/>
<img src="https://imgur.com/FSFtVtq.png" height="80%" width="80%"/>
<br/>
<br/>
<br/>
<br/>
<br/>
Next, I ran the following command to mount the new shared folder to the share directory that was just created and changed directories into the share directory:
<br/>
<br/>
sudo mount -t vboxsf -o uid=1000,gid=1000 Active_Directory_Project share/
<br/>
<br/>  
<img src="https://imgur.com/MrTSYwh.png" height="80%" width="80%"/>
<br/>
<br/>
<br/>
<br/>
<br/>
After changing into the share directory, I listed the contents of the directory to find the splunk installer and then ran the following code to install it:
<br/>
<br/>
<img src="https://imgur.com/LjWUNmU.png" height="80%" width="80%"/>
<br/>
<br/>
<br/>
<br/>
<br/> 
Changed into the user, splunk, then the directory, bin, to run the installer.
<br/>
<br/> 
<img src="https://imgur.com/ibuLyU4.png" height="80%" width="80%"/> 
<br/>
<br/>
<br/> 
<br/>
<br/>  
Ran one more command to make sure splunk runs with the user, splunk, every time the virtual machine reboots.
<br/>
<br/>
<img src="https://imgur.com/SiK8zLY.png" height="80%" width="80%"/>
<br/>
<br/>
<br/>
<br/> 
<br/>

<h2>Windows 10 (Target Machine):</h2>

<p align="center">
  
<br />
Moving onto the target machine, the first thing I did was change the device name to target-PC.
<br />
<br />
<img src="https://imgur.com/2y2V2Ib.png" height="80%" width="80%"/>
<br />
<br />
<br /> 
<br />
<br />
Next, I used the command ipconfig to take a look at the IP address of the machine. It's set to the address that needs to be used for my Windows Server, so I set a static IP address of 192.168.10.100
<br />
<br />
<img src="https://imgur.com/l9pwBtq.png" height="80%" width="80%"/>
<br />
<br />
<img src="https://imgur.com/dCjwQEj.png" height="80%" width="80%"/>
<br />
<br />
<br /> 
<br />
<br />
After setting the designated IP address, I began installing splunk forwarder. There is no deployment server here, so I only needed to input my splunk server address for the receiving indexer.
<br />
<br />
<img src="https://imgur.com/bYpnVJ9.png" height="80%" width="80%"/>
<br />
<br />
<br />
<br />
<br />
Following the splunk forwarder installation, I downloaded sysmon and used the sysmon configuration by Olaf.
<br />
<br />
<img src="https://imgur.com/XdzyuHw.png" height="80%" width="80%"/>
<br />
<br />
<br />
<br />
<br />
Successfully installed sysmon by running the following command in PowerShell:
<br />
<br />
<img src="https://imgur.com/z5pgWvp.png" height="80%" width="80%"/>
<br />
<br />
<br />
<br />
<br />
Here I created a new inputs.conf file under the local directory and not the default directory. I ran the notepad as an administrator with the following lines of code that instruct the splunk forwarder to push events related to application, security, system, and sysmon over to the splunk server. I have the index pointing to an index named endpoint, so any events that fall under the aforementioned categories will be send to splunk and placed under that specific index.
<br />
<br />
<img src="https://imgur.com/6hgRg2L.png" height="80%" width="80%"/>
<br />
<br />
<img src="https://imgur.com/TPYlOjJ.png" height="80%" width="80%"/>
<br />
<br />
<br />
<br />
<br />
Before restarting splunk forwarder for the changes to take effect, I needed to change the logon setting to logon as a local system account instead of the NT SERVICE account. I did this because the splunk forwarder would not be able to collect logs due to some of the permissions.
<br />
<br />
<img src="https://imgur.com/njRedhb.png" height="80%" width="80%"/>
<br />
<br />
<br />
<br />
<br />
After getting splunk and sysmon successfully installed on the target-PC, I logged onto my splunk enterprise web portal to create my endpoint index.
<br />
<br />
<img src="https://imgur.com/7786WSE.png" height="80%" width="80%"/>
<br />
<br />
<img src="https://imgur.com/kaKeZ2I.png" height="80%" width="80%"/>
<br />
<br />
<br />
<br />
<br />
Once the endpoint index was created, I enabled the splunk server to receive the data.
<br />
<br />
<img src="https://imgur.com/4YjoK2Z.png" height="80%" width="80%"/>
<br />
<br />
<br />
<br />
<br />
Checking to see if the data is being received by the splunk server. Confirming the host machine that's been logged is correct and identifying the categories that I included in the new inputs.conf file in the local directory.
<br />
<br />
<img src="https://imgur.com/uSYZXyg.png" height="80%" width="80%"/>
<br />
<br />
<img src="https://imgur.com/KtaN8ec.png" height="80%" width="80%"/>
<br />
<br />
<img src="https://imgur.com/0PcuSZa.png" height="80%" width="80%"/>
<br />
<br />
<br />
<br />
<br />

<h2>Windows Server (ADDC):</h2>

<p align="center">
  
<br />
To install sysmon and splunk on my Active Directory server, I followed the same steps as the target-PC. The first thing I did was rename the device to ADDC01. In the end, I verified all of my work in the splunk enterprise web portal. Everything was successfully installed and configured, and now two hosts were logged instead of one. The two hosts being the target-PC and ADDC01.
<br />
<br />
<img src="https://imgur.com/8v0eeNs.png" height="80%" width="80%"/>
<br />
<br />
<br />
<br />
<br />
After installing sysmon and splunk, I assigned a static IP address of 192.168.10.7 with the same subnet mask of 255.255.255.0 since it is a /24. Still using Google's DNS of 8.8.8.8.
<br />
<br />
Then I opened the server manager and installed AD DS (Active Directory Domain Services), followed by promoting the server to a domain controller (DC)
<br />
<br />
<img src="https://imgur.com/maOd4eD.png" height="80%" width="80%"/>
<br />
<br />
<img src="https://imgur.com/YMw1hJd.png" height="80%" width="80%"/>
<br />
<br />
<br />
<br />
<br />
Next, I created two organizational units and two users.
<br />
<br />
<img src="https://imgur.com/CfZuJdY.png" height="80%" width="80%"/>
<br />
<br />
<img src="https://imgur.com/k3z188z.png" height="80%" width="80%"/>
<br />
<br />
<br />
<br />
<br />
Joinning the target-PC to the new domain, ab.local, and authenticating using James Brown's account. But first, the target-PC could not resolve ab.local due to the DNS server pointing to Google's DNS. So I changed it to point to the DC at 192.168.10.7 and confirmed with ipconfig /all in the command prompt that the new DNS server is active.
<br />
<br />
<img src="https://imgur.com/CEk89PK.png" height="80%" width="80%"/>
<br />
<br />
<img src="https://imgur.com/wXWtg9E.png" height="80%" width="80%"/>
<br />
<br />
<img src="https://imgur.com/RVnAVty.png" height="80%" width="80%"/>
<br />
<br />
<img src="https://imgur.com/3EBq6D3.png" height="80%" width="80%"/>
<br />
<br />
<br />
<br />
<br />
<h2>Kali Linux VM (Attacker Machine):</h2>

<p align="center">
  
<br />
Assigning a static IP address of 192.168.10.250/24. Verified the change took place by running the command ip a and pinging google.com and the splunk server at 192.168.10.10.
<br />
<br />
<img src="https://imgur.com/Xf6UHpf.png" height="80%" width="80%"/>
<br />
<br />
<img src="https://imgur.com/hKU8CJd.png" height="80%" width="80%"/>
<br />
<br />
<img src="https://imgur.com/ffvCanW.png" height="80%" width="80%"/>
<br />
<br />
<br />
<br />
<br />
Installed updates and upgrades then created a new directory named ad-project. Installed the tool crowbar to use for the brute force attack. 
<br />
<br />
<img src="https://imgur.com/GO9FcOj.png" height="80%" width="80%"/>








</p>
<br />
<br />
<h2>Conclusion</h2>
In this project, I set up a home lab using Elastic SIEM and a Kali VM. I configured the system to forward data from the Kali VM to the SIEM using the Elastic Beats agent, generated security events on the Kali VM using Nmap, and queried and analyzed the logs in the SIEM using the Elastic web interface. 
<br />
<br />
I also created a dashboard to visualize security events and then created an alert to detect security events. This home lab provided a valuable environment for learning and practicing the skills necessary for effective security monitoring and incident response using Elastic SIEM.

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
