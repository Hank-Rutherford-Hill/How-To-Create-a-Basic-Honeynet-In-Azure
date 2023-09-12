# How To Create a Basic Honeynet In Azure

![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/412575b5-39c9-4d4a-a220-d3486f67cd4c)


If it was not somehwat obvious to you off of the break, we are going to need to create an Azure account first.  You can do this for free (well, free for 30 days and up to $200 worth of resource credits) by [clicking here](https://azure.microsoft.com/en-us/free/).  By the way, when signing up for this account, you're much better off using a personal email account (an @gmail.com, @yahoo.com, etc.) rather than your school or work email.  The reason for this is that your organization may already have an Azure account.  Thus, you risk being joined into their Azure tenant where you won't have any permissions to create resources & therefore won't be able to create your honeynet!  Additionally, you're going to want to open a notepad to start storing all of your Azure credentials.  The last thing you want to do is get locked out of your account, or some of your resources.  Here's [a Chrome Extension link](https://chrome.google.com/webstore/detail/note-board-sticky-notes-a/goficmpcgcnombioohjcgdhbaloknabb) to my favorite notepad app.  

Lastly, you will need a credit card to open the account.  You will not be charged anything for the first 30 days.  However, once your 30 days are up, you will be charged for your resources you use.  You may also be charged prior to the 30 day free trial period if the resources you use within the 30 days surpass the $200 of free credit you're allotted upon opening this trial account.  Honestly, if we are just making a honeynet, you won't be using nearly $200 worth of credit in under 30 days, unless you leave your virtual machines running the whole time!  If you want more information on the pricing of certain resources, [click here](https://azure.microsoft.com/en-us/pricing/details/cloud-services/).  If you've made it this far and feel a little "lost in the sauce", you'd probably benefit tremendously by taking this short [Introduction to Azure Fundamentals](https://learn.microsoft.com/en-us/training/modules/describe-cloud-compute/1-introduction-microsoft-azure-fundamentals?source=recommendations) learning module.  You'll then be properly acquainted with cloud computing, and will even recieve a badge upon completion!


```PLEASE use a STRONG PASSWORD for your Azure account, and enable MFA.  The virtual machines in our honeynet don't need to have a strong password (afterall, it's a honeynet), but our Azure account IS NOT part of the honeynet!  So secure your account!```

## Creating Our First Resources

Once you've set up your free Azure account, you are now ready to start getting into the "nitty-gritty".  We'll begin by creating our first resource, a Windows virtual machine.  Obviously, make sure you're logged in to the [Azure Portal](https://portal.azure.com).  

1. Upon logging in, navigate to the search bar at the top of the Azure Portal, and search for "virtual machines".
![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/9f89b1d7-15f1-4913-a55a-98572f557a67)

2. Next, click "+ Create" (top left of next screen), and select "Azure Virtual Machine".  We don't want a preset machine, as we need to assure this VM lacks the proper security configurations, making it a HONEYNET!

  ![image](https://github.com/Hank-Rutherford-Hill/How-To-Create-a-Basic-Honeynet-In-Azure/assets/143474898/9a6a4ac9-b27a-4961-a084-6b9a1d0af5a9)

3. 

