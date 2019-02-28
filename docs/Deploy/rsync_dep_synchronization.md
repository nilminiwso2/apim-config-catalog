
If you are using rsync, the API artifacts will be synchronized to one direction. As explained in Configuring rsync for Deployment Synchronization section, the synchronization will happen from manager to worker. Hence, The API artifact should be created on one node only, which acts like a manager node for artifact synchronization purpose. Please follow the steps below to configure this:
# 

# Setting up the APIM nodes

Deployment synchronization only applies when you want to configure two APIM nodes for high availability.  see the instructions on how to configure the APIM node(s) in the deployment.

* [Configuring an Active-Active Deployment](active_active_deployment.md)
* [Configuring a Fully Distributed Deployment](fully_distributed_deployment.md)

# Configuring RSync
1. Create a file named workers-list.txt, somewhere in your machine, that lists all the worker nodes in the deployment. The following is a sample of the file where there are two worker nodes.
   ......
   ```java
    ubuntu@192.168.1.1:~/setup/192.168.1.1/as/as_worker/repository/deployment/server
	ubuntu@192.168.1.2:~/setup/192.168.1.2/as/as_worker/repository/deployment/server
   ```

2. Create a file to synchronize the <API-M_HOME>/repository/deployment/server folders between the manager and all worker nodes.
   ```
   #!/bin/sh
 	manager_server_dir=~/wso2as-5.2.1/repository/deployment/server/
	pem_file=~/.ssh/carbon-440-test.pem 
	
	#delete the lock on exit
	trap 'rm -rf /var/lock/depsync-lock' EXIT
 
	mkdir /tmp/carbon-rsync-logs/
 
 	#keep a lock to stop parallel runs
	if mkdir /var/lock/depsync-lock; then
  		echo "Locking succeeded" >&2
	else
  		 "Lock failed - exit" >&2
  	exit 1
	fi
 
 	#get the workers-list.txt
	pushd `dirname $0` > /dev/null
	SCRIPTPATH=`pwd`
	popd > /dev/null
	echo $SCRIPTPATH
 
 	for x in `cat ${SCRIPTPATH}/workers-list.txt`
	do
	echo ================================================== >> /tmp/carbon-rsync-logs/logs.txt;
	echo Syncing $x;
	rsync --delete -arve "ssh -i  $pem_file -o StrictHostKeyChecking=no" $manager_server_dir $x >> /tmp/carbon-rsync-logs/logs.txt
	echo ================================================== >> /tmp/carbon-rsync-logs/logs.txt;
		done
   ```

# Configuring the APIM nodes

Apply the following configurations to the APIM nodes in your deployment that requires to handle high availability.

1. Add the configurations shown below to the apim.toml file (stored in the <APIM_HOME>/conf directory) of node-1.
   	<b>
   ``` java
   // The config section that groups the parameters for the user store.
   [Config_Section]

   This config is added to the .toml file....
   parameter1="value"

   This config is added to the .toml file....
   parameter2="value"
   ```
   </b>
2. Add the configurations shown below to the apim.toml file (stored in the <APIM_HOME>/conf directory) of node-2. This configuration sets the gateway server URL to point to node-1. Note that <node-1-mgt-transport-port> is the management transport port, which is by default 9443.
   	<b>
   ``` java
   // The config section that groups the parameters for the user store.
   [Config_Section]

   This config is added to the .toml file....
   parameter1="value"

   This config is added to the .toml file....
   parameter2="value"
   ```
   </b>

3. Configure the API Publisher in both nodes to be able to publish to the API-M Gateway of one of the nodes. Do this by pointing the <ServerURL> to the same Gateway node. This step is required only if you are using rsync to share files. When you use rsync the file synchronization will happen in only one direction. Therefore, use the following configuration to enable synchronization in both directions between two nodes.
   	<b>
   ``` java
   // The config section that groups the parameters for the user store.
   [Config_Section]

   This config is added to the .toml file....
   parameter1="value"

   This config is added to the .toml file....
   parameter2="value"
   ```
   </b>