# Create the Computes #

Oracle Cloud computes come in many shapes.  They can be virtual machines or bare metal hardware.  In this lab we will create two virtual machines.  One will be a bastion server, sometimes called a "jump server", and it will be created in your public subnet, and the second will be the application server and it will created in your private subnet.

## Disclaimer ##
The following is intended to outline our general product direction. It is intended for information purposes only, and may not be incorporated into any contract. It is not a commitment to deliver any material, code, or functionality, and should not be relied upon in making purchasing decisions. The development, release, and timing of any features or functionality described for Oracle’s products remains at the sole discretion of Oracle.

## Requirements ##

- Web browser
- SSH public and private keys
  - For instructor-led class, your instructor may provide you with the SSH keys
  - You may also create your own SSH keys with PuTTyGen or equivalent
- WinSCP or equivalent
- PuTTY or equivalent

## Step 1: Create the Bastion Compute ##

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\bastion-diagram.PNG" style="zoom: 150%;" />

We will create our first compute as a Bastion host on the public subnet. A Bastion is a “jump” host that will allow us to jump to the private compute. Private computes on private subnets can not be accessed from the internet directly.

​	1. Log in to Oracle Cloud

​	2. Click on the navigation menu on the top left, select Compute Instances

​	3. Click Create Instance

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\menu-compute.PNG)

​	4. Give your instance a unique name.

​	5. Change the Image and Select Oracle Cloud Developer Image from Oracle Images 

The Oracle Cloud Developer image has the Linux OS along with software client tools we will use. 

​	6. After selecting the image, you must scroll down the screen and click Select

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\Create-compute-image.PNG)

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\select-developer-image.PNG)

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\select-image.PNG" style="zoom: 80%;" />

​	7. Select Availability Domain 1 (AD1)

​	8. Select Virtual Machine 

​	9. Instance Shape should be 1 Core OCPU, 15 GB Memory. You can keep this default.

​	10. Select your Compartment and the VCN you created earlier

​	11. Select your subnet Compartment.  It will be the same as your VCN Compartment in this lab. In some cases you can create subnet in other compartments. 

​	12. Select the **Public** subnet to create your compute on

​	13. Then select **Assign public IP address**

​	14. Keep the Boot Volume settings as is, with boxes unchecked

​	15. Choose the SSH public key provided by the instructor or use your own 

​	16. Create your Instance

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\compute-config-1.png)

![Compute config 2](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\compute-config-2.png)

![Compute config 3](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\compute-config-3.png)

![Compute config 4](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\compute-config-4.png)

![Compute config 5](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\compute-config-5.PNG)

Once you click Create Instance, your instance will be in provisioning state. This will take a couple of minutes to create.

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\compute-provisioning-status.png)

You can see the more status by clicking on Work Request.

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\work-request.PNG" style="zoom:67%;" />

Your running compute instance will have both the Public and Private IP address created. Resources inside the VCN can access Private IP addresses.

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\compute-ip-detail.jpg)



## Step 2: Connect to your Bastion Compute ##

Use PuTTy or SSH to connect to your bastion compute. Connect with the compute public IP address and your client side SSH private key provided by the instructor or use your own SSH key. You can use PuTTyGen or equivalent to create your own SSH public and private keys.  opc is the default admin user for the compute

For Linux SSH:

`ssh –i ~/../privatekey opc@<your compute public IP address>`

For PuTTY:

​	1. Enter the public IP address of the compute

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\putty-1.PNG" style="zoom:75%;" />

​	2. Browse for your SSH private key

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\putty-2.PNG" style="zoom: 67%;" />

​	3. Save your settings and click Open

​	4. Click Yes when prompted

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\putty-3.PNG" style="zoom:67%;" />

Oracle computes are provisioned with the default **opc** user with sudo privileges.

​	5. Login as **opc**

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\putty-session.png" style="zoom:75%;" />



## Step 3: Create your Application Server

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\app-server-diagram.PNG)

1. On the Private subnet create a compute for your application server. 
2. Name your instance as your app server.
3. If you have more than one availability domain, you can just select AD1.
4. Use the latest default Oracle Linux image.
5. Please use the standard virtual machine with 1 core as pictured.
6. Ensure you select your compartment. Your subnet compartment can actually be configured on another compartment from your VCN. But in this case it will be the same as your VCN.
7. Select the Private subnet where the App Server will be installed.
8. Use the same SSH key as your bastion compute.  However the best practice is to use a different key.
9. Click Create.

  ![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\create-app-server-1.png)             

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\create-app-server-2.png)

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\create-app-server-3.PNG)

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\compute-config-4.png)

You can see the work request and status of the compute being provisioned in the compute details and Work Requests page.

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\create-app-server-5.png"  />

Once your compute App Server instance is running, view other details. Note the compute is provisioned on the private subnet, you only get a private IP address.  There is no public IP address.  From the private subnet we have a secure App Server that will connect to ATP.

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\app-server-details.png)

Now we need to turn off VNIC source/destination check to allow packets to be forwarded.  Otherwise if the packet is not for the VNIC it will be dropped.

## Step 4: Turn off VNIC source/destination check

​	1. View the compute App Server instance details

​	2. Select Attached VNICs

​	3. Click on the 3 dots on the right of the VNIC

​	4. Select Edit VNIC

​	5. Check the box **Skip Source/Destination Check**

​	6. Click Update the VNIC

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\vnic-details.PNG)

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\update-vnic.PNG)



## Step 5: Check Security List for Each Subnet

Let's see how your security list are set up to ensure they allow the right traffic.

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\security-list-diagram.PNG)

​	1. Navigate to your VCN details

​	2. Select Security Lists from your Resources

​	3. Check default security list used by the public subnet

		- Allow TCP ingress on port 22. (port 22 is for SSH)
		- Allow all egress to all destinations, ie: 0.0.0.0/0

​	ICMP is defaulted and is TCP/IP layer protocol for control and error messages.

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\vcn-details.png)

​	![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\ingress-rules.png)

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\egress-rules.png)

1. Check security list for private subnet. 

		- Ie: Ingress allows SSH from the VCN (this will also allow ingress for SCP copy of the ATP Wallet we will do shortly). 
		- Ie: Egress allows all protocol traffic to all destination.



![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\seclist-ingress-private-subnet.png)

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab400\images\security-list-egress-private-subnet.png)

## Acknowledgements ##

- **Author** - Milton Wan, Database Product Management, April 2020

