<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- cmd (Command)

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Create your Resources via Azure (Domain Controller and Client VMs)
- Step 2: Install and Deploy Active Directory
- Step 3: Join Client VM to Domain Controller VM
- Step 4: Create Users

<h2>Deployment and Configuration Steps</h2>

<p>
<img width="323" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/0b1aa6d6-658b-4f21-b88d-dfbd236c9b69">
</p>
<p>
1. Create your resources in Azure: 1 Resource Group > 2 VMs > 1 vnet. The first VM will be a Windows Server 2022 named Domain Controller (DC-1) and the second VM will be a Windows 10 named Client (Client-1). Be sure to set the DC-1's NIC Private IP address to static by following this string of actions: DC-1 > Settings > Networking > Network Interface > IP configurations > Click on result >Switch Assignment to 'Static' from 'Dynamic' > save changes.
</p>
<br />

<p>
<img width="892" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/3a311c6a-c630-4606-a14c-7eb97e6f9f0c">
<img width="431" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/12bb24b6-e0eb-463e-b7df-996551e74a01">
</p>
<p>
2. Ensure Connectivity between Client-1 and DC-1: Using Remote Desktop, login to Client-1 and send a perpetual ping to DC-1's private IP address using 'ping -t (IP address)' via cmd. You will see the connection being timed out. Next, login to DC-1 using RDP and enable ICMPv4 on the local Windows Firewall by following this string of actions: Start menu > search 'Firewall' > open 'Windows Defender Firewall with Advance Security' > click on 'Inbound Rules' > filter by 'Protocol' > enable both 'Core Netowrking Diagnostics - ICMP Echo Request (ICMPv4-In)'. Lastly, check back on Client-1's VM to see if the ping succeeds. 
</p>
<br />

<p>
<img width="365" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/344606fa-c9a4-40f8-b793-bf6e6ea67657">
<img width="782" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/410e31fe-4ca5-4b04-b26c-714201815450">
</p>
<p>
3. Install Active Directory (AD): Return to DC-1's VM to begin installing AD. Server Manager should be open; follow this string of actions to begin installing: Click on 'Add Roles and Features' > 'Next' through the first 3 sections > under 'Server Roles' check box for 'Active Directory Domain Services'> select 'Add Features' > Next through the remaining sections > click "Install'. Once AD installs, go to the top right corner of your Server Manager window to click on the Flag and Hazard icon. To finish installing AD, follow this string of actions: click 'Promote this server to a domain controller' > select 'Add a new forest' > type in 'mydomain.com' in root domain name > 'Next' > Create a password and type it in > 'Next' through each section until you get to the option to click 'Install'. Once it's complete, your computer will automatically restart.
</p>
<br />

<p>
<img width="309" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/ec5b7748-e53b-4d89-abbc-74036838e3c2">
<img width="395" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/ea682246-c823-4ce5-a213-50ed9468bbef">
</p>
<p>
4. Create Organizational Units and Users within Active Directory (AD): Since your DC-1 VM restarted, you'll need to log back in using the FQDN (Fully Qualified Domain Name) as the username, i.e., mydomain.com\labuser. Once you're logged in, click on the 'Start' menu and search for 'Active Directory Users and Computers'. First you'll need to create 2 new Organizational Units (OUs). Follow this string of actions to do so: Right-click 'mydomain.com' > 'New' > 'Organizational Unit' > type in '_EMPLOYEES' >'OK'. Repeat the steps to create another named '_ADMINS'.
</p>
<p>
<img width="487" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/e73e5a96-f3fb-47fc-b9ed-62fec578c450">
<img width="252" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/3c83dafa-a7e8-4005-aa22-72200f0c1f4d">
</p>
<p>
Now that you've created your OUs, you'll create another Adminstrative User. Follow this string of actions to create an Admin User named Jane Doe: Right-click '_ADMINS' folder > 'New' > 'User' > Fill in required fields > 'Next" > Create a password > 'Next' > 'Finish'. Now, you'll need to add administrative permissions to Jane Doe's account. Follow these steps: Right-click Jane Doe's name > select 'Properties' > click on 'Member Of' tab > 'Add' > type in 'Domain Admins' > click 'Check Names' > 'Apply' > 'OK'. When you're finished, log out of DC-1's VM and log back in using Jane Doe's profile with 'mydomain.com\jane_admin'.
</p>
<br />

<p>
<img width="388" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/a40eb540-d84f-45e0-9c83-d4226c34d894">
</p>
<p>
5. Join Client-1 to Your Domain (mydomain.com): In Azure, the first thing you'll want to do is copy DC-1's Private IP Address. Go back to Virtual Machines and select Client-1. To join Client-1 to 'mydomain.com', follow this string of actions: click 'Networking' on left-hand side > click on 'Network Interface' hyperlink > click on 'DNS Servers' on the left-hand sided > select 'Custom' versus 'Inherit from virtual network' > paste DC-1's Private IP address into the field box > hit 'Save' > return to Clien'1's VM home page and click 'Restart' icon at the top of your window. Using Remote Desktop, log back into Client-1. Once you're logged in, open 'cmd' from the 'Start' menu and enter 'ipconfig /all' to check if the DNS Servers reflect's the DC-1's private IP address.
</p>
<br />

<p>
<img width="556" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/a9d495c6-996d-4193-ab92-30ee596ff88d">
</p>
<p>
Moving along, go to the 'Start' menu and click on 'System' for settings. Click on 'Rename this PC (advanced)' located on the right-hand side > click'Change' > click on 'Domain' and enter 'mydomain.com' > OK > enter Jane Doe's credentials > 'OK'. Your computer will restart.
</p>
<br />

<p>
<img width="471" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/f946bebc-ae80-4302-9aa5-9983f0093667">
</p>
<p>
6. Set-up Remote Desktop for Non-Admin Users on Client-1: Log back into Client-1's VM using Jane Doe's credentials, i.e., 'mydomain.com\jane_admin' and password. Open system properties again using the 'Start' menu. Select 'Remote desktop' on the right-hand side. Click on 'Select users that can remotely access this PC' at the bottom. Click 'Add' and type in 'Domain Users'. Click 'Check Names' and once it populates, click 'OK'. Now all domain users will be able to Remote Desktop or login to Client-1's computer.
</p>
<br />

<p>
<img width="389" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/b22d3097-6858-4d20-999b-cc3445011300">
<img width="656" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/486d7e93-ed3d-473e-9f7e-a2912bb1e8f4">
</p>
<p>
7. Create Additional Users and Attempt to Login to Client-1: Go back to DC-1's VM where you should still be logged in as Jane Doe. Follow this string of actions to create thousands of random domain users: 'Start' menu > search 'PowerShell ISE' > right-click to 'Run as an administrator' > click 'Yes' > click on the paper icon to create a New Script > paste the contents of this <a href="https://github.com/AsiaPonder001/BunchofUsers/blob/main/README.md?plain=1)"> script </a> into it > 'Run' script by selecting the green play icon > Observe the accounts being created.
</p>
<br />

<p>
<img>
</p>
<p>
Text
</p>
<br />

<p>
<img>
</p>
<p>
Text
</p>
<br />
