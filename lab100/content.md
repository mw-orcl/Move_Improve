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

## Step 1 ##

- Select your Region on the upper right of the OCI console
- Select the cloud services menu on the top left corner and select Networking
- Select Virtual Cloud Networks
- Select your Compartment
- Click Networking Quickstart

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab100\images\Region and compartment selection.PNG)

## Step 2 ##

- Select VCN and Internet Connectivity

- Click Start Workflow

  ![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab100\images\Network Quickstart.PNG)

- Enter a unique name for your VCN
- Select your assigned compartment
- Enter a VCN CIDR block of 10.0.0.0/16.  Note: The CIDR Block is the range of IP addresses that can be used.
- Enter the public subnet CIDR block of 10.0.0.0/24
- Enter the private subnet CIDR block of 10.0.1.0/24. Note: A private subnet is not visible to the internet and is accessible from inside the VCN

- Select use DNS hostnames
- Click Next

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab100\images\VCN configuration info.PNG)

A summary is displayed. 

You can view the default security and route rules that will be created.

- Click Show Rules

  ![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab100\images\Security Rules.PNG)

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab100\images\Route Rules.PNG)

- Click Create. 

The VCN is created instantaneously with all the default network resources. 

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab100\images\VCN summary info.PNG)

- Click View Virtual Cloud Network to see the details and what has been created. 

You will see a number of resources created including public, private subnets, default security list, default route table, and the gateways.

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab100\images\View VCN config.PNG)

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab100\images\VCN details.PNG)

Below is a diagram of what has been created by the Networking Quickstart.

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab100\images\Lab architecture created.PNG)

## Acknowledgements ##

- **Author** - Milton Wan, Database Product Management, PTS - April 2020

