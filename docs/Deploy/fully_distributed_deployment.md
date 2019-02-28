Typically, when you get started with WSO2 API Manager in a development environment, you deploy WSO2 API Manager as a single instance with all its components on a single server. See the instructions on [configuring a single node deployment](single_node_deployment.md), or [configuring a single node HA deployment](active_active_deployment.md).

However, in a production deployment, the five APIM components need to be distributed among separate produc nodes. Follow the instructions given below.

# Prerequisites

See that the following prerequisites are in place:

* Note that your configurations may vary depending on the WSO2 API Manager deployment pattern that you choose. If you are using multi-tenancy, all nodes should use the same user store, as all servers are servicing the same set of tenants, and it has to share the same Governance Registry space across all nodes.
* In a clustered setup, if the Key Manager is NOT fronted by a load balancer, you have to set the KeyValidatorClientType element to ThriftClient in the <API-M_HOME>/repository/conf/api-manager.xml file, to enable Thrift as the communication protocol. You need to configure this in all the Gateway and Key Manager component.

# Installing WSO2 APIM

The following steps describe how to download, install, and configure WSO2 API Manager, with five instances.

* Download the WSO2 API Manager in each of the five servers in the cluster for distributed deployment.
* Unzip the WSO2 API Manager zipped archive, and rename each of those directories respectively as Key Manager, Gateway, Publisher, Store, and Traffic Manager. These five directories are located in a server of their own and are used for each component of WSO2 API-M. Each of these unzipped directories are referred to as <API-M_HOME> or <PRODUCT_HOME> in this document.

# Creating and importing SSL certificates

The default keystore and truststore of your APIM instance is located in the /repository/security/ directory. Create a SSL certificate for each of the WSO2 API-M nodes (e.g., Publisher, Store, Key Manager, Gateway, and Traffic Manager) and import them to the keyStore and the trustStore. For more information, see Creating SSL Certificates. 

Note that you should use the same primary keystore for all the API Manager instances here in order to decrypt the registry resources. For more information, see Configuring Primary Keystores in the Administration Guide. When creating the keystore, always use a longer validity period so that it will avoid the need of migration on the registry data when shifting to a new keystore.

# Setting up the load balancer

Go to [Configuring the Load Balancer](configuring_load_balancing.md), and follow the instructions on setting up a load balancer for a single node deployment of APIM.

<p style="background-color: #C2F9DE;padding: 10px">
	In a clustered environment, you use session affinity (sticky sessions) to ensure that requests from the same client always get routed to the same server. </br>
	It is  mandatory  to set up Session Affinity in the load balancers that front the  Publisher  and  Store  clusters, and it is  optional  in the load balancer (if any) that fronts a  Key Manager  cluster or Gateway Cluster. However, you need to enable Session Affinity if you are working with multiple Gateway Managers in a Gateway High Availability (HA) deployment. </br>
	However, authentication via session ID fails when session affinity is disabled in the load balancer.</br>
	First time authentication happens via Basic Auth and the Gateway gets a cookie. This cookie is used in every consequent request along with the Basic Auth credentials. The admin service validates the cookie and if the validation fails it re-authenticates it using Basic Auth and issues a new cookie.
</p>

# Setting up the databases

Go to [Configuring Databases for APIM](configuring_db_for_apim.md), and follow the instructions to setting up all the required databases.

# Configuring the Key Manager Node

Follow the steps given below to configure the key manager node

1. Open the apim.toml file of the APIM node, which is stored in the <APIM_KM_HOME>/conf/ directory.
2. To specify the default settings that should be applicable to the apim component(s), copy the following to the apim.toml file and update the values.
  <b>
   ``` java
	// This config header groups the parameters that define the default component settings that should aplly to the APIM node.
	[deployment]

	// This property specifies which apim component the apim node will run as. When this parameter is set to 'keymanager' the general configurations that are needed for the key manager deployment are inferred by default.
	node_act_as="keymanager"
   ```
 </b>
   To find additional parameters for configuring the default apim component, see the [**configuration catalog**](../all_configs/all_product_configs.md#configuring-the-default-apim-component).

3. To connect the APIM node to the load balancer, copy the following to the apim.toml file and update the values.
  <b>
   ``` java
	// This config header groups the parameters that define the connection from the APIM node to the load balancer.
	[reverse_proxy_connection]

	// Set the following property to enable load balancing. You can change the value to 'auto' based on the X-Forwarded funciton.
	enabled="true"

	// The hostname for the load balancer. If reverse proxy do not have a domain name use IP.
	host="hostname"

	// The context name.
	context="/publisher"

	// If a different path is used for registy, uncomment this config. Use only if different path is used for registry.
	regContext="value"
   ```
   </b>
    To find additional parameters for configuring the reverse proxy, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-the-load-balanceproxy-server).

4. To enable all the APIM components to access the shared database, copy the following to the apim.toml file and update the values.
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
    To find additional parameters for configuring the database connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-data-stores).

5. The Key Manager, Publisher, and Store components of the APIM node should be able to access the API Manager database. To configure this requirement, add the following to the apim.toml file and update the values.
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
     To find additional parameters for configuring the database connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-data-stores).

6. The Key Manager, Publisher, and Store components of the APIM node should be granted permissions to access and manage users and roles in the system. To configure this requirement, add the following to the apim.toml file and update the values.
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
    To find additional parameters for connecting to the user store, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-the-user-store).

 7. example configuration....
    <b>
    ``` java
    // This config section groups the parameters that are required for...
    [sample_section]
    
    // Description of the sample parameter.
    sample_parameter="value"
    ```
    </b>
    To find additional parameters for configuring the gateway connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#configuring-example).

# Configuring the API Publisher Node

Follow the steps given below to configure the API publisher node

1. Open the apim.toml file of the APIM node, which is stored in the <APIM_KM_HOME>/conf/ directory.
2. To specify the default settings that should be applicable to the apim component(s), copy the following to the apim.toml file and update the values.
  <b>
   ``` java
	// This config header groups the parameters that define the default component settings that should aplly to the APIM node.
	[deployment]

	// This property specifies which apim component the apim node will run as. When this parameter is set to 'publisher' the general configurations that are needed for the publisher deployment are inferred by default. 
	node_act_as="publisher"
   ```
   </b>
   To find additional parameters for configuring the default apim component, see the [**configuration catalog**](../all_configs/all_product_configs.md#configuring-the-default-apim-component).

3. To connect the APIM node to the load balancer, copy the following to the apim.toml file and update the values.
  <b>
   ``` java
	// This config header groups the parameters that define the connection from the APIM node to the load balancer.
	[reverse_proxy_connection]

	// Set the following property to enable load balancing. You can change the value to 'auto' based on the X-Forwarded funciton.
	enabled="true"

	// The hostname for the load balancer. If reverse proxy do not have a domain name use IP.
	host="hostname"

	// The context name.
	context="/publisher"

	// If a different path is used for registy, uncomment this config. Use only if different path is used for registry.
	regContext="value"
   ```
   </b>
   To find additional parameters for configuring the reverse proxy, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-the-load-balanceproxy-server).

4. To enable all the APIM components to access the shared database, copy the following to the apim.toml file and update the values.
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

    To find additional parameters for configuring the database connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-data-stores).

5. The Key Manager, Publisher, and Store components of the APIM node should be able to access the API Manager database. To configure this requirement, add the following to the apim.toml file and update the values.
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
     To find additional parameters for configuring the database connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-data-stores).

6. The Key Manager, Publisher, and Store components of the APIM node should be granted permissions to access and manage users and roles in the system. To configure this requirement, add the following to the apim.toml file and update the values.
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
    To find additional parameters for connecting to the user store, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-the-user-store).

 7. example configuration....
    <b>
    ``` java
    // This config section groups the parameters that are required for...
    [sample_section]
    
    // Description of the sample parameter.
    sample_parameter="value"
    ```
    </b>
    To find additional parameters for configuring the gateway connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#configuring-example).

# Configuring the API Store Node

Follow the steps given below to configure the API Store node

1. Open the apim.toml file of the APIM node, which is stored in the <APIM_KM_HOME>/conf/ directory.
2. To specify the default settings that should be applicable to the apim component(s), copy the following to the apim.toml file and update the values.
  <b>
   ``` java
	// This config header groups the parameters that define the default component settings that should aplly to the APIM node.
	[deployment]

	// This property specifies which apim component the apim node will run as. When this parameter is set to 'store' the general configurations that are needed for the API store deployment are inferred by default. 
	node_act_as="store"
   ```
   </b>
    To find additional parameters for configuring the default apim component, see the [**configuration catalog**](../all_configs/all_product_configs.md#configuring-the-default-apim-component).

3. To connect the APIM node to the load balancer, copy the following to the apim.toml file and update the values.
  <b>
   ``` java
	// This config header groups the parameters that define the connection from the APIM node to the load balancer.
	[reverse_proxy_connection]

	// Set the following property to enable load balancing. You can change the value to 'auto' based on the X-Forwarded funciton.
	enabled="true"

	// The hostname for the load balancer. If reverse proxy do not have a domain name use IP.
	host="hostname"

	// The context name.
	context="/publisher"

	// If a different path is used for registy, uncomment this config. Use only if different path is used for registry.
	regContext="value"
   ```
   </b>
   To find additional parameters for configuring the reverse proxy, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-the-load-balanceproxy-server).

4. To enable all the APIM components to access the shared database, copy the following to the apim.toml file and update the values.
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
    To find additional parameters for configuring the database connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-data-stores).

5. The Key Manager, Publisher, and Store components of the APIM node should be able to access the API Manager database. To configure this requirement, add the following to the apim.toml file and update the values.
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
     To find additional parameters for configuring the database connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-data-stores).

6. The Key Manager, Publisher, and Store components of the APIM node should be granted permissions to access and manage users and roles in the system. To configure this requirement, add the following to the apim.toml file and update the values.
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
    To find additional parameters for connecting to the user store, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-the-user-store).

 7. example configuration....
    <b>
    ``` java
    // This config section groups the parameters that are required for...
    [sample_section]
    
    // Description of the sample parameter.
    sample_parameter="value"
    ```
    </b>
    To find additional parameters for configuring the gateway connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#configuring-example).

# Configuring the Traffic Manager Node

Follow the steps given below to configure the Traffic Manager node

1. Open the apim.toml file of the APIM node, which is stored in the <APIM_KM_HOME>/conf/ directory.
2. To specify the default settings that should be applicable to the apim component(s), copy the following to the apim.toml file and update the values.
  <b>
   ``` java
	// This config header groups the parameters that define the default component settings that should aplly to the APIM node.
	[deployment]

	// This property specifies which apim component the apim node will run as. When this parameter is set to 'trafficmanager' the general configurations that are needed for the traffic manager deployment are inferred by default. 
	node_act_as="trafficmanager"
   ```
   </b>
  To find additional parameters for configuring the default apim component, see the [**configuration catalog**](../all_configs/all_product_configs.md#configuring-the-default-apim-component).

3. To connect the APIM node to the load balancer, copy the following to the apim.toml file and update the values.
  <b>
   ``` java
	// This config header groups the parameters that define the connection from the APIM node to the load balancer.
	[reverse_proxy_connection]

	// Set the following property to enable load balancing. You can change the value to 'auto' based on the X-Forwarded funciton.
	enabled="true"

	// The hostname for the load balancer. If reverse proxy do not have a domain name use IP.
	host="hostname"

	// The context name.
	context="/publisher"

	// If a different path is used for registy, uncomment this config. Use only if different path is used for registry.
	regContext="value"
   ```
   </b>
   To find additional parameters for configuring the reverse proxy, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-the-load-balanceproxy-server).

4. To enable all the APIM components to access the shared database, copy the following to the apim.toml file and update the values.
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
  To find additional parameters for configuring the database connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-data-stores).

5. The Key Manager, Publisher, and Store components of the APIM node should be able to access the API Manager database. To configure this requirement, add the following to the apim.toml file and update the values.
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
     To find additional parameters for configuring the database connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-data-stores).

6. The Key Manager, Publisher, and Store components of the APIM node should be granted permissions to access and manage users and roles in the system. To configure this requirement, add the following to the apim.toml file and update the values.
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
    To find additional parameters for connecting to the user store, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-the-user-store).

 7. example configuration....
    <b>
    ``` java
    // This config section groups the parameters that are required for...
    [sample_section]
    
    // Description of the sample parameter.
    sample_parameter="value"
    ```
    </b>
  To find additional parameters for configuring the gateway connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#configuring-example).

# Configuring the Gateway Node

Follow the steps given below to configure the Gateway node

1. Open the apim.toml file of the APIM node, which is stored in the <APIM_KM_HOME>/conf/ directory.
2. To specify the default settings that should be applicable to the apim component(s), copy the following to the apim.toml file and update the values.
  <b>
   ``` java
	// This config header groups the parameters that define the default component settings that should aplly to the APIM node.
	[deployment]

	// This property specifies which apim component the apim node will run as. When this parameter is set to 'gateway' the general configurations that are needed for the gateway deployment are inferred by default. 
	node_act_as="gateway"
   ```
   </b>
  To find additional parameters for configuring the default apim component, see the [**configuration catalog**](../all_configs/all_product_configs.md#configuring-the-default-apim-component).

3. To connect the APIM node to the load balancer, copy the following to the apim.toml file and update the values.
  <b>
   ``` java
	// This config header groups the parameters that define the connection from the APIM node to the load balancer.
	[reverse_proxy_connection]

	// Set the following property to enable load balancing. You can change the value to 'auto' based on the X-Forwarded funciton.
	enabled="true"

	// The hostname for the load balancer. If reverse proxy do not have a domain name use IP.
	host="hostname"

	// The context name.
	context="/publisher"

	// If a different path is used for registy, uncomment this config. Use only if different path is used for registry.
	regContext="value"
   ```
   </b>
  To find additional parameters for configuring the reverse proxy, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-the-load-balanceproxy-server).

4. To enable all the APIM components to access the shared database, copy the following to the apim.toml file and update the values.
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
  To find additional parameters for configuring the database connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-data-stores).

5. The Key Manager, Publisher, and Store components of the APIM node should be able to access the API Manager database. To configure this requirement, add the following to the apim.toml file and update the values.
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
  To find additional parameters for configuring the database connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-data-stores).

6. The Key Manager, Publisher, and Store components of the APIM node should be granted permissions to access and manage users and roles in the system. To configure this requirement, add the following to the apim.toml file and update the values.
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
    To find additional parameters for connecting to the user store, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-the-user-store)

 7. example configuration....
    <b>
    ``` java
    // This config section groups the parameters that are required for...
    [sample_section]
    
    // Description of the sample parameter.
    sample_parameter="value"
    ```
    </b>
    To find additional parameters for configuring the gateway connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#configuring-example).

# Configuring APIM Analytics

If you wish to view reports, statistics, and graphs related to the APIs deployed in the Store, you need to configure API-M Analytics. Follow the standard setup to configure API-M Analytics in a production setup, and follow the quick setup to configure API-M Analytics in a development setup. See [Configuring APIM Analytics](analytics_deployment.md) for instructions.

# Verifying security hardening

Ensure that you have taken into account the respective security hardening factors (e.g., changing and encrypting the default passwords, configuring JVM security etc.) before deploying WSO2 API-M. For more information, see the [Production Deployment Guidelines](deployment_guidelines.md).

# Starting the WSO2 APIM servers

Start the server using the following standard start-up script.

* On <b>Linux/Mac Os</b>: `cd <API-M_HOME>/bin/sh wso2server.sh`
* On <b>Windows</b>: `cd <API-M_HOME>\bin\wso2server.bat --run`