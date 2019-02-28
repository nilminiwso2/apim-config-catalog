This page walks you through how to manually configure and deploy WSO2 API Manager in a standalone single instance without using a [distributed deployment](fully_distributed_deployment.md) or a [single-node HA deployment](active_active_deployment.md) pattern. In this setup, API traffic is served by one instance of WSO2 API Manager.

Read more about all the [deployment patterns](deployment_patterns.md) supported by WSO2 API Manager. To start deploying the **standalone single-node** pattern, follow the instructions given below.
# 

# Prerequisites

See that the following prerequisites are in place:

* <b>Hardware requirements</b>: Ensure that the minimum hardware requirements mentioned in the hardware requirements section are met. Since this is an all-in-one deployment, it is recommended to use a higher hardware specification. You can further fine tune your operating system for production by tuning performance. For more information on installing the product on different operating systems, see Installing the Product.
* <b>Software requirements</b>: Oracle JDK 1.8 or Open JDK 8

# Installing WSO2 APIM

Follow the instructions here to download and install WSO2 APIM on a single machine.

# Setting up the load balancer

Go to [Configuring the Load Balancer](configuring_load_balancing.md), and follow the instructions on setting up a load balancer for a single node deployment of APIM.

# Setting up the databases

Go to [Configuring Databases for APIM](configuring_db_for_apim.md), and follow the instructions to setting up all the required databases.

# Creating and importing SSL certificates

The default keystore and truststore of your APIM instance is located in the <APIM_HOME>/repository/security/ directory. Create a new SSL certificate and import it to the keyStore and the trustStore. For more information, see Creating SSL Certificates. 

# Configuring the APIM node

Follow the steps given below to configure the APIM node to serve API traffic.

1. Open the apim.toml file of the APIM node, which is stored in the <APIM_HOME>/conf/ directory.
2. To specify the default settings that should be applicable to the apim component(s), copy the following to the apim.toml file and update the values.
	<b>
   ``` java
	// This config header groups the parameters that define the default component settings that should aplly to the APIM node.
	[deployment]

	// This property specifies which apim component the apim node will run as. Since we are configuring a standalone deployment where all components are represented by a single node, use 'all' for this parameter. This infers the general configurations that are needed for a standalone deployment. Read more about the general configurations for a standalone deployment. 
	node_act_as="all"
   ```
   </b>
   To find additional parameters for configuring the default apim component, see the [**configuration catalog**](../all_configs/all_product_configs.md#configuring-the-default-apim-component).

3. To enable all the APIM components to access the shared database, copy the following to the apim.toml file and update the values.
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

4. The Key Manager, Publisher, and Store components of the APIM node should be able to access the API Manager database. To configure this requirement, add the following to the apim.toml file and update the values.
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

 5. The Key Manager, Publisher, and Store components of the APIM node should be granted permissions to access and manage users and roles in the system. To configure this requirement, add the following to the apim.toml file and update the values.
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

 6. To connect the APIM node to the load balancer, copy the following to the apim.toml file and update the values.
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

 7. Note that this step is only required if you are using a hostname to expose APIs.</b> To configure hostnames that are used to expose APIs, add the following config section.
 	<b>
    ``` java
    // This config section groups the parameters that are required for...
    [gateway_connnection]
    
    // Endpoint URLs for the APIs hosted in this API gateway.
    gateway_endpoint="value"
    ```
    </b>
    To find additional parameters for configuring the gateway connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#connecting-to-the-gateway-profile).

<!--
<ol>
	<li>Open the apim.toml file of the APIM node, which is stored in the <APIM_HOME>/conf/ directory.</li>
	<li>To connect the APIM node to the load balancer, copy the <b>required configurations</b> given below to the apim.toml file and update the values.
	<div style="background-color: #eee">
		<code>
			//This config header groups the parameters that define the connection from the APIM node to the load balancer.</br>
		    [reverse_proxy_connection]</br></br>
		     //Set the following property to enable load balancing. You can change the value to 'auto' based on the X-Forwarded funciton.</br>
		     enabled="true"</br></br>
		     //The hostname for the load balancer. If reverse proxy do not have a domain name use IP.</br>
		     host="hostname"</br></br>
		     //The context name.</br>
		     context="/publisher"</br></br>
		     // If a different path is used for registy, uncomment this config. Use only if different path is used for registry.</br>
		     # regContext="value"
		</code>
	</div>	
	</li>
	<li>To enable all the APIM components to access the shared database, copy the following to the apim.toml file and update the values.
	<div style="background-color: #eee">
		<code>

		</code>
	</div> </br>
	</li>
	<li>The Key Manager, Publisher, and Store components of the APIM node should be able to access the API Manager database. To configure this requirement, add the following to the apim.toml file and update the values.
	<div style="background-color: #eee">
		<code>
		//The config section that groups the parameters for the apim database.</br>
		[database.apim] </br></br>
		//The 'type' parameter specifies the type of the database that is connected to the APIM server.</br>
 		type =  "mysql"</br></br>
 		//The 'hostname' parameter specifies the host on which the database is run.</br>
		hostname = "localhost"</br></br>
		//The port of the MySQl server is 3306 by default.</br>
		port = 3306</br></br>
		//The name of the database is sharedDb.</br>
		name = "sharedDb"</br></br>
		//The username for connecting to the database. By default, 'root' is the MySQL username.</br>
		username = "root"</br></br>
		//The password for connecting to the database. By default, 'root' is the MySQL password.</br>
		password = "root"</br></br>
		//The maximum active threads in the pool.</br>
		pool_options.max_active = 5</br>
		</code>
	</div> </br>
	<li>The Key Manager, Publisher, and Store components of the APIM node should be granted permissions to access and manage users and roles in the system. To configure this requirement, add the following to the apim.toml file and update the values.
	<div style="background-color: #eee">
		<code>
			//The config section that groups the parameters for the user store.</br>
			[user_store] </br></br>
			//The type of user store, which is a JDBC database.</br>
			type = "jdbc" </br></br>
			//This config section is added to the .toml file to define.....</br>
			property1="value" </br></br>
			//This config section is added to the .toml file to define.....</br>
			property2="value" </br></br>
			//This config section is added to the .toml file to define.....</br>
			property3="value" </br></br>
			//This config section is added to the .toml file to define.....</br>
			property4="value" </br></br>
			//This config section is added to the .toml file to define.....</br>
			property5="value" </br>
		</code>
	</div> </br>	
	<li> <b>Note that this step is only required if you are using a hostname to expose APIs.</b> To configure hostnames that are used to expose APIs, add the following config section.
	<div style="background-color: #eee">
		<code>
		//This config section groups the parameters that are required for...</br>
        [gateway_connnection]</br></br>
        //Endpoint URLs for the APIs hosted in this API gateway.</br>
        gateway_endpoint="value"</br>
		</code>
	</div> </br>
	</li>
</ol>
-->

# Optional configurations

If you have already done the configurations explained above, you have the option of applying the following configurations for your deployment.

## [Configuring APIM Analytics](analytics_deployment.md)

If you wish to view reports, statistics, and graphs related to the APIs deployed in the Store, you need to configure API-M Analytics. Follow the standard setup to configure API-M Analytics in a production setup, and follow the quick setup to configure API-M Analytics in a development setup.

## [Configuring WSO2 Identity Server as key manager](using_is_as_key_manager.md)

In this guide, we have configured all the APIM components in the single product instance. You have the option of switching the key manager functionality to WSO2 Identity Server.

# Verifying security hardening

Ensure that you have taken into account the respective security hardening factors (e.g., changing and encrypting the default passwords, configuring JVM security etc.) before deploying WSO2 API-M. For more information, see the [Production Deployment Guidelines](deployment_guidelines.md).

# Starting the WSO2 APIM server

Start the server using the following standard start-up script.

* On <b>Linux/Mac Os</b>: `cd <API-M_HOME>/bin/sh wso2server.sh`
* On <b>Windows</b>: `cd <API-M_HOME>\bin\wso2server.bat --run`