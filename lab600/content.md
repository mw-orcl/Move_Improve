# Run the Application Workload and Scale the ATP

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab600\images\Run Swingbench diagram.PNG)

The Swingbench workload on the App Server runs against ATP through the Service Gateway. Throughout the run, CPU is saturated at 100% utilization, so it’s a good test to scale the cores. First let’s open SQL Developer tool to check the core count. You should see 2 count for 1 core, and 4 count for 2 cores. We take into account Intel Hyper-threads when counting.

## Disclaimer ##

The following is intended to outline our general product direction. It is intended for information purposes only, and may not be incorporated into any contract. It is not a commitment to deliver any material, code, or functionality, and should not be relied upon in making purchasing decisions. The development, release, and timing of any features or functionality described for Oracle’s products remains at the sole discretion of Oracle.

## Requirements ##

- Web Browser
- SSH public and private keys
- PuTTY or equivalent
- SQL Developer 19.1 or higher

## Step 1: Run Swingbench Workload ##

​	1. Open a SQL Developer connection to your ATP

​	2. From SQL Developer worksheet, check your core count by typing in the worksheet

```
show parameter cpu
```

​	3. Select and highlight the script

​	4. Click the run button

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab600\images\SQL Developer show cpu count.PNG" style="zoom:80%;" />

Note we have 2 OCPUs (cores) provisioned but we have cpu_count of 4 because you have 4 Intel Hyper-threads.

​	5. Switch back to your SSH terminal session to the app server.

​	6. Run the script below.  

There are 128 concurrent users making inserts and updates. We will measure transactions per minute (TPM) and transactions per second (TPS). It will run for 1 minute, 30 seconds, ie: -rt 1.30. 

​	7. Replace with your own wallet and service names. Ensure the directory location of your wallet zip and service name is correct. 

If you received the dump file from an instructor-led class note that the soe schema password is **Welcome#2018**. Do not change this.

```
$ cd ~/swingbench/bin

$./charbench -c ../configs/SOE_Server_Side_V2.xml \

-cf ~/Wallet_ATPLABTEST/Wallet_ATPLABTEST.zip \

-cs atplabtest_tp \

-u soe \

-p Welcome#2018 \

-v users,tpm,tps \

-intermin 0 \

-intermax 0 \

-min 0 \

-max 0 \

-uc 128 \

-di SQ,WQ,WA \

-rt 0:1.30
```

Note your runtime TPM and TPS. We will compare them later.  Hitting CTRL-C will stop the workload.

Sample run with 2 cores show below.

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab600\images\Swingbench run 1.PNG" style="zoom:67%;" />



​	8. Now run the workload by changing –rt option to 30 minutes. Ie: -rt 0:30:00 

```
$ ./charbench -c ../configs/SOE_Server_Side_V2.xml \

-cf ~/Wallet_ATPLABTEST/Wallet_ATPLABTEST.zip \

-cs atplabtest_tp \

-u soe \

-p Welcome#2018 \

-v users,tpm,tps \

-intermin 0 \

-intermax 0 \

-min 0 \

-max 0 \

-uc 128 \

-di SQ,WQ,WA \

-rt 0:30.00
```

​	9. After about 3 minutes scale ATP up to 3 cores. Do not enable Auto Scaling yet. We will scale manually first. 

**Note:** Please do not enter a CPU core count beyond **3** as the cloud account is shared by all students and you will run into resource limitations.

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab600\images\ATP details page.PNG" style="zoom: 67%;" />

The ATP service will take a few seconds to scale. Notice the status of SCALING IN PROGRESS.

​	10. From SQL Developer worksheet, check your core count

```
Show parameter cpu
```

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab600\images\SQL Developer show cpu count 6.PNG" style="zoom: 67%;" />



Note your runtime TPM and TPS. You should see a lot more transactions!

You have scaled your cores up dynamically without stopping the service, so no down time!

Sample run with 3 cores show below.

<img src="C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab600\images\Sample run with 3 cores.PNG" style="zoom:67%;" />

## Step 2: Performance Monitoring

​	1. Go to the ATP service console and examine your Performance Monitor data.

From the Service Console Activity you can see both runs were at 100% CPU utilization. Which means we can still add cores to increase performance. Note the rapid dip in cpu utilization as it transitions to more cpu’s. It’s near instantaneous and the server is still running.

![](C:\Users\mwan.ORADEV\Documents\GitHub\Move_Improve\lab600\images\Performance monitoring.PNG)

You can hit crtl-C to stop the workload run.

## Acknowledgements ##

- **Author** - Milton Wan, Database Product Management, PTS - April 2020

