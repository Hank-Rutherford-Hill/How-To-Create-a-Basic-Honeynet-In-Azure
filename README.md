# How To Create a Basic Honeynet In Azure

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/412575b5-39c9-4d4a-a220-d3486f67cd4c)

## The Pre-Game

If it was not somehwat obvious to you off of the break, we are going to need to create an Azure account first.  You can do this for free (well, free for 30 days and up to $200 worth of resource credits) by [clicking here](https://azure.microsoft.com/en-us/free/).  By the way, when signing up for this account, you're much better off using a personal email account (an @gmail.com, @yahoo.com, etc.) rather than your school or work email.  The reason for this is that your organization may already have an Azure account.  Thus, you risk being joined into their Azure tenant where you won't have any permissions to create resources & therefore won't be able to create your honeynet!  Additionally, you're going to want to open a notepad to start storing all of your Azure credentials (for your Azure resources, not your actual Microsoft Azure Portal log-in, which you should use the proper safety percautions to guard).  Normally, you should never actually do this.  However, for the sake of this lab, we just want to stay organized ```It's just a lab, folks!``` But yeah, the last thing you want to do is get locked out of your account, or some of your resources.  Here's [a Chrome Extension link](https://chrome.google.com/webstore/detail/note-board-sticky-notes-a/goficmpcgcnombioohjcgdhbaloknabb) to my favorite notepad app.  

Here's a visual reference of what you should be recording in your notepad:

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/4147be9a-77f5-4288-8b5b-fd69904512b4)


Lastly, you will need a credit card to open the account.  You will not be charged anything for the first 30 days.  However, once your 30 days are up, you will be charged for your resources you use.  You may also be charged prior to the 30 day free trial period if the resources you use within the 30 days surpass the $200 of free credit you're allotted upon opening this trial account.  Honestly, if we are just making a honeynet, you won't be using nearly $200 worth of credit in under 30 days, unless you leave your virtual machines running the whole time!  If you want more information on the pricing of certain resources, [click here](https://azure.microsoft.com/en-us/pricing/details/cloud-services/).  If you've made it this far and feel a little "lost in the sauce", you'd probably benefit tremendously by taking this short [Introduction to Azure Fundamentals](https://learn.microsoft.com/en-us/training/modules/describe-cloud-compute/1-introduction-microsoft-azure-fundamentals?source=recommendations) learning module.  You'll then be properly acquainted with cloud computing, and will even recieve a badge upon completion!


```PLEASE use a STRONG PASSWORD for your Azure account, and enable MFA.  The virtual machines in our honeynet don't need to have a strong password (afterall, it's a honeynet), but our Azure account IS NOT part of the honeynet!  So secure your account!```

## Creating Our First Resources

Once you've set up your free Azure account, you are now ready to start getting into the "nitty-gritty".  We'll begin by creating our first resource, a Windows virtual machine.  Obviously, make sure you're logged in to the [Azure Portal](https://portal.azure.com).  

1. Upon logging in, navigate to the search bar at the top of the Azure Portal, and search for "virtual machines".

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/9f89b1d7-15f1-4913-a55a-98572f557a67)

2. Next, click "+ Create" (top left of next screen), and select "Azure Virtual Machine".  We don't want a preset machine, as we need to assure this VM lacks the proper security configurations, making it a HONEYNET!

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/9a6a4ac9-b27a-4961-a084-6b9a1d0af5a9)

3. On the next screen, we need to fill out some basic details about our first VM.  Create this VM under the subscription you just opened when you created your free Azure account.  Under resource group, select "Create New".  A resource group is a logical way to group your Azure resources together, much like a filing system on an operating system.  Make sure you create a unique name for this resource group.  Next, name your VM, select your region (just choose something in the area which you're actually located, just to simplify things).  Leave availability zones and availability alone, and skip down to "Security Type" and "Image".

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/13e7a288-eda4-4eb3-bcd4-1def0ef07a27)

5. For "Security Type", select standard (remember this is a honeynet!), and for image, we are selecting our OS version.  For this VM, just pick a Windows 10 Pro x64 or 11 Pro x64.  Select x64 for "VM Architecture".  Now, for the "Size" section, you want to keep in mind that with a free Azure account, you're only allowed to use a total of 4 vCPU (Virtual Central Processing Units).  We are going to need to create another VM in the honeynet, as well as an Attack VM, so only select an option with 1 or 2 vCPUs here.  Additionally, you don't need to select anything spectacular in regards to memory (8GiB is fine).  It's worth noting that the dollar amount you see next to the various vCPU size selections is the amount you'd be charged for that resource if you were to leave the VM running 24/7 for an entire month.  Considering you can deallocate your VMs at any given time (meaning the resource is turned off and you won't be billed for it being in use), your charges should be MUCH lower.  Next, select a username and password for the admin account for this VM.  Record this info on a notepad on your computer.  Remember, you wouldn't typically do this in any "real life" scenario, as it's a security concern.  However, this is just a lab (& a honeynet lab at that). 

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/638616d1-b477-4dec-a944-7eeb3692b52f)

6. As we continue to scroll, below the username and password section you'll make sure you have selected "Allow Selected Ports" and "RDP (3389).  This will allow all IP addresses to connect to your machine via RDP once it gets configured in the "Networking" secion, which is what we want for this honeynet.  Next, make sure to select the "Confirm" checkbox at the bottom.  Then, select "Next: Disks >".

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/cace74e6-aa1b-4a14-bfdc-3895baa8c8db)


7. We aren't doing anything with the "Disks" section, so click "Next: Networking >".  Under "Networking", we are going to have to create a VNet (virtual network) for our virtual machine to exist on.  So, click "Create New" under the "Virtual Network" dropdown box.  In the following window, name your Vnet, and hit "Ok".  Then, hit "Review + Create", then "Create".

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/d9298605-905e-4409-9f52-fabbea765e00)


8. Nexxt up, we are going to create our Network Security Group (NSG).  This is essentially a layer 3/layer 4 firewall.  Azure offers its own firewall, which is much more resilient.  The one we are making is a mini-firewall.  The thing is, we are going to configure it to let all traffic in because once again, this is a honeynet.  So, go into the main search bar in Azure portal, and search for "Resource Groups".  Click your appropriate resource group, and it'll open up a window with a list of resources.  Among them, you will see your NSG.  By the way, you're able to get to your NSG by searching for it directly into the search box.  You dont necessarily have to go to "Resource Groups" first, in order to find your NSG.  Anyway, click on the NSG and we will get started.

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/7455d394-48cb-4579-954d-cff7d9b05fbf)

9. We need to create a high priority rule that allows all traffic to come in.  In the picture above, the sidebar shows "Inbound security rules".  Select that, then click "+ Add" at the top.  Put the rules as the pictures show, and make special note of the priority.  We want the priority to be a lower value than the first rule on the list, so it is administered first.  Make sure the rest of the NSG rule is the same as it is in the photograph, and click "Create".

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/c9aa166c-4073-47c0-a7b9-38780d7bed83)

10. Now, we're going to go to the Azure portal search bar and search for "Virtual Machines", select your appropriate honeynet virtual machine, and click "Overview".  Copy your VM's public IP address.  Open up command Prompt, and type "ping [IP Address for VM]" and hit enter.

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/d30c94d7-e2ff-48a4-99d7-b46d1fe37c46)

You'll notice the ping has timed out, meaning nothing is getting through to the virtual machine associated with that IP address.  The reason being is that the virtual machine actually has its own Windows firewall inside, so we are going to have to RDP into the VM and misconfigure the firewall.

11.  To RDP into your newly created Windows honeynet VM, ensure your VM is turned on in the Azure portal.  At the search bar on the top, type in "Virtual Machines".  Select the appropriate VM from the list, and in the next pop up window, select "Start".  A pop up notification should appear at the top right of the Azure portal letting you know that the VM has started.  Now, if you're on Windows device, go down to the search bar on your personal workstation and type "RDP".  Remote Desktop Connection will appear.  To select it, hit enter.  If you're using a Mac, you will need to download Remote Desktop for Windows. Regardless of wether you're using a Mac or Windows to RDP into your Windows VM, you'll need to have the IP address of the VM, the the username, and the password.  On Windows, it will look like this:

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/5cb20c6c-05bd-4785-a8eb-27147cf46f5a)
![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/da75cd4a-2512-45e8-90b1-16883cf6034d)

12.  After a few moments, you'll have succesfully logged into your honeynet VM.  Once in the VM, go down to the search bar on the task bar at the bottom.  Type "wf.msc", and hit enter.

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/46e1fcee-8a83-49b4-85c5-45ba019c9c75)

This will bring up the Windows Defender Firewall security settings.  Look under the "Public Profile is Active" section, and click the blue text that says "Windows Defender Firewall Properties".

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/3fde2ef0-aa83-429c-abea-a25e607d4776)

In the next window, where it says "Firewall State", click the dropdown box and turn the firewall off.  Go through each tab at the top ("Domain Profile", "Private Profile", "Public Profile"), and again, where it says "Firewall State", toggle it to "off".  Don't worry about doing anything on the "IP Sec Settings" tab.  Click apply, and we're finished with the task.

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/832564c3-84a1-45f8-ae58-0a5a1d9c8e50)

13.  Now, toggle back to your local device.  Open up the Command Prompt.  If you've closed it, go to the search bar on the task bar at the bottom, type "cmd" and press enter.  Now, type "ping [honenet VM's IP address].

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/cf176609-4f17-42fa-84e2-167175aaf884)

Congratulations!  Traffic is now getting through to the honeynet VM!

14.  Next, we need to download Microsoft SQL Server onto the honeynet VM.  An SQL server is a lucrative target for attackers due to it's data richness, widespread usage, known vulnerabilities, and many more reasons.  If an attacker executes a port scan and realizes port 1433 is active, it's an indicator that a SQL server is running on the target IP machine.  After that, all it takes is for the attacker to banner grab to verify this, and suddenly a whole new realm is opened for the attacker.  A worthwhile honeynet will include a SQL server running on it.  Luckily we'll be able to use a free trial.

Navigate to Google within the VM, and search for "MSSQL Server 2022 trial".  Click this result:

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/584ead2d-9298-4136-bc5d-c45e2288404c)

You'll then be taken to a page where you have to register for a trial.  Fill out the form and continue.

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/968d19d8-0f84-4cdc-9d57-90ceb23c9f9e)

15.  Still inside of the honeynet VM, navigate over to your desktop.  Right click your newly downloaded MSSQL Server, and click "Mount".

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/1967b35e-265f-49e5-ba7a-42005922a82a)


On the following screen, select the EXE download 64 bit edition of MS SQL.  Open the downloaded file, and allow the app to make changes to your device.  On the next screen, hit "Donwload Media".  Then, hit the blue "Browse" folder button, and just keep it simple and save it on the desktop of the VM.  ```Make sure you're downloading an ISO file``` and click "Download".

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/49feb956-1520-4108-9c5f-0b3655a2ac15)

16.  Once finished downloading, go back to the SQL Server 2022 desktop icon.  Right click, and select "Mount" again.  This time, in the window that opens up, click "Setup" from the list, and click "Yes" to allow this app to make changes to your device.

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/a934e86a-645e-4ffb-bd4b-22159d1cc155)

In the next window that opens, near the left of it, select "Installation".  Now, in the right pane of the window, select "New SQL Server standalone installation...".  In the next window that pops up, select that you want to install the "Evaluation" edition, and click next.  Continue to go through the setup propmpts.  When you arrive at this window:

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/4ab4b3f3-cb59-4383-967f-f7129404c203)

Select "Database Engine", and click next.  Keep going, selecting "Next" through the setup.  When you reach this window:

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/6c68f202-ef63-4a82-ad94-3328c4440192)

Select "Mixed Mode".  Using "Mixed Mode" allows one to log into the SQL server using either their Windows credentials (which would be the same credentials used to log into the VM that houses the SQL server, or SA (or SysAdmin) credentials that are separate from the Windows credentials.  In the same window, type your log-in password for your SA account.  Below that, click the "Add Current User" button, and click "Next".  ```"Add Current User" button will allow your Windows account to log in to the SQL Server with the Windows credentials.  Selecting the Password will be the password for the SA account, should you so choose to log in via that method (rather than using your Windows account credentials).```

Continue through the setup prompts and hit "Install".  Once it's fininshed installing, you can close out the windows within your VM associated with SQL server, and voila!  You now have a working instance of MS SQL server running on your Windows honeynet VM that you can log into using either your Windows honeynet VM credentials, ```OR``` your SA account (with the SA account password you selected right before you started the completion of the installation).

## Installing MS SQL Server Management Studio

For a basic honeynet, you do not need to install SQL Server Management Studio.  However, I figured that in the future, I may create an additional tutorial here in Github that involves simulating an attack on the SQL server for the purpose of analyzing logs.  We would be using MS SQL Server Management Studio as a part of this.  So, in the event you want to follow along in the future, it would be wise to complete this step.  If not, feel free to skip; you can always come back to this if you change your mind!

1.  Back in your honeynet VM, navigate to Google.  Search for "SSMS download" and select this search result:

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/6e60c204-ee44-40f3-9a39-3a450308481c)

In the next page, select the free download link.  Once finished downloading, open the folder, and double click the SSMS Setup file that was just downloaded.  On the next window, click "Install", and "Allow app to make changes".  Once it's installed, it may prompt you to restart your VM.  If that's the case, go ahead and let it restart, then RDP back into your honeynet VM.

2.  Now, we need to make some changes within our VM to be able to have the logs associated with attack attempts on our SQL Server show up if we were to look for them in Event Viewer or using KQL queries in Azure.  Specifically, we are going to provide full permissions for the SQL service account to the Windows registry hive.  The Windows registry is basically an app on the computer where you can make a lot of really granular modifications that affect the way the OS runs.  The reason why it's a good idea to make these changes is so you can actually view and understand/get a feel for what these SQL attack-associated logs are, and where to find them (analyzing logs is a crucial part of many cybersecurity roles).  Without making these changes to the registry, we wouldn't be able to view the SQL associated logs in the Event Viewer.

Let's start by navigating to the search bar on the task bar within our honeynet VM, typing "Registry Editor", and hitting enter.

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/0a35f763-2374-4287-8246-d8e505f58494)

Once we are in Registry Editor, we are going to follow this path: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Security

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/bc762973-2ceb-406e-af6c-e2a5ecc2265e)
![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/fd94bf11-9c58-46ad-a15f-16b3b0add25d)
![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/63af608c-85c7-409a-bacb-e8e5247188ab)

Then, under the "Security" tree, you'll see another "Security" tree.  Right click it, and select "Permissions".  Here's an image so you don't get confused between the two "Security" trees:

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/33017f53-8aed-43e4-8ff8-71cbed000b69)

Next, click "Add", and in the "Enter the object names to select" box, type "NETWORK SERVICE".  To the right of the box you just entered "NETWORK SERVICE" into, click the "Check Names" button.  Then, click "OK".  In the window that appears, select the "Full Control" box, and click "Apply" and then "OK".

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/cffdfa7e-64fc-4816-94a8-dcafff831e3f)
![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/b2aa215c-40cd-499c-b6c1-b86f4095cd91)

We need to do one more thing to allow the SQL server logs to be collected and viewed in Event Viewer.  Open up the Command Prompt (task bar search bar > type "cmd" > right click "Command Prompt" > Run as Administrator), and copy this :

```auditpol /set /subcategory:"application generated" /success:enable /failure:enable```

and paste it into the Command Prompt, and hit enter.  Remember, this is being executed in the Command Prompt inside the VM, not your local machine.  

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/deb04596-7505-43e1-a185-4aa3ab5abbe9)

Congratulations, now your VMs OS is configured to collect logs concerning MS SQL Server, allowing them to be examined in Event Viewer or within Azure!

# Verifying SQL Server Log Generation

Before we skate off into the sunset thinking we've set up everything we need to in our Windows honeynet VM, we should do our due dilligence and verify that we can indeed see that these logs are being generated.  To do this, lets open up SSMS (SQL Server Management Studio), and then log in to our SQL Server.

1.  In your Windows honeynet VM, go to the search bar within the task bar at the bottom.  Type "SSMS", and select the SSMS app (not the SSMS setup .exe file).

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/7ca650d6-f078-498b-a6c9-8b7ec102fefe)

Once SSMS is opened, you'll see a window like this: 

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/c9bd5b13-30fa-43ed-8558-bf11393e58dc)

You can either hit "Connect" and you'll log in via your Windows VM credentials, or you can select "SQL Server Authentication" from the "Authentication" section.  If you choose this option, your login would be "sa" (for system administrator), and your password would be the one that you chose when we were configuring the installation of or SQL Server.  ```You should have written this down in your notepad app ;).```

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/486b8efe-a67b-4b8a-ae6d-41c0a5dd62f1)

2.  Once connected to your SQL server (by windows authentication or sa authentication, doesn't matter), right click on your server (which should be at the very top of the "Object Explorer" on the left side of SSMS) and select "Properties".

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/06841f1e-0572-4faa-9ef9-a357ecfd9a3e)

3.  Select "Security" from the menu on the left.  Under the "Login Auditing" section, choose "Both failed and successful logins".  Select "OK".

In conjunction with the Windows Registry changes we made a few steps prior, this will make sure both failed and successful log in attempts are ported to the Event Viewer (and thus Azure).

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/6ae12168-e22e-4729-aead-5b98a87cae40)

4. After making a change like this, it's a good idea to restart the SQL server.  Right click the server under the Object Explorer section, and select restart.


![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/27098401-6d07-4781-a042-caa2a820bb05)

5.  Once we've restarted the SQL server, right click your server under the Object Explorer and select disconnect.  This will log us out of the server.

6.  Under the Object Expolorer, you'll see "Connect" with a plug next to it.  Click the plug, and a login window will come up.

7.  Select SQL Server Authentication from the dropdown under the "Authentication" section, put "sa" in the login box, and type an ```INCORRECT``` password.  Continue to fail a login at least 3-4 more times.  We are about to check in the Event Viewer that both failed and successful logins are being logged.

8.  Finally, login successfully.

9.  Now, disconnect from your SQL server, and exit SSMS.

10.  Open Event Viewer by typing "Event Viewer" into the search bar on the task bar at the bottom of your VM.

11.  Looking on the left pane under the "Windows Logs" tree, select "Application".  Scroll through the logs, looking for Event ID 18453 (successful login) and Event ID 18456 (Failed Login).

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/fcf87c0a-882e-4fa1-87bd-a56d67a7746b)

If after failing several login attempts, you do not see any Event ID 18456 for failed logins, or any Event ID 18453 for successful logins, go back and repeat the steps in this section.  If you repeat the steps in this section, and still do not see the appropriate Event IDs in Event Viewer, you may have made a mistake when you made changes to the Windows Registry, and you would have to go back and repeat that section, followed by this section.  However, it's not totally necessary if all you want to do is create a super basic honeynet.  If you think you may want to follow along with any future tutorials I may release here on GitHub, it would behoove you to get this situation worked out.

# Creating A Linux VM
