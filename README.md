<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
<br />

<h1>Deploying Active Directory in Azure</h1>

<h2>Description</h2>
In this section of the tutorial, I will show off how to install Active Directory on the domain controller, as well as create a domain admin. Then, I will join the client VM to the domain.  <br/>
<br />

Start from here if you want to follow along! -> [Active Directory: Preparing Infrastructure in Azure](https://github.com/cbhylgz/Azure-AD-Preparation) <br />
<br />

<h2>Environments and Utilities Used</h2>

- <b>Microsoft Azure</b>
- <b>Virtual Machines</b>
- <b>Remote Desktop Connection</b>
- <b>Active Directory</b>

<h2>Operating Systems Used </h2>

- <b>Windows Server </b>
- <b>Windows 10</b>

<h2>Steps to Take:</h2>

<p align="center">
Log into the DC (Domain Controller), and if it's not already up, bring up the Server Manager: <br/>
<img src="https://i.imgur.com/aQb4iJ6.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Click on "Add roles and features", then Next twice. For server selection there should only be one to select: <br/>
<img src="https://i.imgur.com/EHrmMTS.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Check "Active Directory Domain Services", then "Add Features":  <br/>
<img src="https://i.imgur.com/auv06iV.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Select "Active Directory Domain Services" then click "Next" three times. Now select "Restart the destination server automatically if required" and click "Install". After that, select "Restart the destination server automatically if required" and click "Install". Next up is to promote the VM as a DC by configuring it as a new forest as "mydomain.com" (or anything you wish if easier for you). Select the flag icon on the top-right side of the Server Manager Dashboard with the caution symbol, and then click "Promote this server to a domain controller":<br/>
<img src="https://i.imgur.com/7QIfWrO.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
<img src="https://i.imgur.com/tWUuLF7.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Click "Add new forest" and in "Root domain name", type "mydomain.com" (or any other URL you please). Afterwards, click "Next":  <br/>
<img src="https://i.imgur.com/DIPlTd0.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Type in the password you will be using, then click on "Next":  <br/>
<img src="https://i.imgur.com/0mbmFsN.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Don't check "Create DNS delegation"; instead click "Next". Click "Next" until you reach this screen, at which point you click "Install". The machine will restart after the installation:  <br/>
<img src="https://i.imgur.com/WcmGn1w.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
It may take some time to restart, but once it does, sign back in. Now, you have to sign in as "mydomain.com\[your username here]" instead of your username only, due to the VM having been promoted to a DC and it needs specification on who's signing in, whether it's a domain user/admin or local user. Since my username is Polybiusb13, mydomain.com\Polybiusb13 is what I'm using as a username to log in: <br/>
<img src="https://i.imgur.com/xO6BMoT.png" height="80%" width="80%" alt="Setting Up in Azure"/> <br />
Next, we'll create an admin user. Search for "Active Directory Users and Computers" in the Start Searchbar:  
<img src="https://i.imgur.com/ArS11GE.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Right click on "mydomain.com" and select "New" > "Organizational Unit" and give it "_EMPLOYEES" as a name. (In a future project we will run a script that uses this name to work). Repeat this again to create an OU (Organizational Unit) named "_ADMINS":  <br/>
<img src="https://i.imgur.com/xZXyq5k.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
<img src="https://i.imgur.com/3VTST4p.png" height="80%" width="80%" alt="Setting Up in Azure"/> <br />
Create a new user in "_ADMINS" by right-clicking on "_ADMINS", then on "New" and "User". I've already made a user named Jane Doe, but for demonstration's sake, this is how you should fill it out: <br />
<img src ="https://i.imgur.com/aBlgzjU.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Next, create a password. Make sure that "User must change password at next logon" is unchecked, and  "Password never expires" is checked. DISCLAIMER: In a real life setting, this probably won't be applied; this is mainly for demonstration.  <br/>
<img src="https://i.imgur.com/Bl1TDNe.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Jane Doe is now apart of the "_ADMINS" OU; however, we still need to make her into an admin. Right-click on her name, then click Properties, Member Of, Add..., Enter "domain admins" , Check Names, OK, Apply, and OK once more:  <br/>
<img src="https://i.imgur.com/AiSPSJh.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Now, log out of DC, and log back in with Jane Doe's login information:  <br/>
<img src="https://i.imgur.com/SmV7vsi.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Once logged in, log into client-1 VM, if not already. Then, join the client to the domain by right-clicking the start menu, then clicking System, Rename this PC (advanced), Change, Select "Domain" and enter "mydomain.com" and click "OK":  <br/>
<img src="https://i.imgur.com/QUuV21L.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
I've already done so, but since it will be your first time, it will ask for an account with permission to join the domain. Since you now have one in the form of Jane Doe's account, use that. Then a pop up welcoming you to the domain will appear and the machine will try and restart. Afterwards, client-1 will now be classified as part of the domain, which you can check in the DC VM by typing out Active Directory Users and Computers and clicking it when it pops up as a result. From there, check within mydomain.com for Computers. client-1 should be inside. Make a new OU (remember, that's Organizational Unit) and name it "_CLIENTS", then right click "client-1" in Computers and select "cut", then click "_CLIENTS" and right click the empty space to the right (with the text "There are no items to show in this view") and click "paste": <br/>
<img src="https://i.imgur.com/QUuV21L.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />

<h2>There you have it-- the Active Directory has been Deployed!<h2>
<br />
<br />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
