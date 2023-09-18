# How To Create a Basic Honeynet In Azure

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/412575b5-39c9-4d4a-a220-d3486f67cd4c)

# The Pre-Game

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

14.  Next, we need to download Microsoft SQL Server onto the honeynet VM.  For this, we'll use a free trial.  Navigate to Google within the VM, and search for "MSSQL Server 2022 trial".  Click this result:

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/584ead2d-9298-4136-bc5d-c45e2288404c)

You'll then be taken to a page where you have to register for a trial.  Fill out the form and continue.

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/968d19d8-0f84-4cdc-9d57-90ceb23c9f9e)

On the following screen, select the EXE download 64 bit edition of MS SQL.  Open the downloaded file, and allow the app to make changes to your device.  On the next screen, hit "Donwload Media".  Then, hit the blue "Browse" folder button, and just keep it simple and save it on the desktop of the VM.  ```Make sure you're downloading an ISO file``` and click "Download".

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/49feb956-1520-4108-9c5f-0b3655a2ac15)
