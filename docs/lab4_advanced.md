# MongoDB Advanced operations lab

## Overview
 
In this lab we will perform some advanced MongoDB operations in a MongoDB deployment.  

1. Setup a MongoDB replication cluster 


### MongoDB advanced operations 
#### Setup a MongoDB replication cluster 
Make sure you are connected to your Linux guest from the previous step.

In this part of the lab we are going to setup a three node MongoDB replication cluster in a single Linux Guest, **Just for lab** Do not try in production. In production environment we need to setup the MongoDB replication nodes at least in three different servers.

We will follow these steps

Start the mongod services specifying the name of the replica set. In our case **rs0**.  

Start another mongod instance with a different database directory and a port number

Start one more mongod instance with a different database directory and a port number

Start replication

Check the replication 

###### Setup the first replication node
We are going to setup a three node replication node and in the first node which will act as a primary node.

We will start the mongod services for the first node with the following parameters:

The name of the replica set as **rs0**

The data file path as **/var/lib/mongodb**

The log file path as **/var/log/mongodb/mongod.log**

And we will set the the mongod to be running in the background  
 
Enter the following commands  


``` bash
   mongod  --dbpath /var/lib/mongodb --logpath /var/log/mongodb/mongod.log --fork --bind_ip_all --replSet rs0
```

???- example "The following is an example where the terminal will show that the MongoDB is started and waiting for connections [Click to expand me]"

      ```
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#    mongod  --dbpath /var/lib/mongodb --logpath /var/log/mongodb/mongod.log --fork --bind_ip_all --replSet rs0
      about to fork child process, waiting until server is ready for connections.
      forked process: 113404
      child process started successfully, parent exiting
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#
  
      ```
     

###### Setup the second replication node
We are going to setup the second node in our replication cluster which will act as secondary node. As our nodes are running in the same Linux guest it should have its own data, log directory as well as a new port to listen.

We will start the mongod services for the second node with the following parameters:

The name of the replica set as **rs0**

The data file path as **/var/lib/mongodb1**

The log file path as **/var/log/mongodb1/mongod.log**

The port number as  **27018**  

And we will set the the mongod to be running in the background

 
Enter the following commands  


``` bash
   mkdir /var/lib/mongodb1/;
   mkdir /var/log/mongodb1/;
   mongod --port 27018  --dbpath /var/lib/mongodb1 --logpath /var/log/mongodb1/mongod.log --fork --bind_ip_all --replSet rs0
```


???- example "The following is an example where the terminal will show that the MongoDB is started and waiting for connections [Click to expand me]"
      ```
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#    mkdir /var/lib/mongodb1/;
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#    mkdir /var/log/mongodb1/;
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#    mongod --port 27018  --dbpath /var/lib/mongodb1 --logpath /var/log/mongodb1/mongod.log --fork --bind_ip_all --replSet rs0
      about to fork child process, waiting until server is ready for connections.
      forked process: 113514
      child process started successfully, parent exiting
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#
     
      ```
     

###### Setup the third replication node
We are going to setup the third node in our replication cluster which will act as another secondary node. As our nodes are running in the same Linux guest this node should also have its own data, log directory as well as a new port to listen.

We will start the mongod services for the second node with the following parameters:

The name of the replica set as **rs0**

The data file path as **/var/lib/mongodb2**

The log file path as **/var/log/mongodb2/mongod.log**

The port number as  **27019**  

And we will set the the mongod to be running in the background

 
Enter the following commands  

``` bash
   mkdir /var/lib/mongodb2/;
   mkdir /var/log/mongodb2/;
   mongod --port 27019  --dbpath /var/lib/mongodb2 --logpath /var/log/mongodb2/mongod.log --fork --bind_ip_all --replSet rs0
```

???- example "The following is an example where the terminal will show that the MongoDB is started and waiting for connections [Click to expand me]"

      ```
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#    mkdir /var/lib/mongodb2/;
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#    mkdir /var/log/mongodb2/;
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#    mongod --port 27019  --dbpath /var/lib/mongodb2 --logpath /var/log/mongodb2/mongod.log --fork --bind_ip_all --replSet rs0
      about to fork child process, waiting until server is ready for connections.
      forked process: 113604
      child process started successfully, parent exiting
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#

      
      ```

###### Initialize the replication nodes 

To initialize the replication cluster, open mongosh shell and run **rs.initiate()** in the primary node 

Add the MongoDB secondary nodes that we already started, by running **rs.add(<hostname:port>)** 

You can run **rs.status()** to check the replication node status


Enter the following commands  


``` 
   mongosh
   rs.initiate() 
   rs.status() 
   **In the following command change the hostname to your hostname**
   rs.add("HOSTNAME:27018")
   rs.status() 
   rs.add("HOSTNAME:27019")
   Enterprise test> rs.status() 

```

???- example "The following is an example where the terminal will show that the MongoDB instances are started and waiting for replication [Click to expand me]"

      ```
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]# mongosh
      Current Mongosh Log ID: 67d4aef9b5283a4da4a63a57
      Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.4.2
      Using MongoDB:          7.0.17
      Using Mongosh:          2.4.2

      For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/

      ------
         The server generated these startup warnings when booting
         2025-03-14T18:00:12.416-04:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
         2025-03-14T18:00:12.623-04:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
         2025-03-14T18:00:12.623-04:00: You are running this process as the root user, which is not recommended
         2025-03-14T18:00:12.623-04:00: /sys/kernel/mm/transparent_hugepage/enabled is 'always'. We suggest setting it to 'never' in this binary version
         2025-03-14T18:00:12.623-04:00: Soft rlimits for open file descriptors too low
      ------

      Enterprise test> cls

      Enterprise test> rs.initiate()
      {
      info2: 'no configuration specified. Using a default configuration for the set',
      me: 'p1243-zvm1:27017',
      ok: 1
      }
      Enterprise rs0 [direct: other] test> rs.status()
      {
      set: 'rs0',
      date: ISODate('2025-03-14T22:35:10.178Z'),
      myState: 1,
      term: Long('1'),
      syncSourceHost: '',
      syncSourceId: -1,
      heartbeatIntervalMillis: Long('2000'),
      majorityVoteCount: 1,
      writeMajorityCount: 1,
      votingMembersCount: 1,
      writableVotingMembersCount: 1,
      optimes: {
         lastCommittedOpTime: { ts: Timestamp({ t: 1741991701, i: 23 }), t: Long('1') },
         lastCommittedWallTime: ISODate('2025-03-14T22:35:01.597Z'),
         readConcernMajorityOpTime: { ts: Timestamp({ t: 1741991701, i: 23 }), t: Long('1') },
         appliedOpTime: { ts: Timestamp({ t: 1741991701, i: 23 }), t: Long('1') },
         durableOpTime: { ts: Timestamp({ t: 1741991701, i: 23 }), t: Long('1') },
         lastAppliedWallTime: ISODate('2025-03-14T22:35:01.597Z'),
         lastDurableWallTime: ISODate('2025-03-14T22:35:01.597Z')
      },
      lastStableRecoveryTimestamp: Timestamp({ t: 1741991701, i: 1 }),
      electionCandidateMetrics: {
         lastElectionReason: 'electionTimeout',
         lastElectionDate: ISODate('2025-03-14T22:35:01.518Z'),
         electionTerm: Long('1'),
         lastCommittedOpTimeAtElection: { ts: Timestamp({ t: 1741991701, i: 1 }), t: Long('-1') },
         lastSeenOpTimeAtElection: { ts: Timestamp({ t: 1741991701, i: 1 }), t: Long('-1') },
         numVotesNeeded: 1,
         priorityAtElection: 1,
         electionTimeoutMillis: Long('10000'),
         newTermStartDate: ISODate('2025-03-14T22:35:01.535Z'),
         wMajorityWriteAvailabilityDate: ISODate('2025-03-14T22:35:01.549Z')
      },
      members: [
         {
            _id: 0,
            name: 'p1243-zvm1:27017',
            health: 1,
            state: 1,
            stateStr: 'PRIMARY',
            uptime: 2098,
            optime: { ts: Timestamp({ t: 1741991701, i: 23 }), t: Long('1') },
            optimeDate: ISODate('2025-03-14T22:35:01.000Z'),
            lastAppliedWallTime: ISODate('2025-03-14T22:35:01.597Z'),
            lastDurableWallTime: ISODate('2025-03-14T22:35:01.597Z'),
            syncSourceHost: '',
            syncSourceId: -1,
            infoMessage: 'Could not find member to sync from',
            electionTime: Timestamp({ t: 1741991701, i: 2 }),
            electionDate: ISODate('2025-03-14T22:35:01.000Z'),
            configVersion: 1,
            configTerm: 1,
            self: true,
            lastHeartbeatMessage: ''
         }
      ],
      ok: 1,
      '$clusterTime': {
         clusterTime: Timestamp({ t: 1741991701, i: 23 }),
         signature: {
            hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
            keyId: Long('0')
         }
      },
      operationTime: Timestamp({ t: 1741991701, i: 23 })
      }
      Enterprise rs0 [direct: primary] test>

      -------------------------
      Enterprise rs0 [direct: primary] test> rs.add("p1243-zvm1:27018")
      {
      ok: 1,
      '$clusterTime': {
         clusterTime: Timestamp({ t: 1741991791, i: 1 }),
         signature: {
            hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
            keyId: Long('0')
         }
      },
      operationTime: Timestamp({ t: 1741991791, i: 1 })
      }
      Enterprise rs0 [direct: primary] test> rs.status()
      {
      set: 'rs0',
      date: ISODate('2025-03-14T22:36:35.567Z'),
      myState: 1,
      term: Long('1'),
      syncSourceHost: '',
      syncSourceId: -1,
      heartbeatIntervalMillis: Long('2000'),
      majorityVoteCount: 2,
      writeMajorityCount: 2,
      votingMembersCount: 2,
      writableVotingMembersCount: 2,
      optimes: {
         lastCommittedOpTime: { ts: Timestamp({ t: 1741991793, i: 1 }), t: Long('1') },
         lastCommittedWallTime: ISODate('2025-03-14T22:36:33.074Z'),
         readConcernMajorityOpTime: { ts: Timestamp({ t: 1741991793, i: 1 }), t: Long('1') },
         appliedOpTime: { ts: Timestamp({ t: 1741991793, i: 1 }), t: Long('1') },
         durableOpTime: { ts: Timestamp({ t: 1741991793, i: 1 }), t: Long('1') },
         lastAppliedWallTime: ISODate('2025-03-14T22:36:33.074Z'),
         lastDurableWallTime: ISODate('2025-03-14T22:36:33.074Z')
      },
      lastStableRecoveryTimestamp: Timestamp({ t: 1741991751, i: 1 }),
      electionCandidateMetrics: {
         lastElectionReason: 'electionTimeout',
         lastElectionDate: ISODate('2025-03-14T22:35:01.518Z'),
         electionTerm: Long('1'),
         lastCommittedOpTimeAtElection: { ts: Timestamp({ t: 1741991701, i: 1 }), t: Long('-1') },
         lastSeenOpTimeAtElection: { ts: Timestamp({ t: 1741991701, i: 1 }), t: Long('-1') },
         numVotesNeeded: 1,
         priorityAtElection: 1,
         electionTimeoutMillis: Long('10000'),
         newTermStartDate: ISODate('2025-03-14T22:35:01.535Z'),
         wMajorityWriteAvailabilityDate: ISODate('2025-03-14T22:35:01.549Z')
      },
      members: [
         {
            _id: 0,
            name: 'p1243-zvm1:27017',
            health: 1,
            state: 1,
            stateStr: 'PRIMARY',
            uptime: 2183,
            optime: { ts: Timestamp({ t: 1741991793, i: 1 }), t: Long('1') },
            optimeDate: ISODate('2025-03-14T22:36:33.000Z'),
            lastAppliedWallTime: ISODate('2025-03-14T22:36:33.074Z'),
            lastDurableWallTime: ISODate('2025-03-14T22:36:33.074Z'),
            syncSourceHost: '',
            syncSourceId: -1,
            infoMessage: 'Could not find member to sync from',
            electionTime: Timestamp({ t: 1741991701, i: 2 }),
            electionDate: ISODate('2025-03-14T22:35:01.000Z'),
            configVersion: 3,
            configTerm: 1,
            self: true,
            lastHeartbeatMessage: ''
         },
         {
            _id: 1,
            name: 'p1243-zvm1:27018',
            health: 1,
            state: 2,
            stateStr: 'SECONDARY',
            uptime: 4,
            optime: { ts: Timestamp({ t: 1741991793, i: 1 }), t: Long('1') },
            optimeDurable: { ts: Timestamp({ t: 1741991793, i: 1 }), t: Long('1') },
            optimeDate: ISODate('2025-03-14T22:36:33.000Z'),
            optimeDurableDate: ISODate('2025-03-14T22:36:33.000Z'),
            lastAppliedWallTime: ISODate('2025-03-14T22:36:33.074Z'),
            lastDurableWallTime: ISODate('2025-03-14T22:36:33.074Z'),
            lastHeartbeat: ISODate('2025-03-14T22:36:35.077Z'),
            lastHeartbeatRecv: ISODate('2025-03-14T22:36:33.577Z'),
            pingMs: Long('0'),
            lastHeartbeatMessage: '',
            syncSourceHost: 'p1243-zvm1:27017',
            syncSourceId: 0,
            infoMessage: '',
            configVersion: 3,
            configTerm: 1
         }
      ],
      ok: 1,
      '$clusterTime': {
         clusterTime: Timestamp({ t: 1741991793, i: 1 }),
         signature: {
            hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
            keyId: Long('0')
         }
      },
      operationTime: Timestamp({ t: 1741991793, i: 1 })
      }
      Enterprise rs0 [direct: primary] test>

      ---------------------------------
      Enterprise rs0 [direct: primary] test> rs.add("p1243-zvm1:27019")
      {
      ok: 1,
      '$clusterTime': {
         clusterTime: Timestamp({ t: 1741991876, i: 1 }),
         signature: {
            hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
            keyId: Long('0')
         }
      },
      operationTime: Timestamp({ t: 1741991876, i: 1 })
      }
      Enterprise rs0 [direct: primary] test> rs.status()
      {
      set: 'rs0',
      date: ISODate('2025-03-14T22:38:04.243Z'),
      myState: 1,
      term: Long('1'),
      syncSourceHost: '',
      syncSourceId: -1,
      heartbeatIntervalMillis: Long('2000'),
      majorityVoteCount: 2,
      writeMajorityCount: 2,
      votingMembersCount: 3,
      writableVotingMembersCount: 3,
      optimes: {
         lastCommittedOpTime: { ts: Timestamp({ t: 1741991878, i: 1 }), t: Long('1') },
         lastCommittedWallTime: ISODate('2025-03-14T22:37:58.648Z'),
         readConcernMajorityOpTime: { ts: Timestamp({ t: 1741991878, i: 1 }), t: Long('1') },
         appliedOpTime: { ts: Timestamp({ t: 1741991878, i: 1 }), t: Long('1') },
         durableOpTime: { ts: Timestamp({ t: 1741991878, i: 1 }), t: Long('1') },
         lastAppliedWallTime: ISODate('2025-03-14T22:37:58.648Z'),
         lastDurableWallTime: ISODate('2025-03-14T22:37:58.648Z')
      },
      lastStableRecoveryTimestamp: Timestamp({ t: 1741991878, i: 1 }),
      electionCandidateMetrics: {
         lastElectionReason: 'electionTimeout',
         lastElectionDate: ISODate('2025-03-14T22:35:01.518Z'),
         electionTerm: Long('1'),
         lastCommittedOpTimeAtElection: { ts: Timestamp({ t: 1741991701, i: 1 }), t: Long('-1') },
         lastSeenOpTimeAtElection: { ts: Timestamp({ t: 1741991701, i: 1 }), t: Long('-1') },
         numVotesNeeded: 1,
         priorityAtElection: 1,
         electionTimeoutMillis: Long('10000'),
         newTermStartDate: ISODate('2025-03-14T22:35:01.535Z'),
         wMajorityWriteAvailabilityDate: ISODate('2025-03-14T22:35:01.549Z')
      },
      members: [
         {
            _id: 0,
            name: 'p1243-zvm1:27017',
            health: 1,
            state: 1,
            stateStr: 'PRIMARY',
            uptime: 2272,
            optime: { ts: Timestamp({ t: 1741991878, i: 1 }), t: Long('1') },
            optimeDate: ISODate('2025-03-14T22:37:58.000Z'),
            lastAppliedWallTime: ISODate('2025-03-14T22:37:58.648Z'),
            lastDurableWallTime: ISODate('2025-03-14T22:37:58.648Z'),
            syncSourceHost: '',
            syncSourceId: -1,
            infoMessage: '',
            electionTime: Timestamp({ t: 1741991701, i: 2 }),
            electionDate: ISODate('2025-03-14T22:35:01.000Z'),
            configVersion: 5,
            configTerm: 1,
            self: true,
            lastHeartbeatMessage: ''
         },
         {
            _id: 1,
            name: 'p1243-zvm1:27018',
            health: 1,
            state: 2,
            stateStr: 'SECONDARY',
            uptime: 93,
            optime: { ts: Timestamp({ t: 1741991878, i: 1 }), t: Long('1') },
            optimeDurable: { ts: Timestamp({ t: 1741991878, i: 1 }), t: Long('1') },
            optimeDate: ISODate('2025-03-14T22:37:58.000Z'),
            optimeDurableDate: ISODate('2025-03-14T22:37:58.000Z'),
            lastAppliedWallTime: ISODate('2025-03-14T22:37:58.648Z'),
            lastDurableWallTime: ISODate('2025-03-14T22:37:58.648Z'),
            lastHeartbeat: ISODate('2025-03-14T22:38:02.651Z'),
            lastHeartbeatRecv: ISODate('2025-03-14T22:38:02.651Z'),
            pingMs: Long('0'),
            lastHeartbeatMessage: '',
            syncSourceHost: 'p1243-zvm1:27017',
            syncSourceId: 0,
            infoMessage: '',
            configVersion: 5,
            configTerm: 1
         },
         {
            _id: 2,
            name: 'p1243-zvm1:27019',
            health: 1,
            state: 2,
            stateStr: 'SECONDARY',
            uptime: 7,
            optime: { ts: Timestamp({ t: 1741991878, i: 1 }), t: Long('1') },
            optimeDurable: { ts: Timestamp({ t: 1741991878, i: 1 }), t: Long('1') },
            optimeDate: ISODate('2025-03-14T22:37:58.000Z'),
            optimeDurableDate: ISODate('2025-03-14T22:37:58.000Z'),
            lastAppliedWallTime: ISODate('2025-03-14T22:37:58.648Z'),
            lastDurableWallTime: ISODate('2025-03-14T22:37:58.648Z'),
            lastHeartbeat: ISODate('2025-03-14T22:38:02.653Z'),
            lastHeartbeatRecv: ISODate('2025-03-14T22:38:03.152Z'),
            pingMs: Long('2'),
            lastHeartbeatMessage: '',
            syncSourceHost: 'p1243-zvm1:27018',
            syncSourceId: 1,
            infoMessage: '',
            configVersion: 5,
            configTerm: 1
         }
      ],
      ok: 1,
      '$clusterTime': {
         clusterTime: Timestamp({ t: 1741991878, i: 1 }),
         signature: {
            hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
            keyId: Long('0')
         }
      },
      operationTime: Timestamp({ t: 1741991878, i: 1 })
      }
      Enterprise rs0 [direct: primary] test>


      
      ```     

###### Check replication  

Now let us check if the replication is happening correctly.

In the PRIMARY instance, insert a document to our MongoDB Database labtest 

Enter the following commands  


``` 
   
   show dbs
   use labtest 
      db.employee.insertOne(
   {
    name: "Iron Man",
    number: 5000  
      }
     )
   db.employee.find()

```

???- example "The following is an example output of the previous commands [Click to expand me]"

      ```
      Enterprise rs0 [direct: primary] test> show dbs
      admin     80.00 KiB
      config   200.00 KiB
      labtest   40.00 KiB
      local    732.00 KiB
      Enterprise rs0 [direct: primary] test>    
      Enterprise rs0 [direct: primary] test> use labtest
      switched to db labtest
      Enterprise rs0 [direct: primary] labtest>    db.employee.find()
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
      Enterprise rs0 [direct: primary] labtest> db.employee.insertOne(
      ...    {
      ...     name: "Iron Man",
      ...     number: 5000
      ...    }
      ...   )
      {
      acknowledged: true,
      insertedId: ObjectId('67d5e07dbc29fd3a01a63a58')
      }
      Enterprise rs0 [direct: primary] labtest> db.employee.find()
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
      },
      {
         _id: ObjectId('67d5e07dbc29fd3a01a63a58'),
         name: 'Iron Man',
         number: 5000
      }
      ]
      Enterprise rs0 [direct: primary] labtest>


      ```

Now it is time to check in the SECONDARY instances to see if the replication happened.

To connect to MongoDB instances running other than the default port we have to specify the port number. 
Once connected check if the labtest database has the same set of documents as in the primary instance. Use the following set of commands after exiting the primary instance mongo shell.


``` bash
   exit
   mongosh --port 27018
   show dbs
   use labtest
   db.employee.find()
```
As seen in the following output the number of databases and the labtest database records are same as in the primary instance

???- example "The following is an example output of the previous commands [Click to expand me]"

      ```
      Enterprise rs0 [direct: primary] labtest> exit
      [root@p1243-zvm1 ~]# mongosh --port 27018
      Current Mongosh Log ID: 67d5e4fc11240b7aa8a63a57
      Connecting to:          mongodb://127.0.0.1:27018/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.4.2
      Using MongoDB:          7.0.17
      Using Mongosh:          2.4.2

      For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/

      ------
         The server generated these startup warnings when booting
         2025-03-14T18:14:08.067-04:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
         2025-03-14T18:14:08.122-04:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
         2025-03-14T18:14:08.122-04:00: You are running this process as the root user, which is not recommended
         2025-03-14T18:14:08.123-04:00: /sys/kernel/mm/transparent_hugepage/enabled is 'always'. We suggest setting it to 'never' in this binary version
         2025-03-14T18:14:08.123-04:00: Soft rlimits for open file descriptors too low
      ------

      Enterprise rs0 [direct: secondary] test> cls

      Enterprise rs0 [direct: secondary] test> show dbs
      admin     80.00 KiB
      config   308.00 KiB
      labtest   72.00 KiB
      local    700.00 KiB
      Enterprise rs0 [direct: secondary] test> use labtest
      switched to db labtest
      Enterprise rs0 [direct: secondary] labtest> db.employee.find()
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
      },
      {
         _id: ObjectId('67d5e07dbc29fd3a01a63a58'),
         name: 'Iron Man',
         number: 5000
      }
      ]
      Enterprise rs0 [direct: secondary]

      ```

 
Now let us check the third instance running at port 27019 if the labtest database has the same set of documents as in the primary instance. Use the following set of commands after exiting the connected mongo shell.


``` bash
   exit
   mongosh --port 27019
   show dbs
   use labtest
   db.employee.find()
```
As seen in the following output the number of databases and the labtest database records are same as in the primary instance

???- example "The following is an example output of the previous commands [Click to expand me]"


      ```
      Enterprise rs0 [direct: secondary] labtest> exit
      [root@p1243-zvm1 ~]# mongosh --port 27019
      Current Mongosh Log ID: 67d5e54bc60fc8c961a63a57
      Connecting to:          mongodb://127.0.0.1:27019/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.4.2
      Using MongoDB:          7.0.17
      Using Mongosh:          2.4.2

      For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/

      ------
         The server generated these startup warnings when booting
         2025-03-14T18:26:33.204-04:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
         2025-03-14T18:26:33.227-04:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
         2025-03-14T18:26:33.227-04:00: You are running this process as the root user, which is not recommended
         2025-03-14T18:26:33.227-04:00: /sys/kernel/mm/transparent_hugepage/enabled is 'always'. We suggest setting it to 'never' in this binary version
         2025-03-14T18:26:33.227-04:00: Soft rlimits for open file descriptors too low
      ------

      Enterprise rs0 [direct: secondary] test> cls

      Enterprise rs0 [direct: secondary] test> use labtest
      switched to db labtest
      Enterprise rs0 [direct: secondary] labtest> db.employee.find()
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
      },
      {
         _id: ObjectId('67d5e07dbc29fd3a01a63a58'),
         name: 'Iron Man',
         number: 5000
      }
      ]
      Enterprise rs0 [direct: secondary] labtest>
      ```



## Summary
 
In this lab we have setup a replication cluster and did some basic replication activities. 

Now you can exit the mongosh shell environment by issuing exit command



``` bash
   exit
```


???- example "The following example shows the output [click to expand me]"

 
      ```
      Enterprise labtest>    exit
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]#

      ```

