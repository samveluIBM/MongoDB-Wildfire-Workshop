# MongoDB installation

## Overview
 
We will be using the Ansible playbooks to install MongoDB in the lab environment by following the steps below.

1. Clone the MongoDB git repository
2. Run the setup script 
3. Run the ansible playbook 


### MongoDB installation steps
#### Clone the mongoDB git repository
Make sure you are connected to the Linux guest from the previous step.

???- example "The following is an example where the terminal will show that you are connected to the Linux guest [click to expand me]"

      ```
      
      login as: cecuser
      Pre-authentication banner message from server:
      |
      | ______________________________________________________________________
      |
      |                     Welcome to IBM Technology Zone
      | ______________________________________________________________________
      |
      |
      | IBM's internal systems must only be used for conducting IBM's business
      | or for purposes authorized by IBM management.  Use is subject to audit
      | at any time by IBM management.
      |
      | Unauthorized access will be investigated and penalties will be pursued
      | in conformance with applicable laws and regulations. If you are not an
      | authorized user disconnect now.
      |
      |
      End of banner message from server
      cecuser@129.40.60.161's password:

      ______________________________________________________________________

                        Welcome to IBM Technology Zone
      ______________________________________________________________________


      IBM's internal systems must only be used for conducting IBM's business
      or for purposes authorized by IBM management.  Use is subject to audit
      at any time by IBM management.

      Unauthorized access will be investigated and penalties will be pursued
      in conformance with applicable laws and regulations. If you are not an
      authorized user disconnect now.
      ______________________________________________________________________

      IaaS Red Hat Enterprise Linux 8.10

      Activate the web console with: systemctl enable --now cockpit.socket

      Register this system with Red Hat Insights: insights-client --register
      Create an account or view all your systems at https://red.ht/insights-dashboard
      Last login: Thu Mar 13 10:18:51 2025 from 9.19.184.79
      [cecuser@p1243-zvm1 ~]$

      ```

Enter the following Linux commands to switch to **super user** for installation and then change the directory to a temporary working directory **/tmp** .

``` bash
   sudo -i 
   cd /tmp
```

???- example "The following is an example where the terminal will show that you are connected to the Linux terminal as “root” and the working directory is tmp [click to expand me]"

      ```
      [cecuser@p1243-zvm1 /]$    sudo -i
      [root@p1243-zvm1 ~]#    cd /tmp
      [root@p1243-zvm1 tmp]#

      ```

We have created an Ansible playbook repository to install MongoDB on a Red Hat Linux guest and clone that repository using the following command.

``` bash
   git clone https://github.com/samveluIBM/MongoDB-Wildfire-Workshop
```

???- example "The following example shows the output of succesful cloning of the repository [click to expand me]"

      ```
      [root@p1243-zvm1 tmp]#    git clone https://github.com/samveluIBM/MongoDB-Wildfire-Workshop
      Cloning into 'MongoDB-Wildfire-Workshop'...
      remote: Enumerating objects: 20, done.
      remote: Counting objects: 100% (20/20), done.
      remote: Compressing objects: 100% (14/14), done.
      remote: Total 20 (delta 1), reused 3 (delta 0), pack-reused 0 (from 0)
      Receiving objects: 100% (20/20), 4.99 KiB | 4.99 MiB/s, done.
      Resolving deltas: 100% (1/1), done.
      [root@p1243-zvm1 tmp]#

      ```
#### Run the setup script
This step will install the python and ansibles to use ansible playbooks to install MongoDB on the Linux guest. 
Change to the cloned directory and then run the **setup** script.
Use the following commands:

``` bash
   cd MongoDB-Wildfire-Workshop/
```

``` bash
    ./setup.sh
```
???- example "The following example shows the output of succesful execution of setup.sh script [click to expand me]"

      ```
      
      Installed products updated.
      ```
#### Run the ansible playbook
In this step we will run the ansible playbook to install MongoDB. Use the following command:

``` bash
   ansible-playbook mongodbInstall.yml 
```

???- example "The following example shows the output of succesful MongoDB installtion [click to expand me]"

      ```
      [root@zdblab05 zmongodb]# ansible-playbook mongodbInstall.yml

      PLAY [Install MongoDB on a Linux Guest.] ***********************************************************

      TASK [Gathering Facts] *****************************************************************************
      ok: [127.0.0.1]

      TASK [Create repo file for mongodb-enterprise] *****************************************************
      ok: [127.0.0.1]

      TASK [Install  mongodb-enterprise.] ****************************************************************
      changed: [127.0.0.1] => (item={'pkg': 'mongodb-enterprise', 'when': True})
      ok: [127.0.0.1] => (item={'pkg': 'libselinux-utils', 'when': "db_path != '/var/lib/mongodb' or log_file != '/var/log/mongodb/mongod.log'"})
      ok: [127.0.0.1] => (item={'pkg': 'policycoreutils-python-utils', 'when': "db_path != '/var/lib/mongodb' or log_file != '/var/log/mongodb/mongod.log'"})

      TASK [Set SELinux to Permissive mode.] *************************************************************
      skipping: [127.0.0.1]

      TASK [Disable SELinux permanently in configuration.] ***********************************************
      skipping: [127.0.0.1]

      TASK [Ensure db and log files exist and are owned by mongod user.] *********************************
      changed: [127.0.0.1] => (item={'path': '/var/lib/mongodb', 'state': 'directory', 'recurse': True})
      changed: [127.0.0.1] => (item={'path': '/var/log/mongodb/mongod.log', 'state': 'touch', 'recurse': False})

      TASK [Change db and/or log file paths.] ************************************************************
      changed: [127.0.0.1] => (item={'key': 'dbPath', 'value': '/var/lib/mongodb', 'when': "db_path != '/var/lib/mongodb'"})
      ok: [127.0.0.1] => (item={'key': 'path', 'value': '/var/log/mongodb/mongod.log', 'when': "log_file != '/var/log/mongodb/mongod.log'"})

      TASK [Start mongod service.] ***********************************************************************
      changed: [127.0.0.1]

      TASK [Check mongod status.] ************************************************************************
      changed: [127.0.0.1]

      TASK [Print mongod status.] ************************************************************************
      ok: [127.0.0.1] =>
      msg: |-
      ● mongod.service - MongoDB Database Server
       Loaded: loaded (/usr/lib/systemd/system/mongod.service; enabled; vendor preset: disabled)
       Active: active (running) since Mon 2025-03-10 21:51:38 UTC; 288ms ago
         Docs: https://docs.mongodb.org/manual
      Main PID: 490438 (mongod)
       Memory: 70.3M
       CGroup: /system.slice/mongod.service
               └─490438 /usr/bin/mongod -f /etc/mongod.conf

      Mar 10 21:51:38 zdblab05 systemd[1]: Started MongoDB Database Server.
      Mar 10 21:51:38 zdblab05 mongod[490438]: {"t":{"$date":"2025-03-10T21:51:38.431Z"},"s":"I",  "c":"CONTROL",  "id":7484500, "ctx":"main","msg":"Environment variable MONGODB_CONFIG_OVERRIDE_NOFORK == 1, overriding \"processManagement.fork\" to false"}

      TASK [Check mongod version.] ***********************************************************************
      changed: [127.0.0.1]

      TASK [Print mongod version.] ***********************************************************************
      ok: [127.0.0.1] =>
      msg: mongod_version.stdout

      TASK [Set bindIP to 0.0.0.0 in mongod.conf] ********************************************************
      changed: [127.0.0.1]

      TASK [Verify MongoDB Enterprise Edition installed successfully.] ***********************************
      changed: [127.0.0.1]

      TASK [Congratulations!] ****************************************************************************
      ok: [127.0.0.1] =>
      msg: MongoDB installation complete.

      PLAY RECAP *****************************************************************************************
      127.0.0.1                  : ok=13   changed=8    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0

      [root@zdblab05 zmongodb]#
     
      ```

### MongoDB Uninstallation
#### To Uninstall MongoDB
In case if you want to uninstall MongoDB you can run the ansible playbook to uninstall mongoDB. Use the following command:

``` bash
   ansible-playbook mongodbUnInstall.yml 
```

???- example "The following example shows the output of succesful MongoDB uninstalltion [click to expand me]"

      ```
      [root@zdblab05 zmongodb]# ansible-playbook mongodbUnInstall.yml

      PLAY [UnInstall MongoDB on a Linux Guest.] *********************************************************

      TASK [Gathering Facts] *****************************************************************************
      ok: [127.0.0.1]

      TASK [Gather info about installed  services.] ******************************************************
      ok: [127.0.0.1]

      TASK [Stop mongod service.] ************************************************************************
      changed: [127.0.0.1]

      TASK [Uninstall MongoDB.] **************************************************************************
      changed: [127.0.0.1]

      TASK [Remove data and log directories] *************************************************************
      ok: [127.0.0.1] => (item=/var/log/mongodb/mongod.log)
      changed: [127.0.0.1] => (item=/var/lib/mongodb)

      TASK [Done.] ***************************************************************************************
      ok: [127.0.0.1] =>
      msg: MongoDB has been uninstalled successfully.

      PLAY RECAP *****************************************************************************************
      127.0.0.1                  : ok=6    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

      [root@zdblab05 zmongodb]#
      ```
!!! Note
    In case you have **_uninstalled_** _MongoDB_, go back and install it again by running the Ansible playbook to install **MongoDB**. 
