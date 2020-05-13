# Run the Application Workload 

![](./images/run-swingbench-diagram.PNG)

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

<img src="./images/sql-developer-show-cpu-count.PNG" style="zoom:80%;" />

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

<img src="./images/swingbench-run-1.PNG" style="zoom:67%;" />



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

<img src="./images/atp-details-page.PNG" style="zoom: 67%;" />

The ATP service will take a few seconds to scale. Notice the status of SCALING IN PROGRESS.

​	10. From SQL Developer worksheet, check your core count

```
Show parameter cpu
```

<img src="./images/sql-developer-show-cpu-count-6.PNG" style="zoom: 67%;" />



Note your runtime TPM and TPS. You should see a lot more transactions!

You have scaled your cores up dynamically without stopping the service, so no down time!

Sample run with 3 cores show below.

<img src="./images/sample-run-with-3-cores.PNG" style="zoom:67%;" />

## Step 2: Performance Monitoring

​	1. Go to the ATP Service Console and examine your Performance Monitor data.

From the Service Console Activity you can see both runs were at 100% CPU utilization. Which means we can still add cores to increase performance. Note the rapid dip in cpu utilization as it transitions to more cpu’s. It’s near instantaneous and the server is still running.

![](./images/performance-monitoring.PNG)

You can hit crtl-C to stop the workload run.

### Auto Scaling

Let’s look at the auto scaling feature. Auto scaling automatically scales your cores to a maximum of 3 times the initially core count when the CPU utilization is high.

​	1. Scale your ATP back down to 1 core and click update. No auto scale yet.

<img src="./images/atp-scaling-ui.png" style="zoom: 50%;" />

​	2. From SQL Developer worksheet, check your core count. With one core you should see 2 cpu threads.

```
Show parameter cpu
```

​	3. Run the charbench again for 30 minutes if the workload has stopped.  You may see some java connection warnings. You can ignore this.

```
./charbench -c ../configs/SOE_Server_Side_V2.xml \

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

 

 4. After about 3 minutes, enable auto scale.

    <img src="./images/auto-scale-ui.PNG" style="zoom: 67%;" />

After the scaling is completed you should see Auto Scaling enabled. Note your transactions continue to run as scaling begins.  There is no down time.

<img src="./images/atp-details-auto-scale.PNG" style="zoom: 67%;" />

​	5. From SQL Developer worksheet, check your core count

```
Show parameter cpu
```

Note that ATP has automatically scaled to 3 cores, the 3X maximum that it will scale to. Note the 3X increase in transactions per second and minutes. Check your performance monitor from the service console again.  We are still at 100% CPU utilized so this workload needs more CPUs!

<img src="./images/sample-run-with-auto-scale.png" style="zoom:67%;" />



​	6. Stop the Swingbench run with ctrl-C

In a few minutes ATP will scale back down automatically.  

​	7. From SQL Developer worksheet, check your core count.

```
Show parameter cpu
```

Why is the cpu count still the same even with the scale down after the workload stops?

### Show the Effects of Database Services

We are going to set up two connections, one running the Swingbench workload and the other running a query on another schema.

​	1. Scale the ATP back to 2 cores and disable the auto scale.

​	2. Set up another connection from SQL Developer to your ATP with a database service of Medium. This will be good for running our database queries.

  ![](./images/sql-developer-medium-connection.PNG)                             

​	3. From SQL Developer worksheet, check your core count is 4 (ie: 2 cores thus 4 Hyper-threads)

```
Show parameter cpu
```

​	4. From SQL Developer worksheet, run the following query 5 times and note the completion time with your MEDIUM service.

```
select /*+NO_RESULT_CACHE*/ c_city, c_region, count(*)

from ssb.customer c_high

group by c_city, c_region

order by count(*);
```

 <img src="./images/sql-developer-run-query.png" style="zoom: 50%;" />

You should have gotten similar completion times: 0.524, 0.406, 0.332, 0.357, 0.376 seconds

​	5. Now run both the Swingbench workload and the query together.

```
./charbench -c ../configs/SOE_Server_Side_V2.xml \

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

​	6. Wait until the workload goes full strength, about 3 minutes, then from SQL Developer run the query

```
select /*+NO_RESULT_CACHE*/ c_city, c_region, count(*)

from ssb.customer c_high

group by c_city, c_region

order by count(*);
```

Note your completion times. They should be longer like this: 1.259, 1.007, 0.625, 0.665, 0.578 seconds

​	7. Try connecting with the TPURGENT service. See the results.

What do you think of the results? Why is the MEDIUM results better?

By setting ATP database service, you can control the resources and the response times. Analyze your performance activity from the ATP console.



END OF LAB 

## Acknowledgements ##

- **Author** - Milton Wan, Database Product Management, April 2020

