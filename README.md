# AzureVM
<h2 dir="auto" tabindex="-1">Environments and Technologies Used</h2>
<ul dir="auto">
 	<li>Microsoft Azure (Virtual Machines/Compute)</li>
 	<li>Remote Desktop</li>
 	<li>Active Directory Domain Services</li>
 	<li>PowerShell</li>
</ul>
<h2 dir="auto" tabindex="-1">Operating Systems Used</h2>
<ul dir="auto">
 	<li>Windows Server 2022</li>
 	<li>Windows 10 (21H2)</li>
</ul>
<h2 dir="auto" tabindex="-1">Deployment and Configuration Steps</h2>
<ul dir="auto">
 	<li>Create Resources</li>
 	<li>Ensure Connectivity between the client and Domain Controller</li>
 	<li>Install Active Directory</li>
 	<li>Create an Admin and Normal User Account in AD</li>
 	<li>Join Client-1 to your domain (mydomain.com)</li>
 	<li>Setup Remote Desktop for non-administrative users on Client-1</li>
 	<li>Create a menu additional users and attempt to log into client-1 with one of the users</li>
</ul>
&nbsp;
<h3 dir="auto" tabindex="-1" align="center">Setup Resources in Azure</h3>
<img class="alignnone wp-image-151 size-full" src="https://vikaspatel.tech/wp-content/uploads/2023/04/Image-1-1-e1680401746466.jpg" alt="" style="width: 100%; height: auto;" />
<ul>
 	<li style="text-align: left;">We will establish two Virtual Machines (VMs) within a single Virtual Network (VNET). One of these machines will function as a Domain Controller, while the other will operate as a Client. We will modify the Domain Controller's IP address to be static because it will provide Active Directory services to the Client. The Client will then be joined to the domain, and we will manage the DNS configurations on this machine by setting the Domain Controller as the DNS server.</li>
 	<li>Create a resource group</li>
</ul>
<img class="alignnone wp-image-151 size-full" src="https://vikaspatel.tech/wp-content/uploads/2023/04/Image-1-1-e1680401746466.jpg" alt="" style="width: 100%; height: auto;" />
<ul>
 	<li>After that create a windows server and virtual machine within that resource group</li>
</ul>
<a href="https://vikaspatel.tech/wp-content/uploads/2023/04/2.png"><img class="alignnone size-full wp-image-150" src="https://vikaspatel.tech/wp-content/uploads/2023/04/2.png" alt="" width="2612" height="1630" /></a>
<ul>
 	<li>To prevent the IP address of DC-1 from changing, we need to access its network settings. We can do this by selecting the 'Networking' option and clicking on the hyperlink next to the 'Network Interface' heading. From here, we should navigate to the 'IP Configurations' menu and locate 'ipconfig1'. By changing the assignment from dynamic to static, we can ensure that DC-1 retains its assigned IP address. Finally, we should verify that both VMs are connected to the same 'Vnet' by checking the NIC settings. This will facilitate communication and connectivity between the two machines during the remainder of the lab.</li>
</ul>
<a href="https://vikaspatel.tech/wp-content/uploads/2023/04/3.png"><img class="alignnone size-full wp-image-152" src="https://vikaspatel.tech/wp-content/uploads/2023/04/3.png" alt="" width="2612" height="1630" /></a>
<ul>
 	<li>To establish a connection between DC-1 and Client-1, DC-1 must have a fixed private IP address. However, when we attempt to ping DC-1 from Client-1, the initial attempt fails. To rectify this, we need to activate ICMPv4 on the firewall located on DC-1. After enabling this feature, we can successfully ping DC-1 from Client-1.</li>
</ul>
<a href="https://vikaspatel.tech/wp-content/uploads/2023/04/4.png"><img class="alignnone size-full wp-image-153" src="https://vikaspatel.tech/wp-content/uploads/2023/04/4.png" alt="" width="1044" height="781" /></a>
<ul>
 	<li>Ensure the communication between both VMs via ping using CMD.</li>
 	<li>Now we will install Active directory domain services in DC1.</li>
</ul>
<a href="https://vikaspatel.tech/wp-content/uploads/2023/04/5.png"><img class="alignnone size-full wp-image-154" src="https://vikaspatel.tech/wp-content/uploads/2023/04/5.png" alt="" width="2876" height="1716" /></a>
<ul>
 	<li>Promote this server to a domain controller.</li>
</ul>
<a href="https://vikaspatel.tech/wp-content/uploads/2023/04/6.png"><img class="alignnone size-full wp-image-155" src="https://vikaspatel.tech/wp-content/uploads/2023/04/6.png" alt="" width="2872" height="1722" /></a>
<ul>
 	<li>to proceed, we need to log back into DC-1 and install AD Users &amp; Computers. We should then promote the VM to DC and establish a new forest using the name 'mydomain.com'. Once the setup is complete, we should restart the machine and log back in using the user name 'mydomain.com\labuser'. If all of the preceding steps have been executed correctly, we should be able to run AD Users &amp; Computers, as demonstrated in the example below.</li>
</ul>
<a href="https://vikaspatel.tech/wp-content/uploads/2023/04/7.png"><img class="alignnone size-full wp-image-156" src="https://vikaspatel.tech/wp-content/uploads/2023/04/7.png" alt="" width="2612" height="1630" /></a>
<ul>
 	<li>We can now proceed to create Organizational Units (OUs) within our system. The first step is to create an OU called '_EMPLOYEES' and another called '_ADMINS'. To do this, we need to right-click on the domain area, select 'New', and then choose 'Organizational Unit'. Once we have entered the necessary information, we can click inside the OU and create a new user by selecting 'New', then 'User', and entering the relevant data for the user. For this exercise, we will create a new user named Vikas Patel and assign him the username 'vikas_admin'. Finally, we should add Vikas to the domain admins security group.</li>
</ul>
<a href="https://vikaspatel.tech/wp-content/uploads/2023/04/1-1.png"><img class="alignnone size-full wp-image-159" src="https://vikaspatel.tech/wp-content/uploads/2023/04/1-1.png" alt="" width="2872" height="1630" /></a>
<ul>
 	<li>Going forward, we will use the vikas_admin account as the administrator account. Next, we need to connect Client-1 to the domain (mydomain.com) by modifying its DNS settings in the Azure portal to match the private IP address of DC-1. After making these changes, we should restart Client-1 from within the portal. Once this is done, we can verify that Client-1 is connected to the DC-1 DNS, as depicted in the picture below.</li>
</ul>
<a href="https://vikaspatel.tech/wp-content/uploads/2023/04/9.png"><img class="alignnone size-full wp-image-158" src="https://vikaspatel.tech/wp-content/uploads/2023/04/9.png" alt="" width="2612" height="1630" /></a>
<ul>
 	<li>In order to connect Client-1 to the domain, we need to navigate to the system settings and select 'About'. Then, we should select 'Rename this PC (Advanced)' and opt to change the domain. We should enter 'mydomain.com' and provide the necessary credentials for mydomain.com\labuser. Once this is complete, the computer will restart and Client-1 will become part of the mydomain.com domain.</li>
</ul>
<a href="https://vikaspatel.tech/wp-content/uploads/2023/04/5-1.png"><img class="alignnone size-full wp-image-160" src="https://vikaspatel.tech/wp-content/uploads/2023/04/5-1.png" alt="" width="2612" height="1630" /></a>
<ul>
 	<li>Great! With Client-1 now a part of the domain, we can proceed to set up remote desktop access for non-administrative users. To do this, we will need to log into Client-1 as an administrator and open the system properties. Then, we should click on 'Remote Desktop' and grant access to 'domain users'. Once these steps are complete, normal users should be able to log into Client-1 using remote desktop access.</li>
</ul>
<a href="https://vikaspatel.tech/wp-content/uploads/2023/04/8-1.png"><img class="alignnone size-full wp-image-161" src="https://vikaspatel.tech/wp-content/uploads/2023/04/8-1.png" alt="" width="2612" height="1630" /></a>
<ul>
 	<li dir="auto">Lastly to verify that normal users can RDP into Client-1 we will use a script to generate thousands of users into the domain. We will input the script in powershell, after the users are created we will select one and RDP into Client-1.</li>
</ul>
<a href="https://vikaspatel.tech/wp-content/uploads/2023/04/9-1.png"><img class="alignnone size-full wp-image-162" src="https://vikaspatel.tech/wp-content/uploads/2023/04/9-1.png" alt="" width="2612" height="1630" /></a>
<ul>
 	<li>Now we can login with any user</li>
</ul>
<a href="https://vikaspatel.tech/wp-content/uploads/2023/04/11.png"><img class="alignnone size-full wp-image-163" src="https://vikaspatel.tech/wp-content/uploads/2023/04/11.png" alt="" width="2612" height="1630" /></a>
<ul>
 	<li> The login attempt with the user's name &amp; generic password should be successful. That is the conclusion of this lab.</li>
</ul>
