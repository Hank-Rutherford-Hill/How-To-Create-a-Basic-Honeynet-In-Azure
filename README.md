# How To Create a Basic Honeynet In Azure

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/412575b5-39c9-4d4a-a220-d3486f67cd4c)


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

4. For "Security Type", select standard (remember this is a honeynet!), and for image, we are selecting our OS version.  For this VM, just pick a Windows 10 Pro x64 or 11 Pro x64.  Select x64 for "VM Architecture".  Now, for the "Size" section, you want to keep in mind that with a free Azure account, you're only allowed to use a total of 4 vCPU (Virtual Central Processing Units).  We are going to need to create another VM in the honeynet, as well as an Attack VM, so only select an option with 1 or 2 vCPUs here.  Additionally, you don't need to select anything spectacular in regards to memory (8GiB is fine).  It's worth noting that the dollar amount you see next to the various vCPU size selections is the amount you'd be charged for that resource if you were to leave the VM running 24/7 for an entire month.  Considering you can deallocate your VMs at any given time (meaning the resource is turned off and you won't be billed for it being in use), your charges should be MUCH lower.  Next, select a username and password for the admin account for this VM.  Record this info on a notepad on your computer.  Remember, you wouldn't typically do this in any "real life" scenario, as it's a security concern.  However, this is just a lab (& a honeynet lab at that). 

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/638616d1-b477-4dec-a944-7eeb3692b52f)

5. As we continue to scroll, below the username and password section you'll make sure you have selected "Allow Selected Ports" and "RDP (3389).  This will allow all IP addresses to connect to your machine via RDP once it gets configured in the "Networking" secion, which is what we want for this honeynet.  Next, make sure to select the "Confirm" checkbox at the bottom.  Then, select "Next: Disks >".

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/cace74e6-aa1b-4a14-bfdc-3895baa8c8db)


6. We aren't doing anything with the "Disks" section, so click "Next: Networking >".  Under "Networking", we are going to have to create a VNet (virtual network) for our virtual machine to exist on.  So, click "Create New" under the "Virtual Network" dropdown box.  In the following window, name your Vnet, and hit "Ok".  Then, hit "Review + Create", then "Create".

   ![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/d9298605-905e-4409-9f52-fabbea765e00)


7. Nexxt up, we are going to create our Network Security Group (NSG).  This is essentially a layer 3/layer 4 firewall.  Azure offers its own firewall, which is much more resilient.  The one we are making is a mini-firewall.  The thing is, we are going to configure it to let all traffic in because once again, this is a honeynet.  So, go into the main search bar in Azure portal, and search for "Resource Groups".  Click your appropriate resource group, and it'll open up a window with a list of resources.  Among them, you will see your NSG.  By the way, you're able to get to your NSG by searching for it directly into the search box.  You dont necessarily have to go to "Resource Groups" first, in order to find your NSG.  Anyway, click on the NSG and we will get started.

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/7455d394-48cb-4579-954d-cff7d9b05fbf)


