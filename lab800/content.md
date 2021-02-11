# Run the Application Workload 

![](./images/run-swingbench-diagram.PNG)

The Swingbench workload on the App Server runs against ATP through the Service Gateway. Throughout the run, CPU is saturated at 100% utilization, so it’s a good test to scale the cores. First let’s open SQL Developer tool to check the core count. 

## Requirements ##

- Web Browser
- SSH public and private keys
- PuTTY or equivalent
- SQL Developer 19.1 or higher

## Step 1: Run Swingbench Workload ##

​	1. Open a SQL Developer connection to your ATP

​	2. From SQL Developer worksheet, check your cpu count by typing in the worksheet

```
show parameter cpu
```

​	3. Reconnect to ATP if SQL Developer has lost the connection.	

​	4. Click the run button

<img src="./images/sql-developer-show-cpu-count.PNG" style="zoom:80%;" />

Note we have 2 OCPUs (cores) provisioned but we have cpu_count of 4 because you have 4 Intel Hyper-threads.

​	5. Switch back to your SSH terminal session to the app server.

​	6. Run the script below.  

There are 128 concurrent users making inserts and updates. We will measure users, transactions per minute (TPM) and transactions per second (TPS). It will run for 1 minute, 30 seconds, ie: -rt 1.30. 

​	7. Replace with your own wallet and service names. Ensure the directory location of your wallet zip and service name is correct.  ie: replace Wallet_ATPLABTEST with your own wallet file name, and atplabtest_tp service with your own service name.

Note that the schema is **soe**.  ie: -u soe.  The soe schema password is **Welcome#2018**. If you want to change the password or the user is locked, log in to sqlplus or SQL Developer and enter:

```
ALTER USER soe IDENTIFIED BY <new password> ACCOUNT UNLOCK;
```

<u>Troubleshooting</u>: If you are having trouble running the script, put it all in one line without the \ .

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

Note your results for TPM and TPS.  You should get about 1000 transactions per second (tps).  We will compare them later.  Hitting CTRL-C will stop the workload.

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

**Note:** Please do not enter a CPU core count beyond **3** as the cloud account is shared by other students and you will run into resource limitations.

<img src="./images/atp-details-page.PNG" style="zoom: 67%;" />

The ATP service will take a few seconds to scale. Notice the status of SCALING IN PROGRESS and the database is still up and processing user transactions during scaling.   There is no downtime.

​	10. From SQL Developer worksheet, check your cpu count

```
Show parameter cpu
```

<img src="./images/sql-developer-show-cpu-count-6.PNG" style="zoom: 67%;" />



Note your runtime TPM and TPS. You should see a lot more transactions!

You have scaled your cores up dynamically without stopping the service.

Sample run with 3 cores show below.

<img src="./images/sample-run-with-3-cores.PNG" style="zoom:67%;" />



## Step 2: Performance Monitoring

	1. Go to the ATP Service Console and examine your Performance Monitor data.
	2. Select Activity from the menu

From the Service Console Activity you can see both runs were at 100% CPU utilization. Which means we can still add cores to increase performance. Note the rapid dip in cpu utilization as it transitions to more cpu’s. It’s near instantaneous and the server is still running.

![](./images/performance-monitoring.PNG)

You can hit crtl-C to stop the workload run.

### Auto Scaling

Let’s look at the auto scaling feature. Auto scaling automatically scales your cores to a maximum of 3 times the initially core count when the CPU utilization is high.

​	1. Scale your ATP back down to 1 core and click update. No auto scale yet.

<img src="./images/atp-scaling-ui.png" style="zoom: 50%;" />

​	2. From SQL Developer worksheet, check your cpu count. With one core you should see 2 cpu threads.  Notice the scaling is very fast even before the console UI updates its status.

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

 

 4. After about 3 minutes, enable auto scale while the workload is still running.

    <img src="./images/auto-scale-ui.PNG" style="zoom: 67%;" />

After the scaling is completed you should see Auto Scaling enabled. Note your transactions continue to run as scaling begins.  There is no down time.

<img src="./images/atp-details-auto-scale.PNG" style="zoom: 67%;" />

​	5. From SQL Developer worksheet, check your cpu count

```
Show parameter cpu
```

Note that ATP has automatically scaled to 3 cores, the 3X maximum that it will scale to. Note the 3X increase in transactions per second and minutes. Check your performance monitor from the service console again.  We are still at 100% CPU utilization so this workload needs more CPUs :)  But you have just examined one of the autonomous features of ATP - auto scaling.

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

​	2. Set up another connection from SQL Developer to your ATP with a database service of **Medium**. This will be good for running our database queries.

  ![](./images/sql-developer-medium-connection.PNG)                             

​	3. From SQL Developer worksheet, check your cpu count is 4 (ie: 2 cores thus 4 Hyper-threads)

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

​	5. Now run both the Swingbench workload and the query together.  Remember to use your wallet names and service names.

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

​	6. Wait until the workload goes into a steady state with full transactions, about 3 minutes, then from SQL Developer run the query

```
select /*+NO_RESULT_CACHE*/ c_city, c_region, count(*)
from ssb.customer c_high
group by c_city, c_region
order by count(*);
```

Note your completion times. They should be longer like this: 1.259, 1.007, 1.25, 1.115, 1.008 seconds

​	7. Try connecting with the TPURGENT service. See the results.

What do you think of the results? 

By setting ATP database service, you can control the resources and the response times for different connections.  Analyze your performance activity from the ATP console.



END OF LAB 

## Acknowledgements ##

- **Author** - Milton Wan, Database Product Management, April 2020

