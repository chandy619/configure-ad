<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This lab outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Command Prompt
- PowerShell ISE

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Steps</h2>

- Step 1: Create your Resources via Azure (Domain Controller and Client VMs)
- Step 2: Install and Deploy Active Directory
- Step 3: Join Client-1 VM to DC-1 VM
- Step 4: Create Domain Users

<h2>Deployment and Configuration Steps</h2>

<p>
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/f16b5755-735c-4253-a507-746633587493">
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/e926b571-b92c-491e-9892-e20704ec99af">
</p>
<p>
1. Create Your Resources in Azure: 1 Resource Group > 2 VMs > 1 vnet. The first VM will be a Windows Server 2022 as your Domain Controller (named DC-1) and the second VM will be a Windows 10 PC Client (named Client-1). Be sure to set the DC-1's NIC Private IP address to static by following this string of actions: DC-1 > Settings > Networking > Network Interface hyperlink > IP configurations > Click on result > Switch Assignment from 'Dynamic' to 'Static'> 'Save' changes.
</p>
<br />

<p>
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/960f38b6-6ec4-48e4-a7d9-e89235aa183f">
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/67313a5c-ee88-4bc1-b259-5869ca6efab9">
</p>
<p>
2. Ensure Connectivity Between Client-1 and DC-1: Using Remote Desktop, login to Client-1 and send a perpetual ping to DC-1's private IP address using 'ping -t (IP address)' via Command. You will see the connection being timed out. Next, login to DC-1 using Remote Desktop and enable ICMPv4 on the local Windows Firewall by following this string of actions: Start menu > search 'Firewall' > open 'Windows Defender Firewall with Advance Security' > click on 'Inbound Rules' > filter by Protocol > enable both 'Core Netowrking Diagnostics - ICMP Echo Request (ICMPv4-In)'. Lastly, check back on Client-1's VM to see if the ping succeeds. 
</p>
<br />

<p>
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/5ed02098-46f7-4790-81b9-287cd2be6565">
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/7ad702d1-5c78-401d-94f7-0c2a45343bda">
</p>
<p>
3. Install Active Directory (AD): Return to DC-1's VM to begin installing AD. Server Manager should be open; follow this string of actions to begin installing: Click on 'Add Roles and Features' > 'Next' through the first 3 sections > under 'Server Roles' check box for 'Active Directory Domain Services'> select 'Add Features' > 'Next' through the remaining sections > click 'Install'. Once AD installs, go to the top right corner of your Server Manager window to click on the Flag/Hazard icon. To finish installing AD, follow this string of actions: click 'Promote this server to a domain controller' > select 'Add a new forest' > type in 'mydomain.com' in root domain name > 'Next' > create a password and type it in (you won't need to remember it for this lab) > 'Next' through each section until you get the option to click 'Install'. Once it's complete, your computer will automatically restart.
</p>
<br />

<p>
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/aaf5c058-b247-4de0-9523-8c9068ea8ba1">
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/83dee9e8-b3c1-4759-a6a5-ca1f39b07a62">

</p>
<p>
4. Create Organizational Units (OUs) and an Admin User within AD: Since your DC-1 VM restarted, you'll need to log back in using the FQDN (Fully Qualified Domain Name) as the username, i.e., 'mydomain.com\labuser'. Once you're logged in, click on the Start menu and search for 'Active Directory Users and Computers'. First you'll need to create 2 new OUs. Follow this string of actions to do so: Click on 'mydomain.com to expand its folder > right-click 'mydomain.com' > click 'New' > select 'Organizational Unit' > type in '_EMPLOYEES' > OK. Repeat the steps to create another OU named '_ADMINS'.
</p>
<p>
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/9c404a0b-2ee5-4f08-bd09-e1adcf2a71ce">
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/56608981-ba37-428e-b35d-00dd7997e822">
</p>
<p>
Now that you've created your OUs, you'll need to create an Adminstrative User. Follow this string of actions to create an admin named Jane Doe: Right-click '_ADMINS' folder > 'New' > 'User' > Fill in required fields > 'Next' > create a password > 'Next' > 'Finish'. Now, you'll need to add administrative permissions to Jane Doe's account. Follow these steps: Double-click the '_ADMINS' folder > double-click on Jane Doe's name > click on the 'Member Of' tab > 'Add' > type in 'Domain Admins' > click 'Check Names' > 'Apply' > 'OK'. When you're finished, log out of DC-1's VM and log back in using Jane Doe's credentials, i.e., 'mydomain.com\jane_admin' and password.
</p>
<br />

<p>
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/853e0bec-54a7-4939-bb79-4e7ba62a35cf">
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/3598bf78-bfdc-4ddd-bcfa-b6125172f72f">
</p>
<p>
5. Join Client-1 to Your Domain (mydomain.com): In Azure, the first thing you'll want to do is copy DC-1's private IP address. Go back to Virtual Machines and select 'Client-1'. To join Client-1 to 'mydomain.com', follow this string of actions: click 'Networking' on the left-hand side > click on 'Network Interface' hyperlink > click on 'DNS Servers' on the left-hand side > select 'Custom' versus 'Inherit from virtual network' > paste DC-1's private IP address into the field box > hit 'Save' > return to Client-1's VM home page and click the 'Restart' icon at the top of your window.
</p>
<br />

<p>
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/6c469433-7f49-47cf-90eb-2e33ca294bed">
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/33929693-ac82-47d2-ac5d-5d84813644af">
</p>
<p>
Using Remote Desktop, log back into Client-1. Once you're logged in, open Command from the Start menu and enter 'ipconfig /all' to check if the DNS Servers reflect's the DC-1's private IP address. Now, right-click the Start menu and click on 'System' for settings. Click on 'Rename this PC (advanced)' located on the right-hand side > click the 'Change' button > click on 'Domain' and type 'mydomain.com' > 'OK' > enter Jane's FQDN credentials, i.e., 'mydomain.com\jane_admin' and password. > 'OK'. Your computer will need to restart.
</p>
<br />

<p>
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/d5dc49a3-6daa-4817-b4e0-596063036dcd">
</p>
<p>
6. Set-up Remote Desktop for Non-Admin Users on Client-1: Log back into Client-1's VM using Jane Doe's FQDN credentials. Open system properties again using the Start menu. Select 'Remote desktop' on the right-hand side. Click on 'Select users that can remotely access this PC' at the bottom. Click 'Add' and type in 'Domain Users'. Click 'Check Names' and once it populates, click 'OK'. Now all domain users will be able to Remote Desktop login to Client-1's computer.
</p>
<br />

<p>
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/4f744b09-0fdc-45b3-8aed-45183448ad0c">
</p>
<p>
7. Create Additional Users and Attempt to Login to Client-1: Go back to DC-1's VM where you should still be logged in as Jane Doe. Follow this string of actions to create hundreds or thousands of random domain users: Start menu > search 'PowerShell ISE' > right-click to 'Run as an administrator' > click 'Yes' > click on the paper icon to create a 'New Script' > paste the contents of this <a href="https://github.com/AsiaPonder001/BunchofUsers/blob/main/README.md?plain=1)"> script </a> into it > 'Run' script by selecting the green play icon > Observe the accounts being created.
</p>
<br />

<p>
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/c395eb35-7fe7-4226-acda-ec6432c599cf">
<img width="960" alt="image" src="https://github.com/chandy619/configure-ad/assets/144288806/72fe0c00-bbf4-4da0-a331-81b0d948ce34">
</p>
<p>
After creating the domain users, return to 'Active Directory Users and Computers' and double-click the '_EMPLOYEES' folder to view the list of random domain usernames you've just generated. Choose one to Remote Desktop login using Client-1's VM. Don't forget to use the FQDN username and password (Password1) to suggessfully login to Client-1's VM. In the next couple of labs, we'll get more practice with Domain Name System and Newtork File Shares and Permissions.
</p>
<br />
