# Install the Application and Migrate the Database #

This lab will demonstrate the move and improve process to OCI and ATP. We assume that the original application and database were running on premise.  We will now install the application on the App Server and migrate the on premise Oracle database to ATP. We will then run the application workload against the ATP database, scale the workload, and see why ATP’s capability is an improvement over an on premise database. 

The sample application we will use is Swingbench with an associated database to store the data for Swingbench, but imagine this is your own application. This is the flow of the move and improve process.

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab500\images\Move Improve flow diagram.png" style="zoom:75%;" />

## Disclaimer ##
The following is intended to outline our general product direction. It is intended for information purposes only, and may not be incorporated into any contract. It is not a commitment to deliver any material, code, or functionality, and should not be relied upon in making purchasing decisions. The development, release, and timing of any features or functionality described for Oracle’s products remains at the sole discretion of Oracle.

## Requirements ##

- Web Browser
- SSH public and private keys
- PuTTY or equivalent
- SQL Developer 19.1 or higher
- Soedump18C_1G.dmp file (provided by Instructor in instructor-led class)
- OCI Auth Token password (provided by Instructor in instructor-led class)

## Step 1: Install Swingbench on the App Server ##

Swingbench is a popular application that can be set up to drive a workload against the Oracle database. We will configure it to drive many OLTP transactions so it saturates the database CPU cores to near 100% utilization.

Install and configure the Swingbench application on the App Server.

​	1. SSH to your App Server. Replace with your SSH key and private IP address.

```
$ ssh -i privatekey opc@10.0.1.2 
```

By default, Java is not installed on the compute VM so we will need to install it via yum. We’ll need to update yum first. 

​	2. From your App Server session execute the following.

```
$ sudo yum makecache fast
```

​	3. Then install Java and its dependencies

```
$ sudo yum install java-1.8.0-openjdk-headless.x86_64
```

•      Type **y**

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab500\images\Java install.png" style="zoom:75%;" />



​	4. Let’s check the version and make sure that java works correctly.

```
$ java -version
```

​	5. We can now pull the Swingbench code from the website

```
$ curl http://www.dominicgiles.com/swingbench/swingbench261082.zip -o swingbench.zip
```

​	6. Unzip it and check the files

```
$ unzip swingbench.zip

$ cd swingbench/bin

$ ls
```

The application is now installed. You should see the Swingbench application files. 

Now we will install the Oracle Instant Client software which has tools to help us move the on premise database to the Oracle Cloud.   

## Step 2: Install the Oracle Instant Client 

To prepare for moving the Swingbench database to ATP, we need the Data Pump import tool. It’s in the Oracle Instant Client software. We will install the Oracle Instant Client software packages from the yum server.

​	1. Connect to your App Server.

​	2. Get the latest repository version from the closest region yum server

```
$ cd /etc/yum.repos.d

$ export REGION=`curl http://169.254.169.254/opc/v1/instance/ -s | jq -r '.region'| cut -d '-' -f 2`

$ echo $REGION

$ sudo -E wget http://yum-$REGION.oracle.com/yum-$REGION-ol7.repo
```

​	3. Enable the Instant Client repository

```
$ sudo yum-config-manager --enable ol7_oracle_instantclient
```

​	4. List the packages

```
$ sudo yum list oracle-instantclient*
```

​	5. Install the latest Instant Client Basic, SQL Plus, Tools RPM packages. 

```
$ sudo yum install -y oracle-instantclient18.5-basic oracle-instantclient18.5-sqlplus oracle-instantclient18.5-tools
```



## Step 3: Configure the Instant Client software

​	1. Locate the ATP wallet and unzip it to a wallet folder. Note the extracted files cwallet.sso, sqlnet.ora and tnsnames.ora. Replace the names with your own files.  ie: replace the sample Wallet_ATPLABTEST with your own wallet.

```
$ cd /home/opc

 $ mkdir Wallet_ATPLABTEST

 $ mv Wallet_ATPLABTEST.zip Wallet_ATPLABTEST

 $ cd Wallet_ATPLABTEST

 $ unzip Wallet_ATPLABTEST.zip

 $ ls
```

​	2. Copy sqlnet.ora and tnsnames.ora to /usr/lib/oracle/18.5/client64/lib/network/admin directory

```
 $ ls /usr/lib/oracle/18.5/client64/lib/network/admin

 $ sudo cp sqlnet.ora /usr/lib/oracle/18.5/client64/lib/network/admin/sqlnet.ora

 $ sudo cp tnsnames.ora /usr/lib/oracle/18.5/client64/lib/network/admin/tnsnames.ora

 $ cd /usr/lib/oracle/18.5/client64/lib/network/admin
```

​	2. Edit the sqlnet.ora

```
$ sudo vi sqlnet.ora
```

	3. Set the WALLET_LOCATION parameter to point to the wallet directory containing the cwallet.sso file as shown by the example below![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab500\images\Vi sqlnet.png)

	4. Exit and save the file                               
 	5. View your tnsnames.ora and note your five services
 	6. Export the bin path
 	7. Test the Instant Client with SQLPlus, the username is admin, but enter your password and service name

```
$ more tnsnames.ora

$ export PATH=/usr/lib/oracle/18.5/client64/bin:$PATH

$ export LD_LIBRARY_PATH=/usr/lib/oracle/18.5/client64/lib

$ sqlplus admin/<password>@<service_tp>
```

### Move the On Premise Database to Oracle Cloud ###

There are a number of ways to move or migrate your existing on premise Oracle database to the Oracle Cloud. In this lab the instructor has already used Data Pump to export the on premise database to a .dmp file. You can use this procedure also for your own database.  You will then upload the .dmp file to the Object Storage, and then import the .dmp file to Autonomous Database from the Object Storage. 

## Step 4: Upload the Database Dump File

We have conveniently exported the Swingbench database into a Data Pump .dmp file. The file is called **soedump18C_1G.dmp**. You will upload this file to the Object Storage. **In some cases, your instructor may have uploaded it to the lab material bucket to save time.**

​	1. From your OCI console, select Object Storage. 

​	2. From your compartment, create a bucket to hold your Database dump file. 

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab500\images\Object storage.PNG)



<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab500\images\Object details.PNG" style="zoom: 50%;" />

## Step 5: Set Credential for ATP to access the Object Store

In order for ATP to access the Object Storage we need to create a credential and then set the credential in the ATP database. 

​	1. Connect to your ATP from SQL Developer

From the SQL Developer worksheet create a credential for ATP to access the object store. You will need to run the DBMS_CLOUD.CREATE_CREDENTIAL package below from your ATP session. Replace the names in RED with your own names.

​	2. Give the credential a name

​	3. Provide your OCI login username

​	4. Provide the OCI Auth Token password to access the Object Storage. For instructor-led class this password has already been created for you.

Note: For reference, an Auth Token can be created from your OCI user settings. The Auth Token creation will generate the password. 

 

```
BEGIN

 DBMS_CLOUD.CREATE_CREDENTIAL(

  credential_name => ‘STORAGE_CREDENTIAL’,

  username => '<your_oci_username.com>',

  password => ‘<auth token password>’

 );

END;

/
```

​	5. Run the script

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab500\images\Create credential object store.PNG" style="zoom:50%;" />

While we are in SQL Developer check to see if you have the SOE schema in the Other Users folder. You should not see it.  We will import this Swingbench database schema later. 

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab500\images\SQL Developer other schema.png" style="zoom:50%;" />





## Acknowledgements ##

- **Author** - Milton Wan, Database Product Management, PTS - April 2020

