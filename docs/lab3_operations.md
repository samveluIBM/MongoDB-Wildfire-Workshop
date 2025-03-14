# MongoDB basic operations lab

## Overview
 
The objective of this lab is to execute some basic MongoDB operations in a MongoDB deployment.  
We will be performing some of the following basic operations.

1. Backup a MongoDB database
2. Drop a database
3. Restore the database
4. Shutdown a MongoDB Enterprise
5. Startup a MongoDB Enterprise
6. Setup a MongoDB replication cluster 


### MongoDB basic operations steps
#### Backup a MongoDB database 
Make sure you are connected to the Linux guest from the previous step.

In this lab we are going to use the **mongodump** utility to backup our lab database "labtest". 

Enter the following command to save the backup of the labtest database inside /tmp  directory


``` bash
   mongodump -v --db labtest --out /tmp
```


???- example "The following is an example where the terminal will show that the database dump is taken [click to expand me]"

      ```
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]# mongodump -v --db labtest --out /tmp
      2025-03-13T17:05:10.012-0400    dumping up to 1 collections in parallel
      2025-03-13T17:05:10.020-0400    writing labtest.employee to /tmp/labtest/employee.bson
      2025-03-13T17:05:10.022-0400    done dumping labtest.employee (2 documents)
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#


      ```

Enter the following command to list the /tmp/labtest directory 


``` bash
   ls -alF /tmp/labtest/
```


???- example "The following is an example where the terminal will show that the database dump is taken inside /tmp/labtest directory [click to expand me]"

      ```
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]# ls -alF /tmp/labtest/
      total 16
      drwxr-xr-x. 2 root root 4096 Mar 13 17:05 ./
      drwxrwxrwt. 5 root root 4096 Mar 13 17:11 ../
      -rw-r--r--. 1 root root  128 Mar 13 17:05 employee.bson
      -rw-r--r--. 1 root root  175 Mar 13 17:05 employee.metadata.json
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#


      ```
#### Drop a MongoDB database 

In this lab we are going to use the **db.dropDatabase()** utility to drop our database "labtest". 

Enter the following command to connect to MongoDB

``` bash
   mongosh
```


???- example "The following is an example where the terminal will show that there are four databases in the deployment [Click to expand me]"

      ```
      Enterprise test> show dbs
      admin    40.00 KiB
      config   72.00 KiB
      labtest  72.00 KiB
      local    40.00 KiB
      Enterprise test>
 
      ```

Now connect to the "labtest" database,drop the database and then list the databases by entering the following series of commands

``` bash
   use labtest;
   db.dropDatabase();
   show dbs
```


## Summary
 
In this lab we have executed some basic MongoDB commands in a MongoDB deployment 
using  **mongosh**  shell environment. 

Now you can exit the mongosh shell environment by issuing exit command



``` bash
   exit
```


???- example "The following example shows the output [click to expand me]"

 
      ```
      Enterprise labtest>    exit
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#

      ```

