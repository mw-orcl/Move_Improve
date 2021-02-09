# Create Autonomous Database #

Oracle Autonomous Transaction Processing (ATP) is a fully managed Oracle database service with “self-driving” features on the Oracle Cloud Infrastructure (OCI). An application can securely connect to ATP with the proper credentials and a wallet. 

The following lab guide shows how to provision and securely connect to ATP. 

## Disclaimer ##
The following is intended to outline our general product direction. It is intended for information purposes only, and may not be incorporated into any contract. It is not a commitment to deliver any material, code, or functionality, and should not be relied upon in making purchasing decisions. The development, release, and timing of any features or functionality described for Oracle’s products remains at the sole discretion of Oracle.

## Requirements ##

- Web Browser
- SQL Developer 19.1 or higher

## Step 1: Login to Oracle Cloud ##

​	1. From your browser login into Oracle Cloud

### About Regions and compartments

**Important Note**

Always ensure you are in your correct Region and Compartment. 

If this is an instructor-led lab we are sharing the same tenancy account with multiple students, please create a unique name for your OCI resources that you can identify with. Ie: Use your name or other identifier unique to you to name your cloud resources.

## Step 2: Provision ATP ##

Provision the Autonomous Transaction Processing database (ATP) with the steps below.

​	1. Select your assigned Region from the upper right of the OCI console.

​	2. From the hamburger menu (top left side), select Autonomous Transaction Processing.

​         <img src="./images/provision-atp-1.PNG" style="zoom: 67%;" />                          

​	3.  Select your Compartment. You may have to drill in (click “+”) to see your compartment.

​	4. Select Workload Type Transaction Processing.

​	5. Click Create Autonomous Database. 

 <img src="./images/provision-atp-3.PNG" style="zoom:67%;" />

​	6. Choose your compartment.

​	7. Enter any unique name (maybe your name) for your display and database name. The display name is used in the Console UI to identify your database.



<img src="./images/provision-atp-4.PNG" style="zoom:67%;" />



​	8. Ensure Transaction Processing workload type is selected.

​	9. Select **Shared Infrastructure** for deployment type.

​	10. Choose database version 19c.



<img src="./images/provision-atp-5.png" style="zoom: 67%;" />

​	11. Configure the database with **2 cores and 1 TB storage**.

​	12. Uncheck Auto scaling. We will enable it later.

​	13. Enter a password. The username is always ADMIN. (Note: remember your password)



<img src="./images/provision-atp-6.png" style="zoom:67%;" />

​	

​	14. Select Allow secure access from everywhere for this workshop.  As a best practice when you deploy your own application, you should select network access to be from Virtual cloud network.  

​	15. Do not check the box Configure Access Control Rules. However the best practice is to configure Access Control to your ATP.



<img src=".\images\network-access.PNG" style="zoom:67%;" />

​	16. Select BYOL license type.

​	17. Click Create Autonomous Database.

<img src="./images/provision-atp-7.PNG" style="zoom: 67%;" />



Your console will show that ATP is provisioning. This will take about 2 or 3 minutes to complete.

<img src="./images/provision-atp-8.png" style="zoom:67%;" />

You can check the status of the provisioning in the Work Request.

![](./images/provision-atp-9.png)

## Step 3: Download the Wallet

Once your ATP service is running we can connect a client to ATP securely with the Oracle Wallet.

​	1. Click on the details of your ATP.

​	2. Select DB Connection.



<img src="./images/wallet-1.PNG" style="zoom:67%;" />

​	3. Select Instance Wallet for the Wallet Type.

​	4. Download the wallet to your laptop.

​	5. Enter a password for the wallet.



<img src="./images/wallet-2.PNG" style="zoom:67%;" />

<img src="./images/wallet-3.png" style="zoom: 67%;" />



## Step 4: Connect to ATP using SQL Developer

In this lab section you will connect to the ATP with Oracle SQL Developer and browse the ATP configuration. SQL Developer is an Oracle DBA and developer client tool.

Please note that most of the database settings and parameters cannot be modified in a fully-managed Oracle Autonomous Database (ATP and ADW) and that is the whole point of the autonomous service, it runs by itself. 

### Start SQL Developer

1. Start SQL Developer from your client.
2. Click + to create a new connection.

​            <img src="./images/sql-developer.PNG" style="zoom: 67%;" />                   

​	3. Enter a connection name.

​	4. Enter ADMIN as the user.

​	5. Enter the password you used to create your ATP.

​	6. Check Save Password.

​	7. Select Connection Type as Cloud Wallet and Browse for your wallet.

​	8. Browse and select your service. Ie: <your ATP name>_tp. Note there are five services, select the **tp** service.

About connection services:

- High – for long queries, high parallelism, low SQL concurrency

- Medium – for medium queries, parallelism, medium concurrency

- Low – for short queries, no parallelism, high concurrency

- TPurgent – for high priority transaction processing

- TP – for standard transaction processing

  

​	9. Test the connection and Save your connection for later use. Then click Connect.

**Note:** Ensure that you use **ADMIN** user to view any database configuration.

<img src="./images/sql-developer-2.PNG" style="zoom: 67%;" />

​	10. From your SQL Developer worksheet run the test query below against a sample database that is already in ATP.

```
SELECT channel_desc,
 TO_CHAR(SUM(amount_sold),'9,999,999,999') SALES$,
 RANK() OVER (ORDER BY SUM(amount_sold)) AS default_rank,
 RANK() OVER (ORDER BY SUM(amount_sold) DESC NULLS LAST) AS custom_rank
 FROM sh.sales, sh.products, sh.customers, sh.times, sh.channels, sh.countries
 WHERE sales.prod_id=products.prod_id

AND sales.cust_id=customers.cust_id
 AND customers.country_id=countries.country_id
 AND sales.time_id=times.time_id
 AND sales.channel_id=channels.channel_id
 AND times.calendar_month_desc IN ('2000-09','2000-10')
 AND country_iso_code='US'
 GROUP BY channel_desc;
```



 11. Click **F5** or the **Run Script** button. Verify the query executes and results are displayed.  

     Run it again.  Why was it faster?
     
     <img src="./images/sql-developer-3.PNG" style="zoom:67%;" />



You have successfully provisioned and connected SQL Developer to Autonomous Database (ATP) and validated the connection. 

<img src="./images/atp-diagram.PNG" style="zoom: 80%;" />

## Acknowledgements ##

- **Author** - Milton Wan, Database Product Management, April 2020

