# MongoDB basic operations lab

## Overview
 
The objective of this lab is to execute some basic MongoDB operations in a MongoDB deployment.  
We will be performing some of the following basic operations.

1. Backup a MongoDB database
2. Drop a database
3. Restore the database
4. Shutdown a MongoDB Enterprise
5. Startup a MongoDB Enterprise


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
First let us connect to **mongosh** and list the databases in the environment by using **show dbs** command

Enter the following command to connect to MongoDB and list the databases

``` bash
   mongosh
   show dbs

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

Now connect to the "labtest" database, drop that database by **db.dropDatabase()** utility and then list the databases by entering the following series of commands

``` bash
   use labtest;
   db.dropDatabase();
   show dbs
```

???- example "The following is an example where the terminal will show that there are four databases in the deployment [Click to expand me]"

      ```
      Enterprise test> use labtest
      switched to db labtest
      Enterprise labtest> db.dropDatabase();
      { ok: 1, dropped: 'labtest' }
      Enterprise labtest> show dbs
      admin    40.00 KiB
      config  108.00 KiB
      local    40.00 KiB
      Enterprise labtest>

 
      ```

Now let us exit the mongosh shell environment by issuing exit command

   ``` bash
         exit
   ```

#### Restore a MongoDB database 

In this part of the lab we are going to use the **mongorestore** utility to restore our deleted lab database "labtest". 

Enter the following command to restore the backup of the labtest database available inside /tmp  directory


``` bash
   mongorestore -v --db labtest  --drop  /tmp/labtest
```


???- example "The following is an example where the terminal will show that the database dump is restored [click to expand me]"

      ```
         [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#    mongorestore -v --db labtest  --drop  /tmp/labtest
         2025-03-14T14:27:41.092-0400    using write concern: &{majority <nil> 0s}
         2025-03-14T14:27:41.096-0400    The --db and --collection flags are deprecated for this use-case; please use --nsInclude instead, i.e. with --nsInclude=${DATABASE}.${COLLECTION}
         2025-03-14T14:27:41.096-0400    building a list of collections to restore from /tmp/labtest dir
         2025-03-14T14:27:41.096-0400    found collection labtest.employee bson to restore to labtest.employee
         2025-03-14T14:27:41.096-0400    found collection metadata from labtest.employee to restore to labtest.employee
         2025-03-14T14:27:41.096-0400    reading metadata for labtest.employee from /tmp/labtest/employee.metadata.json
         2025-03-14T14:27:41.096-0400    creating collection labtest.employee with no metadata
         2025-03-14T14:27:41.102-0400    restoring labtest.employee from /tmp/labtest/employee.bson
         2025-03-14T14:27:41.113-0400    finished restoring labtest.employee (2 documents, 0 failures)
         2025-03-14T14:27:41.113-0400    no indexes to restore for collection labtest.employee
         2025-03-14T14:27:41.113-0400    2 document(s) restored successfully. 0 document(s) failed to restore.
         [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#



      ```
Let us connect back to **mongosh** and list the databases in the environment by using **show dbs** command


Enter the following command to connect to MongoDB and list the databases and then connect to labtest database and find the documents

``` bash
   mongosh
   show dbs
   use labtest
   db.employee.find()
```
???- example "The following is an example where the terminal will show that the database dump is restored [click to expand me]"

      ```

      Enterprise test> show dbs
      admin    40.00 KiB
      config   96.00 KiB
      labtest  40.00 KiB
      local    40.00 KiB
      Enterprise test> use labtest
      switched to db labtest
      Enterprise labtest> db.employee.find()
      [
      {
         _id: ObjectId('67d46f56d027864d37a63a58'),
         name: 'John Doe',
         number: 1000,
         lastUpdated: ISODate('2025-03-14T18:03:35.947Z')
      },
      {
         _id: ObjectId('67d46f61d027864d37a63a5a'),
         name: 'Super Man',
         number: 2000
      }
      ]
      Enterprise labtest>

      ```


#### Shutdown MongoDB Enterprise 

In this part of the lab we are going to shutdown MongoDB by using  **db.shutdownServer()**. You must issue the db.shutdownServer() operation against the admin database. 

Enter the following commands  


``` bash
   use admin
   db.shutdownServer()
   exit
```

???- example "The following is an example where the terminal will show that there are four databases in the deployment [Click to expand me]"

      ```
      Enterprise labtest> use admin
      switched to db admin
      Enterprise admin> db.shutdownServer()
      MongoNetworkError: connection 4 to 127.0.0.1:27017 closed
      Enterprise admin> exit
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#

 
      ```

#### Start a MongoDB Enterprise 

In this part of the lab we are going to start MongoDB Enterprise. 
We need to specify the following parameters: Data, log file and specify the fork option to start the mongod in the background. Then we will connect to mongosh shell.

Enter the following commands  


``` bash
   mongod  --dbpath /var/lib/mongodb --logpath /var/log/mongodb/mongod.log --fork --bind_ip_all
   mongosh
```

???- example "The following is an example where the terminal will show that the MongoDB is started and waiting for connections [Click to expand me]"

      ```
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#    mongod  --dbpath /var/lib/mongodb --logpath /var/log/mongodb/mongod.log --fork --bind_ip_all
      about to fork child process, waiting until server is ready for connections.
      forked process: 112970
      child process started successfully, parent exiting
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#mongosh
      Current Mongosh Log ID: 67d49ba95b1ee08487a63a57
      Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.4.2
      Using MongoDB:          7.0.17
      Using Mongosh:          2.4.2

      For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/

      ------
         The server generated these startup warnings when booting
         2025-03-14T16:56:05.980-04:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
         2025-03-14T16:56:06.187-04:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
         2025-03-14T16:56:06.187-04:00: You are running this process as the root user, which is not recommended
         2025-03-14T16:56:06.187-04:00: /sys/kernel/mm/transparent_hugepage/enabled is 'always'. We suggest setting it to 'never' in this binary version
         2025-03-14T16:56:06.187-04:00: Soft rlimits for open file descriptors too low
      ------

      Enterprise test>

      
   ```
     
In this part of the lab we will shutdown MongoDB by using  **db.shutdownServer()**. You must issue the db.shutdownServer() operation against the admin database. 

Enter the following commands  

   ``` bash
         use admin
         db.shutdownServer()
         exit
   ```

???- example "The following is an example of shutdown [Click to expand me]"

      ```
      Enterprise test> use admin
      switched to db admin
      Enterprise admin> db.shutdownServer()
      MongoNetworkError: connection 4 to 127.0.0.1:27017 closed
      Enterprise admin> exit
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#

   

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

