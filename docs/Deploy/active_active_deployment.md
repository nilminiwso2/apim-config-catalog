This page walks you through how to manually configure WSO2 API Manager (WSO2 APIM) with two active nodes that each have all the components of the APIM together in one instance (all-in-one instance). 

Read more about all the [deployment patterns](deployment_patterns.md) supported by WSO2 API Manager. To start deploying the **standalone single-node HA** pattern, follow the instructions given below.

# Prerequisites

See that the following prerequisites are in place:

* <b>Hardware requirements</b>: 	Ensure that the minimum hardware requirements mentioned in the hardware requirements section are met. Since this is an all-in-one deployment, it is recommended to use a higher hardware specification. You can further fine tune your operating system for production by tuning performance. For more information on installing the product on different operating systems, see Installing the Product.
* <b>Software requirements</b>: Oracle JDK 1.8 or Open JDK 8

# Installing WSO2 APIM

Follow the instructions here to download and install WSO2 APIM.

# Setting up the load balancer

Go to [Configuring the Load Balancer](configuring_load_balancing.md), and follow the instructions on setting up a load balancer for a single node deployment of APIM.

# Setting up the databases

Go to [Configuring Databases for APIM](configuring_db_for_apim.md), and follow the instructions to setting up all the required databases.

# Creating and importing SSL certificates

The default keystore and truststore of your APIM instance is located in the <APIM_HOME>/repository/security/ directory. Create a new SSL certificate and import it to the keyStore and the trustStore. For more information, see Creating SSL Certificates. 

# Configuring the APIM node 1

Follow the steps given below.

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

    To find additional parameters for configuring the gateway connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#configuring-example).
    
8. You need to configure throttling so that the Gateway can publish data to the Traffic Manager in its own node and the Traffic Manager in the other node. This allows the same event to be sent to both servers at the same time. The WSO2 Complex Event Processor (WSO2 CEP) component that lies within the Traffic Manager receives and processes the data.
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
   
   To find additional parameters for configuring the gateway connection, see the [**configuration catalog**](../all_configs/all_product_configs.md#configuring-example).

<!--
<ol>
	<li>Open the apim.toml file of the APIM node, which is stored in the <APIM_HOME>/conf/ directory.</li>
	<li>To connect the APIM node to the load balancer, add the following configurations to the apim.toml file.
	<div style="background-color: #eee">
		<code>
        [Config_Section]</br>
        parameter="value"</br>
        parameter="value"</br>
		</code>
	</div> </br>
	See the descriptions of the above configurations given below:
	<table>
		<tr>
			<th>Configuration</th>
			<th>Description</th>
		</tr>
		<tr>
			<td><code>[Config_Section]</code></td>
			<td>This config section is added to the .toml file to define.....</td>
		</tr>
		<tr>
			<td><code>[parameter="value"</code></td>
			<td>This config section is added to the .toml file to define.....</td>
		</tr>
		<tr>
			<td><code>[parameter="value"</code></td>
			<td>This config section is added to the .toml file to define.....</td>
		</tr>
	</table>
	</li>
	<li>To enable the APIM components to access the shared database, add the following to the apim.toml file:
	<div style="background-color: #eee">
		<code>
		[database.apim] </br>
 		type =  "mysql"</br>
		hostname = "localhost"</br>
		port = 3306</br>
		name = "sharedDb"</br>
		username = "root"</br>
		password = "root"</br>
		pool_options.max_active = 5</br>
		</code>
	</div> </br>
	See the descriptions of the above configurations given below:
	<table>
		<tr>
			<th>Configuration</th>
			<th>Description</th>
		</tr>
		<tr>
			<td><code>[database.apim]</code></td>
			<td>The config section that groups the parameters for the apim database.</td>
		</tr>
		<tr>
			<td><code>type ="mysql"</code></td>
			<td>The 'type' parameter specifies the type of the database that is connected to the APIM server.</td>
		</tr>
		<tr>
			<td><code>hostname = "localhost"</code></td>
			<td>The 'hostname' parameter specifies the host on which the database is run.</td>
		</tr>
		<tr>
			<td><code>port = 3306</code></td>
			<td>The port of the MySQl server is 3306 by default.</td>
		</tr>
		<tr>
			<td><code>name = "sharedDb"</code></td>
			<td>The name of the database is sharedDb.</td>
		</tr>
		<tr>
			<td><code>username = "root"</code></td>
			<td>The username for connecting to the database. By default, 'root' is the MySQL username.</td>
		</tr>
		<tr>
			<td><code>pool_options.max_active = 5</code></td>
			<td>The maximum active threads in the pool.</td>
		</tr>
	</table>
	</li>
	<li>The Key Manager, Publisher, and Store components of the APIM node should be able to access the API Manager database. To configure this requirement, add the following to the apim.toml file and update the values.
	<div style="background-color: #eee">
		<code>
		[database.apim] </br>
 		type =  "mysql"</br>
		hostname = "localhost"</br>
		port = 3306</br>
		name = "sharedDb"</br>
		username = "root"</br>
		password = "root"</br>
		pool_options.max_active = 5</br>
		</code>
	</div> </br>
	See the descriptions of the above configurations given below:
	<table>
		<tr>
			<th>Configuration</th>
			<th>Description</th>
		</tr>
		<tr>
			<td><code>[database.apim]</code></td>
			<td>The config section that groups the parameters for the apim database.</td>
		</tr>
		<tr>
			<td><code>type ="mysql"</code></td>
			<td>The 'type' parameter specifies the type of the database that is connected to the APIM server.</td>
		</tr>
		<tr>
			<td><code>hostname = "localhost"</code></td>
			<td>The 'hostname' parameter specifies the host on which the database is run.</td>
		</tr>
		<tr>
			<td><code>port = 3306</code></td>
			<td>The port of the MySQl server is 3306 by default.</td>
		</tr>
		<tr>
			<td><code>name = "sharedDb"</code></td>
			<td>The name of the database is sharedDb.</td>
		</tr>
		<tr>
			<td><code>username = "root"</code></td>
			<td>The username for connecting to the database. By default, 'root' is the MySQL username.</td>
		</tr>
		<tr>
			<td><code>pool_options.max_active = 5</code></td>
			<td>The maximum active threads in the pool.</td>
		</tr>
	</table>
	</li>
	<li>The Key Manager, Publisher, and Store components of the APIM node should be granted permissions to access and manage users and roles in the system. To configure this requirement, add the following to the apim.toml file and update the values.
	<div style="background-color: #eee">
		<code>
			[user_store] </br>
			type = "jdbc" </br>
			property1="value" </br>
			property2="value" </br>
			property3="value" </br>
			property4="value" </br>
			property5="value" </br>
		</code>
	</div> </br>
	See the descriptions of the above configurations given below:
	<table>
		<tr>
			<th>Configuration</th>
			<th>Description</th>
		</tr>
		<tr>
			<td><code>[user_store]</code></td>
			<td>The config section that groups the parameters for the user store.</td>
		</tr>
		<tr>
			<td><code>type = "jdbc"</code></td>
			<td>The type of user store, which is a JDBC database. </br>
				<p style="background-color: #C2F9DE;padding: 10px"><b>Using WSO2 Identity Server as Key Manager?</b></br>If you are using the embedded LDAP that comes with WSO2 IS, then you need to point to the particular LDAP user store from WSO2 API Manager. You can copy this configuration from the <IS_KM_HOME>/repository/conf/user-mgt.xml file to the <API-M_HOME>/repository/conf/user-mgt.xml file.When copying configurations, note that you must update the ports. For instance, when configuring the ConnectionURL property, you must update the port, because otherwise it will point to the port of the Identity Server when starting up if you copy it directly. See the section below on Using WSO2 Identity Server as the key manager (Optional) for other configurations that are required for this setup</p>
			</td>
		</tr>
		<tr>
			<td><code>property1="value"</code></td>
			<td>This config section is added to the .toml file to define.....</td>
		</tr>
		<tr>
			<td><code>property1="value"</code></td>
			<td>This config section is added to the .toml file to define.....</td>
		</tr>
		<tr>
			<td><code>property1="value"</code></td>
			<td>This config section is added to the .toml file to define.....</td>
		</tr>
		<tr>
			<td><code>property1="value"</code></td>
			<td>This config section is added to the .toml file to define.....</td>
		</tr>
		<tr>
			<td><code>property1="value"</code></td>
			<td>This config section is added to the .toml file to define.....</td>
		</tr>
	</table>
	</li>
	<li><b>This step is only required if you are using a hostname to expose APIs.</b>To configure hostnames that are used to expose APIs, add the following config section:
	<div style="background-color: #eee">
		<code>
			[Config_Section] </br>
			parameter="value" </br>
			parameter="value" </br>
		</code>
	</div> </br>
	See the descriptions of the above configurations given below:
	<table>
		<tr>
			<th>Configuration</th>
			<th>Description</th>
		</tr>
		<tr>
			<td><code>[Config_Section]</code></td>
			<td>The config section that groups the parameters for the user store.</td>
		</tr>
		<tr>
			<td><code>parameter1="value"</code></td>
			<td>This config is added to the .toml file....</td>
		</tr>
		<tr>
			<td><code>parameter2="value"</code></td>
			<td>This config is added to the .toml file....</td>
		</tr>
	</table>
	</li>
	<li>
		You need to configure throttling so that the Gateway can publish data to the Traffic Manager in its own node and the Traffic Manager in the other node. This allows the same event to be sent to both servers at the same time. The WSO2 Complex Event Processor (WSO2 CEP) component that lies within the Traffic Manager receives and processes the data.
		<div style="background-color: #eee">
		<code>
			[Config_Section] </br>
			parameter="value" </br>
			parameter="value" </br>
		</code>
		</div> </br>
		See the descriptions of the above configurations given below:
		<table>
			<tr>
				<th>Configuration</th>
				<th>Description</th>
			</tr>
			<tr>
				<td><code>[Config_Section]</code></td>
				<td>The config section that groups the parameters for the user store.</td>
			</tr>
			<tr>
				<td><code>parameter1="value"</code></td>
				<td>This config is added to the .toml file....</td>
			</tr>
			<tr>
				<td><code>parameter2="value"</code></td>
				<td>This config is added to the .toml file....</td>
			</tr>
	</table>
	</li>
</ol>
-->

# Configuring the APIM node 2
Make a copy of the active instance (APIM node 1) configured above and use this copy as the second active instance. When making a copy of the node, you need to also make a copy of the SSL certificate that you created for node 1.

# Configuring Deployment Synchronization

Configure deployment synchronization so that all APIs and throttling policies can be shared between all the nodes. It is recommended to use a shared file system such as Network File System (NFS) or any other shared file system that is available. You can set this up by mounting the <API-M_HOME>/repository/deployment/server directory of the two nodes to the shared file system.

<p style="background-color: #C2F9DE;padding: 10px">A shared file system is the preferred method because APIs and throttling decisions can be published to any of the nodes, which will avoid the vulnerability of a single point of failure that is present when using remote synchronization (rsync). </br> However, if you are unable to maintain a shared file system, you can configuring rsync for deployment synchronization.</p>

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