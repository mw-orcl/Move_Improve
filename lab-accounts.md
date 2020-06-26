# **Lab Accounts**

This is your account to log in to the Oracle Cloud.

Oracle Cloud URL:  

Cloud Tenant:  

User Name:  

Password:  



## Region

You will be doing your labs in one of the Oracle Cloud regions.  There are 3 regions you can choose from:  South Korea (Chuncheon), India South (Hyderabad),  Australia Southeast (Melbourne).

Select the region closest to you. 

<img src="./images/region.PNG" style="zoom:50%;" />



## Compartment

All your lab work will be done in a compartment.  A compartment is a workspace which holds all your cloud resources.   For this lab everyone will be using the same compartment.  

Compartment:  APAC-Workshop-1

<img src="./images/compartment.PNG" style="zoom:50%;" />



## Object Store Password

This password will be used by the database to access the Oracle Object Store in one of the labs.  Note it for now.

Auth Token Password:  



## SSH Keys

For an instructor-led workshop, you can use the SSH keys provided below.   Alternatively you can create your own with a tool like puTTYgen (Windows) or ssh-keygen (Linux or Mac).

Public key:  labkey.pub

Private key: labkey.ppk (Windows) or labkey (Mac)

Keys can be downloaded here:  



The keys will be used to securely connect to your compute instance on the Oracle Cloud.  You will provision your compute instance with the public key, **labkey.pub**.  

From a Windows client, you will use **labkey.ppk** with PuTTY to connect to your instance.  For Mac clients, you will use the **labkey** file with no extension to connect to your instance.  .  

Note: SSH keys are required to access Oracle Cloud compute instances.  