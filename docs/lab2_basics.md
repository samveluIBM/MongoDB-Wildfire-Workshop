# MongoDB basic usage lab

## Overview
 
The objective of this lab is to execute some basic commands in a MongoDB deployment.  
We will be using  **mongosh**  shell environment to execute the MongoDB commands.

1. Connect to MongoDB deployment using MongoDB Shell  **mongosh**
2. Execute basic MongoDB commands 


### MongoDB basic usage operations
#### Connect to MongoDB deployment using mongosh
Make sure you are connected to the Linux guest from the previous step.

The MongoDB Shell, mongosh, is a JavaScript and Node.js REPL environment for interacting with MongoDB deployments. We can use the MongoDB Shell to test queries and interact with the data in the MongoDB instance running on our localhost with default port 27017.


Enter the following command to connect to MongoDB

``` bash
   mongosh
```
You can see the MongoDB version and some informational / warning messages.

???- example "The following is an example where the terminal will show that you are connected to the MongoDB environment [click to expand me]"

      ```
      [root@p1243-zvm1 MongoDB-Wildfire-Workshop]# mongosh
      Current Mongosh Log ID: 67d31b0d4065a9f808a63a57
      Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.4.2
      Using MongoDB:          7.0.17
      Using Mongosh:          2.4.2

      For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/

      ------
      The server generated these startup warnings when booting
      2025-03-13T13:07:55.567-04:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
      2025-03-13T13:07:55.593-04:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
      2025-03-13T13:07:55.593-04:00: /sys/kernel/mm/transparent_hugepage/enabled is 'always'. We suggest setting it to 'never' in this binary version
      2025-03-13T13:07:55.593-04:00: vm.max_map_count is too low
      ------

      Enterprise test>

      ```

 
You may use the **cls** command to clear the shell environment messages
 

``` bash
   cls
```
#### Execute basic MongoDB commands
###### List databases
You can list the databases by issuing the **show dbs** command in mongosh shell.

``` bash
   show dbs
```

The output of the above command will show the available databases, and in our case there are three default databases in the MongoDB deployment


???- example "The following example shows that there are three databases [click to expand me]"

      ```
      Enterprise test>    show dbs
      admin   40.00 KiB
      config  60.00 KiB
      local   40.00 KiB
      Enterprise test>

      
      ```
###### Display the current database
You can list the current databases by issuing the **db** command in mongosh shell.

``` bash
   db
```

The output of the above command will show the current available database, you can see the current database as “test” in our case.


???- example "The following example shows the current database [click to expand me]"

      ```
      Enterprise test> db
      test
      Enterprise test>
      
      ```

###### Create a new database
In MongoDB creating a new database is done by issuing **use** command. If the database is not already there the database will be created automatically. Here we will create "labtest" database 

``` bash
   use labtest
```

The output of the above command shows the current database is “labtest” in our case. 

???- example "The following example shows the current databases [click to expand me]"

      ```
      Enterprise test> use labtest
      switched to db labtest
      Enterprise labtest>

      
      ```

###### Create documents  
Let us create documents into our "labtest" database. The dosuments are created in a collection. If a collection doesn’t exist in the database, it will be created for you. 

In our example we will first insert a single document into “labtest” database and “employee” collection using the  **db.collection.insertOne()** method.


``` bash
   db.employee.insertOne(
   {
    name: "John Doe",
    number: 1000  
   }
  )

```
When a document is created MongoDB generates the _id field and its value automatically. The generated ObjectId consists of a unique randomly generated hexadecimal value.

???- example "The following example shows the output of the insert operation  [click to expand me]"

      ```
      Enterprise labtest>    db.employee.insertOne(
      ...    {
      ...     name: "John Doe",
      ...     number: 1000
      ...    }
      ...   )
      {
      acknowledged: true,
      insertedId: ObjectId('67d329a34065a9f808a63a58')
      }
      Enterprise labtest>


      
      ```

Now let us insert two more documents into our "labtest" database and “employee” collection using the **db.collection.insertMany()** method.


``` bash
   db.employee.insertMany([
      {
        name: "Emily John",
        number: 1500  
      },
      {
        name: "Super Man",
       number: 2000  
       }
    ])


```


???- example "The following example shows the output of the insert operation  [click to expand me]"

      ```
      Enterprise labtest>    db.employee.insertMany([
      ...       {
      ...         name: "Emily John",
      ...         number: 1500
      ...       },
      ...       {
      ...         name: "Super Man",
      ...        number: 2000
      ...        }
      ...     ])
      {
      acknowledged: true,
      insertedIds: {
      '0': ObjectId('67d32b594065a9f808a63a59'),
      '1': ObjectId('67d32b594065a9f808a63a5a')
      }
      }
      Enterprise labtest>


      
      ```

###### Query documents  
Let us query the documents in our "labtest" database by using the  **db.collection.find()** method.


``` bash
   db.employee.find()
```


???- example "The following example shows there are three documents [click to expand me]"

      ```
      Enterprise labtest>    db.employee.find()
      [
      {
      _id: ObjectId('67d329a34065a9f808a63a58'),
      name: 'John Doe',
      number: 1000
      },
      {
      _id: ObjectId('67d32b594065a9f808a63a59'),
      name: 'Emily John',
      number: 1500
      },
      {
      _id: ObjectId('67d32b594065a9f808a63a5a'),
      name: 'Super Man',
      number: 2000
      }
      ]
      Enterprise labtest>

      
      ```


Let us query the documents in our "labtest" database for a specific search condition by using the  **db.collection.find()** method.

We will find the documents matching employee number 1000

``` bash
   db.employee.find( { "number": 1000 })
```


???- example "The following example shows there is one document with number 1000 [click to expand me]"

      ```
      Enterprise labtest>    db.employee.find()
      [
      {
      _id: ObjectId('67d329a34065a9f808a63a58'),
      name: 'John Doe',
      number: 1000
      }
      ]
      Enterprise labtest>

      
      ```


###### Update documents  
Let us update a document in our "labtest" database by using the **db.collection.updateOne()** method.
We will add a new field “currentDate” to a single document who has a “number” field value equal to 1000.


``` bash
   db.employee.updateOne(  { "number": 1000 }, 
    {
     $currentDate: { lastUpdated: true } 
    }
   )

```


???- example "The following example shows a single document is updated [click to expand me]"

      ```
      Enterprise labtest>    db.employee.updateOne(  { "number": 1000 },
      ...     {
      ...      $currentDate: { lastUpdated: true }
      ...     }
      ...    )
      {
      acknowledged: true,
      insertedId: null,
      matchedCount: 1,
      modifiedCount: 1,
      upsertedCount: 0
      }
      Enterprise labtest>

      
      ```

Let us query the documents in our "labtest" database by using the  **db.collection.find()** method.


``` bash
   db.employee.find()
```


???- example "The following example shows that there are three documents and only one document with number 1000 has one extra column **lastUpdated**  [click to expand me]"

      ```
      Enterprise labtest>    db.employee.find()
      [
      {
      _id: ObjectId('67d329a34065a9f808a63a58'),
      name: 'John Doe',
      number: 1000,
      lastUpdated: ISODate('2025-03-13T19:42:34.906Z')
      },
      {
      _id: ObjectId('67d32b594065a9f808a63a59'),
      name: 'Emily John',
      number: 1500
      },
      {
      _id: ObjectId('67d32b594065a9f808a63a5a'),
      name: 'Super Man',
      number: 2000
      }
      ]
      Enterprise labtest>
      ```

###### Delete documents  
The MongoDB shell provides the following methods to delete documents from a collection:

To delete a single document, use **db.collection.deleteOne()** 

To delete multiple documents, use **db.collection.deleteMany()**


Let us delete a single document in our "labtest" database by using the  **db.collection.deleteOne()** method which satisfies our criteria to match number 1500


``` bash
   db.employee.deleteOne({ "number": 1500 })
```


???- example "The following example shows the document is deleted [click to expand me]"

      ```
      Enterprise labtest>    db.employee.deleteOne({ "number": 1500 })
      { acknowledged: true, deletedCount: 1 }
      Enterprise labtest>


      
      ```

Let us query the documents in our "labtest" database by using the  **db.collection.find()** method.


``` bash
   db.employee.find()
```


???- example "The following example shows that there are two documents [click to expand me]"

      ```
      Enterprise labtest>    db.employee.find()
      [
      {
      _id: ObjectId('67d329a34065a9f808a63a58'),
      name: 'John Doe',
      number: 1000,
      lastUpdated: ISODate('2025-03-13T19:42:34.906Z')
      },
      {
      _id: ObjectId('67d32b594065a9f808a63a5a'),
      name: 'Super Man',
      number: 2000
      }
      ]
      Enterprise labtest>

      
      ```

###### Count the number of documents  
Let us query the number of documents in our "employee" collection of the "labtest" database by using the **db.collection.countDocuments()** method  


``` bash
   db.employee.countDocuments()
```


???- example "The following example shows that there are two documents [click to expand me]"

 
      ```
      Enterprise labtest>    db.employee.countDocuments()
      2
      Enterprise labtest>

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

