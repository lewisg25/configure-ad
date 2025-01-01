# configure-ad
<p align="center">
    <img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
    </p>
    <hr style="border: 1px solid #ccc; width: 80%; margin: 20px auto;">
  <h1 style="text-align: center; font-weight: bold; font-size: 16pt;">On-premises Active Directory Deployed in the Cloud (Azure)</h1> </p> 

<p>
This tutorial provides a comprehensive guide to deploying an on-premises Active Directory environment within Azure Virtual Machines. It covers the strategic steps required for integration and configuration, ensuring optimal functionality within a virtualized infrastructure. By following this tutorial, IT professionals will gain a clear understanding of how to extend on-premises capabilities securely in the cloud.
</p>
<p><b>Technologies and Platforms used in this Demonstration:</b> <br>

<ul> 
  <li>Microsoft Azure </li>
  <li> Remote Desktop Protocol</li>
  <li> Active Directory Domain Services</li>
  <li>PowerShell </li>
</ul>

</p>

<p><b>Operating Systems:</b> <br>

<ul> 
  <li>Windows Server 2022 </li>
  <li> Windows 10</li>
  <li> Active Directory Domain Services</li>
  <li>PowerShell </li>
</ul>

  </p>  
    
  <hr style="border: 1px solid #ccc; width: 80%; margin: 20px auto;">

  <h1 style="text-align: center; font-weight: bold; font-size: 16pt;">Deployment and Configuration Steps</h1> </p> 
  <p>
    <b>Step 1 - Setting up Virtual Machines</b></p>
    <p>
    Set up two virtual machines (VMs) within the same Virtual Network (VNet). One VM will function as a Domain Controller (DC-1), while the other will serve as a Client machine (Client-1). We will assign a static IP address to the Domain Controller, as it will provide Active Directory services to the Client. The Client machine will be joined to the domain, and we will configure its DNS settings to use the Domain Controller as its DNS server.
    </p>
    <p>
        <img width="401" alt="image" src="https://github.com/user-attachments/assets/327944b5-f93e-4457-828d-44b40ec52f66">
    </p>
    <p> 
        <b>Step 2- Setting up Private Static IP Address</b></p>

<p>
    <img width="270" alt="image" src="https://github.com/user-attachments/assets/3ada21b7-1c18-43d2-a999-6138ed3dda3f"></p>

<p>
    <img width="359" alt="image" src="https://github.com/user-attachments/assets/e0c4b039-3013-4fcf-bedb-cf3043802b79"></p>
Now that we know what statis IP is, lets assign one to DC-1. Client-1 will establish connectivity with DC-1, and to confirm this connection, a ping test will be conducted from Client-1 to DC-1. Initially, the ping attempt is expected to fail due to existing firewall configurations. To address this, we will enable ICMPv4 on the firewall of DC-1. Once ICMPv4 is activated, Client-1 should be able to successfully ping DC-1, confirming connectivity.


 </p>
    <br />
    <p>
      <img src="https://i.imgur.com/HvZBWzc.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
      </p>
      <img src="https://i.imgur.com/1lrrGPw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
    </p>
    <p>
      We will now log back into DC-1 to proceed with the installation of Active Directory Users and Computers. Next, promote the VM to a Domain Controller, set up a new forest with the domain name "mydomain.com," and then restart the system. After restarting, log back into DC-1 as the user "mydomain.com\labuser." If the steps have been completed correctly, you should be able to run Active Directory Users and Computers, as demonstrated below.
    </p>
    <img src="https://i.imgur.com/cGjvRke.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
    <br />
    </p>
    <hr style="border: 1px solid #ccc; width: 80%; margin: 20px auto;">
    <h1 style="text-align: center; font-weight: bold; font-size: 16pt;">Creating Organizational Units (OUs)</h1> </p> 
    <p>
         Now that AD is install, we now proceed with creating Organizational Units (OUs). First, create an OU named "_EMPLOYEES," followed by another named "_ADMINS." To do this, right-click on the domain area, select New > Organizational Unit, and complete the required fields.</p>
         <P>
         Next, open your newly created OU -> right-click -> select New -> User -> enter Jane Doe -> assign her the role of Admin with the username Jane_admin. </P>
         <p>
         Finally, add Jane to the Domain Admins security group to grant her the appropriate permissions. You can use Jane_admin as the administrator account. 

  </p>
    <img src="https://i.imgur.com/hL7g5Y5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
    <br />
    </p>
    <p>
    <img src="https://i.imgur.com/kcgvzdE.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
  </p>
  <p>
   Next, we will join Client-1 to the "mydomain.com" domain. From the Azure portal, update Client-1’s DNS settings to point to the private IP address of the Domain Controller (DC). After updating the DNS settings, restart Client-1 from within the Azure portal. The image below confirms that Client-1 is successfully registered on the DC-1 DNS.
  </p>
    <img src="https://i.imgur.com/jbrGTXW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
    <br />
    </p>
    <img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
    </p>
    <p>
    </p>
    <hr style="border: 1px solid #ccc; width: 80%; margin: 20px auto;">
    <h1 style="text-align: center; font-weight: bold; font-size: 16pt;">RDP</h1> </p> 
    <p>
    <p>
      To join Client-1 to the domain, navigate to the system settings and select About. On the right, choose Rename this PC (advanced), then select Change under the domain settings. Enter "mydomain.com" as the domain name, followed by the credentials for "mydomain.com\labuser." After completing these steps, the system will restart, and Client-1 will be successfully joined to "mydomain.com."
    </p>
    <br />
    <p>
      <p>
    <img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
    </p>
    <p>
  Client-1 is now successfully joined to the domain. Next, we’ll configure Remote Desktop access for non-administrative users on Client-1. Begin by logging into Client-1 as an administrator and opening System Properties. Navigate to the Remote Desktop tab, and grant "Domain Users" access to Remote Desktop. Once these steps are completed, standard users will be able to log into Client-1 remotely.
    </p>
    <br />
        <p>
      <p>
    <img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
    </p>
    <hr style="border: 1px solid #ccc; width: 80%; margin: 20px auto;">
    <h1 style="text-align: center; font-weight: bold; font-size: 16pt;">Adding Users with Powershell</h1> </p> 
    <p>
    <p>
      Finally, to confirm that standard users can successfully access Client-1 via Remote Desktop, we’ll use a PowerShell script to generate a large number of users in the domain. Once the users are created, we will select one of them and initiate an RDP session into Client-1 to verify access.
    </p>
    <br />
    <img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
    <p>
    <p>
      <p>
    <img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
    </p>
    <img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
    <p>
      As shown, the PowerShell script successfully created a user with the username "bab.hubo." We confirmed that we could log in to Client-1 using this user's credentials, verifying access as a standard user.
    </p>
