# Create the Virtual Cloud Network #

In this lab you will create a Virtual Cloud Network (VCN) and related network resources. A VCN is similar to your own on premise enterprise network.  In the Oracle Cloud, the network is software defined and virtual. This makes for very fast creation, but still providing high performance, and complete security. The architecture of your lab design follows.

<img src="./images/lab-architecture-diagram.png" style="zoom:67%;" />

## Disclaimer ##
The following is intended to outline our general product direction. It is intended for information purposes only, and may not be incorporated into any contract. It is not a commitment to deliver any material, code, or functionality, and should not be relied upon in making purchasing decisions. The development, release, and timing of any features or functionality described for Oracle’s products remains at the sole discretion of Oracle.

## Requirements ##

- Laptop or desktop computer

- Web browser

- Account access to Oracle Cloud Infrastructure

- You will need an account that allows you to create a VCN, up to two cores of compute, and up to three cores of Autonomous Database

  

### About Regions and compartments

**Important Note**

Always ensure you are in your correct Region and Compartment. 

If this is an instructor-led lab we are sharing the same tenancy account with multiple students, please create a unique name for your OCI resources that you can identify with. Ie: Use your name or other identifier unique to you to name your cloud resources.



## Step 1 - Select your Cloud Region ##

In this step you will create a VCN with the quick start wizard. This will create all the related network resources including a public subnet, private subnet, internet gateway, NAT gateway, Service gateway, default security lists, and default route rules and table. 

We are taking the quick option, but there is also a custom option to create resources individually. 

1. Select your Region on the upper right of the OCI console

2. Select the cloud services hamburger menu on the top left corner and select Networking

   <img src="./images/hamburger-menu.PNG" style="zoom:50%;" />

3. Select Virtual Cloud Networks

4. Select your Compartment

5. Click Start VCN Wizard

![](./images/start-vcn-wizard.PNG)

## Step 2 - Create the Network  ##

1. Select VCN and Internet Connectivity

2. Click Start VCN Wizard

  <img src="./images/wizard-vcn.PNG" style="zoom:67%;" />

3. Enter a unique name for your VCN

4. Select your assigned compartment

5. Enter a VCN CIDR block of 10.0.0.0/16.  Note: The CIDR Block is the range of IP addresses that can be used.

6. Enter the public subnet CIDR block of 10.0.0.0/24

7. Enter the private subnet CIDR block of 10.0.1.0/24. Note: A private subnet is not visible to the internet and is accessible from inside the VCN only.

8. Select use DNS hostnames

9. Click Next

![](./images/vcn-configuration-info.PNG)

A summary is displayed. 

You can view the default security and route rules that will be created.

​	10. Scroll to Security List and click Show Rules

<img src="./images/security-rules.PNG" style="zoom:67%;" />

<img src="./images/route-rules.PNG" style="zoom:67%;" />



​	11. Click Create. 

The VCN is created instantaneously with all the default network resources. 

![](./images/vcn-summary-info.PNG)

​	

12. Click View Virtual Cloud Network to see the details and what has been created. 

![](./images/view-vcn-config.PNG)



You will see a number of resources created including public, private subnets, default security list, default route table, and the gateways.

![](./images/vcn-details.PNG)



Below is a diagram of what has been created by the Networking Quickstart.  All these resources were created in seconds.

![](./images/lab-architecture-created.PNG)

## Acknowledgements ##

- **Author** - Milton Wan, Database Product Management, April 2020

