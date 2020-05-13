# Create the Virtual Cloud Network #

In this step you will create a VCN with the quick start wizard. This will create all the related network resources including a public subnet, private subnet, internet gateway, NAT gateway, Service gateway, default security lists, and default route rules and table. 

We are taking the quick option, but there is also a custom option to create resources individually. 

## Disclaimer ##
The following is intended to outline our general product direction. It is intended for information purposes only, and may not be incorporated into any contract. It is not a commitment to deliver any material, code, or functionality, and should not be relied upon in making purchasing decisions. The development, release, and timing of any features or functionality described for Oracle’s products remains at the sole discretion of Oracle.

## Requirements ##

- Laptop or desktop computer
- Web browser
- Account access to Oracle Cloud Infrastructure
  - Provided to you if instructor-led training, otherwise use your account
  - You will need an account that allows you to create a VCN, up to two cores of compute, and up to three cores of Autonomous Database.

## Step 1 - Select your Cloud Region ##

1. Select your Region on the upper right of the OCI console

2. Select the cloud services hamburger menu on the top left corner and select Networking

   <img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab300\images\hamburger-menu.PNG" style="zoom:50%;" />

3. Select Virtual Cloud Networks

4. Select your Compartment

5. Click Start VCN Wizard

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab300\images\start-vcn-wizard.PNG)

## Step 2 - Create the Network  ##

1. Select VCN and Internet Connectivity

2. Click Start Workflow

  <img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab300\images\wizard-vcn.PNG" style="zoom:67%;" />

3. Enter a unique name for your VCN

4. Select your assigned compartment

5. Enter a VCN CIDR block of 10.0.0.0/16.  Note: The CIDR Block is the range of IP addresses that can be used.

6. Enter the public subnet CIDR block of 10.0.0.0/24

7. Enter the private subnet CIDR block of 10.0.1.0/24. Note: A private subnet is not visible to the internet and is accessible from inside the VCN

8. Select use DNS hostnames

9. Click Next

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab300\images\vcn-configuration-info.PNG)

A summary is displayed. 

You can view the default security and route rules that will be created.

​	10. Click Show Rules

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab300\images\security-rules.PNG)

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab300\images\route-rules.PNG)

​	11. Click Create. 

The VCN is created instantaneously with all the default network resources. 

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab300\images\vcn-summary-info.PNG)

​	12. Click View Virtual Cloud Network to see the details and what has been created. 

You will see a number of resources created including public, private subnets, default security list, default route table, and the gateways.

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab300\images\view-vcn-config.PNG)

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab300\images\vcn-details.PNG)

Below is a diagram of what has been created by the Networking Quickstart.

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab300\images\lab-architecture-created.PNG)

## Acknowledgements ##

- **Author** - Milton Wan, Database Product Management, April 2020

