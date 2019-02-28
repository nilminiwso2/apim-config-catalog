Follow the instructions in this guide to set up WSO2 Identity Server as the Key Manager component in your APIM deployment, while the rest of the APIM components are set up in one or many instances of WSO2 APIM.

#
# Configuring WSO2 Identity Server as the Key Manager

Follow the steps given below.

1. Download the prepackaged WSO2 Identity Server from here and unzip it.  <IS_KM_HOME> will refer to the root folder of the unzipped WSO2 IS pack.
2. Open the apim.toml file of the WSO2 IS node, which is stored in the <IS_HOME>/conf/ directory.
3. If you are running WSO2 IS on the same machine as the WSO2 APIM node, set a port offset for the IS node. This ensures that the two servers can start simultaneously without port conflicts.
  <b>
   ``` java
   //This config section is added to the .toml file to define.....
   [Config_Section]

   //This config section is added to the .toml file to define.....
   parameter1="value"

   //This config section is added to the .toml file to define.....
   parameter2="value"
   ```
   </b>
    To find additional parameters for configuring the........, see the [**configuration catalog**](../).

4. If you have enabled Hazelcast clustering in the API-M Gateway node, add the following configurations to the iam.toml file to allow the Key Manager to communicate with the Gateway.
  <b>
   ``` java
   //This config section is added to the .toml file to define.....
   [Config_Section]

   //When users and roles are removed via the Key Manager, if the corresponding user tokens are cached on the Gateway, these tokens will only get invalidated when the cache is timed out. However, if Hazelcast clustering is enabled, token invalidation takes place immediately. Therefore, you need to enable communication between the Key Manager and Gateway to enable immediate token invalidation.
   parameter1="value"

   //When tokens are revoked, the corresponding token cache entries should be cleared in the Gateway. For this purpose, open the <IS_KM_HOME>/repository/conf/api-manager.xml file and change the <RevokeAPIURL> element that appears under the <OAuthConfigurations> section, so that it points to the WSO2 API Manager server, or the Gateway worker server if it is a distributed setup. Note the port used here is the NIO port, which is 8243 by default for HTTPS.
   parameter2="value"
   ```
 </b>
    To find additional parameters for configuring the........, see the [**configuration catalog**](../).

5. To enable the the key manager to access the shared database of APIM, add the following to the iam.toml file and update the values:
  <b>
   ``` java
    // The config section that groups the parameters for the apim database.
	[database.shared]

	// The 'type' parameter specifies the type of the database that is connected to the APIM server.
	type = "mysql"

	// The 'hostname' parameter specifies the host on which the database is run.
	hostname = "localhost"

	// The port of the MySQl server is 3306 by default.
	port = 3306

	// The name of the database is sharedDb.
	name = "sharedDb"

	// The username for connecting to the database. By default, 'root' is the MySQL username.
	username = "root"

	// The password for connecting to the database. By default, 'root' is the MySQL password.
	password = "root"

	// The maximum active threads in the pool.
	pool_options.max_active = 5
   ```
 </b>
    To find additional parameters for configuring the........, see the [**configuration catalog**](../).

6. To enable the key manager to access the API Manager database, add the following to the iam.toml file and update the values:
  <b>
   ``` java
   // The config section that groups the parameters for the apim database.
   [database.apim]

   // The 'type' parameter specifies the type of the database that is connected to the APIM server.
   type =  "mysql"

   // The 'hostname' parameter specifies the host on which the database is run.
   hostname = "localhost"

   // The port of the MySQl server is 3306 by default.
   port = 3306

   // The name of the database is sharedDb.
   name = "sharedDb"

   // The username for connecting to the database. By default, 'root' is the MySQL username.
   username = "root"
   
   // The password for connecting to the database. By default, 'root' is the MySQL password.
   password = "root"

   // The maximum active threads in the pool.
   pool_options.max_active = 5
   ```
 </b>
     To find additional parameters for configuring the........, see the [**configuration catalog**](../).

7. To give user management access to key manager, add the following to the iam.toml file.
    <b>
    ``` java
    // The config section that groups the parameters for the user store.
	[user_store]
	
	// The type of user store, which is a JDBC database.
	type = "jdbc"
	
	// This config section is added to the .toml file to define.....
    property1="value"

    // This config section is added to the .toml file to define.....
    property2="value"
	
	// This config section is added to the .toml file to define.....
	property3="value"
	
	// This config section is added to the .toml file to define.....
	property4="value"
	
	// This config section is added to the .toml file to define.....
	property5="value"
    ```
  </b>
    To find additional parameters for configuring the........, see the [**configuration catalog**](../).

8. Add the following configuration to the iam.toml file, to configure the JSON Web Token (JWT) in the WSO2 Identity Server. For more information on JWT Token generation, see Passing Enduser Attributes to the Backend Using JWT.  
   <b>
   ``` java
   //This config section is added to the .toml file to define.....
   [Config_Section]

   //This config section is added to the .toml file to define.....
   parameter1="value"

   //This config section is added to the .toml file to define.....
   parameter2="value"
   ```
   </b>
	
    To find additional parameters for configuring the........, see the [**configuration catalog**](../).

# Configuring High Availability for the Key Manager

These steps are **ONLY** applicable if you need to configure **HA for the Key Manager**.

1. Make a copy of the active instance configured above and use this copy as the second Key Manager active instance.
2. Configure a Load Balancer to front the two Key Manager nodes. For more information, see [Configuring the Load Balancer](configuring_load_balancing.md).

# Configuring the WSO2 APIM node with the Key Manager

1. Open the apim.toml file stored in the <APIM_HOME>/conf/ directory.
2. Add the following configurations to the apim.toml file so that WSO2 API Manager will be aware of the URL of the Key Manager, which in this case is WSO2 Identity Server, in order to handover the Key validation and Authorization related tasks. 
  <b>
   ```
	// The config section that groups the parameters for the user store.
	[Config_Section]

	//Change the ServerURL of the  AuthManager and the ServerURL of the APIKeyValidator to point to WSO2 IS.
	AuthManager = "jdbc"

	// If you are working with a single <b>Key Manager</b> in hybrid single node setup where WSO2 IS is the Key Manager and the rest of the API-M components are in one node, you need to replace {IS-server-host} with the actual host of the WSO2 IS sever node.
	IS-server-host="value"

	// Enable WS Client and disable the Thrift Client. Do this by changing the <KeyValidatorClientType> from ThriftClient to WSClient, and by setting <EnableThriftServer> to false  to optimize performance.
	EnableThriftServer="value"
   ```
 </b>
  To find additional parameters for configuring the........, see the [**configuration catalog**](../).

3. Apply the other required configurations to your APIM nodes. See the topic relevant to your deployment pattern for instructions:
    * [Configuring a Single Node Deployment](single_node_deployment.md)
    * [Configuring an Active-Active Deployment](active_active_deployment.md)
    * [Configuring a Fully Distributed Deployment](fully_distributed_deployment.md)

# Starting the Key Managers

Start WSO2 Identity Server for the changes to take effect. For more information, see Running the Product in the WSO2 Identity Server documentation. See the troubleshooting guide.

* On <b>Linux/Mac Os</b>: `cd <IS_HOME>/bin/sh wso2server.sh`
* On <b>Windows</b>: `cd <IS_HOME>\bin\wso2server.bat --run`