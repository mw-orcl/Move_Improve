# Create the Computes #

Oracle Cloud computes come in many shapes.  They can be virtual machines or bare metal hardware.  In this lab we will create two virtual machines.  One will be a bastion server, sometimes called a "jump server", and it will be created in your public subnet and the second will be the application server and it will created in your private subnet.

## Disclaimer ##
The following is intended to outline our general product direction. It is intended for information purposes only, and may not be incorporated into any contract. It is not a commitment to deliver any material, code, or functionality, and should not be relied upon in making purchasing decisions. The development, release, and timing of any features or functionality described for Oracle’s products remains at the sole discretion of Oracle.

## Requirements ##

- Web browser
- SSH public and private keys
- WinSCP or equivalent
- PuTTY or equivalent

## Step 1 ##

We will create our first compute as a Bastion host on the public subnet. A Bastion is a “jump” host that will allow us to jump to the private compute. Private computes on private subnets can not be accessed from the internet directly.

- Log in to Oracle Cloud
- Click on the navigation menu on the top left, select Compute Instances
- Click Create Instance

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab200\images\Menu compute.PNG)

- Give your instance a unique name.
- Change the Image and Select Oracle Cloud Developer Image from Oracle Images 

The Oracle Cloud Developer image has the Linux OS along with software client tools we will use. 

- After selecting the image, you must scroll down the screen and click Select

  ![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab200\images\Create compute image.PNG)

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab200\images\Select Developer Image.PNG)

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab200\images\Select Image.PNG" style="zoom: 80%;" />

- Select Availability Domain 1 (AD1)
- Select Virtual Machine 
- Instance Shape should be 1 Core OCPU, 15 GB Memory. You can keep this default.
- Select your Compartment and the VCN you created earlier
- Select your subnet Compartment.  It will be the same as your VCN Compartment in this lab. In some cases you can create subnet in other compartments. 

- Select the **Public** subnet to create your compute on

- Then select **Assign public IP address**

- Keep the Boot Volume settings as is, with boxes unchecked

- Choose the SSH public key provided by the instructor or create your own with PuttyGen or equivalent

- Create your Instance

  ![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab200\images\Compute config 1.png)

  ![Compute config 2](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab200\images\Compute config 2.png)

  ![Compute config 3](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab200\images\Compute config 3.png)

  ![Compute config 4](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab200\images\Compute config 4.png)

  ![Compute config 5](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab200\images\Compute config 5.PNG)

### Substep 1.1 ###

Description of substep 1.1

### Substep 1.2 ###

Description of substep 1.2

## Step 2 ##

etc

## Acknowledgements ##

- **Author** - Robert Pastijn, Database Product Management, PTS EMEA - April 2020

