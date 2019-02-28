
This document describes all the configuration parameters that are used in WSO2 API Manager.
#  

# Instructions for use

> Select the configuration sections, parameters, and values that are required for your use and add them to the .toml file. See the example .toml file given below.

```
# This is an example .toml file.

[deployment]             #Config section.
pattern="value"          #Parameter-value pair.
node_act_as="value"      #Parameter-value pair.

[key_mgr_node]           #Config section.
endpoints="value"        #Parameter-value pair.

[gateway].               #Config section.
gateway_environments=["dev","test"]   #Parameter-value pair.

[[database]]               #Config section
pool_options.maxActiv="5"

```

# Configuring the default APIM component

Use the configurations given below to specify the APIM profile that will be running on the APIM instance that you are configuring. 

<table style="width:100%">
  <tr >
    <th style="width:20%">Config Section</th>
    <th ></th>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[deployment]</b></code></td>
    <td> <b>Description:</b> This section groups the parameters that define the type of APIM profile that will be effective on the product instance that you are deploying. <br/><br/> <b>Default Behavior:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profile</a></li></ul> </td>
  </tr>
  <tr >
    <th>Parameters</th>
    <th ></th>
  </tr>
  <tr>
    <td><code><b>node_act_as</code></b></td>
    <td><b>Description:</b> When you deploy WSO2 APIM, you need to set up multiple profiles. Each product instance you deploy will serve as a single profile. You can specify which APIM profile the product instance acts as by giving the profile ID as a value for this parameter. <br/><br/> <b>Possible Values:</b> <ul><li><code><b>gateway</code></b>: The APIM server is configured as a gateway.</li><li><code><b>keymanager</code></b>: The APIM server is configured as an API key manager.</li><li><code><b>trafficmanager</code></b>: The APIM server is configured as an API traffic manager.</li><li><code><b>publisher</code></b>: The APIM server is configured as an API publisher.</li><li><code><b>store</code></b>: The APIM server is configured as an API store.</li></ul> <br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a></li></ul> <br/> <b>Example:</b> <code><b>node_act_as="gateway"</code></b> <br/> 
  </tr>
    <tr>
    <td><code><b>key_manager.url</code></b></td>
    <td> <b>Description:</b> The key manager URL. <br/><br/><b>Possible Values:</b> Use the domain name and port value of your key manager node to construct the key manager URL.  <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Deployment Pattern 1 </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Gateway Profile</a></li><li>Optional for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Deployment Pattern 1 </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Store Profile</a></li></ul> <b>Example:</b> <code><b>endpoints = "apim.km:9443"</code></b> <br/> </td>
  </tr>
</table>

# Connecting to Data Stores

There are two databases used by WSO2 API Manager: The shared database for user management data, and the apim database for apim-specific data. You need to use two separate config headings in the .toml file to configure the connection to each of these databases.

<table style="width:100%">
  <tr >
    <th style="width:20%" colspan="2">Config Section</th>
    <th ></th>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[database.shared]</b></code></td>
    <td> <b>Description:</b>This section groups the configurations that apply to the database that is used for user roles, user permissions, and registry data. See the section below on database parameters for the complete list of parameters that you can use to configure this database connection. <br/><br/> <b>Default Behavior:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a></li></ul> </td>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[database.apim]</b></code></td>
    <td> <b>Description:</b>This section groups the configurations that apply to the database that is used for APIM-specific data. See the section below on database parameters for the complete list of parameters that you can use to configure this database connection. <br/><br/> <b>Default Behavior:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a></li></ul> </td>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[[database]]</b></code></td>
    <td> <b>Description:</b>This section groups the configurations related to the required database connections. You can use this header multiple times to configure the required number of database connections. <br/><br/> <b>Default Behavior:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a></li></ul> </td>
  </tr>
</table>

Listed below are the parameters you can use when configuring the [database.shared] and [database.apim] sections in the .toml file.

<table>
  <tr >
    <th colspan="2">Parameters</th>
    <th ></th>
  </tr>
   <tr>
    <td><code><b>type</code></b></td>
    <td> <b>Description:</b>The type of database. <br/><br/> <b>Possible Values:</b> <br/> <ul><li><code><b>mysql</code></b></li><li><code><b>oracle</code></b></li></ul> <br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a></li></ul> <br/> <b>Example:</b> <code><b>type="mysql"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>hostname</code></b></td>
    <td> <b>Description:</b>The hostname of the database. <br/><br/> <b>Possible Values:</b> Give the hostname of the database server. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a> </li></ul> <br/> <b>Example:</b> <code><b>hostname="localhost"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>port</code></b></td>
    <td> <b>Description:</b>The port on which the database server runs. <br/><br/> <b>Possible Values:</b> Give the port on which your database runs. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a> </li></ul> <br/> <b>Example:</b> <code><b>port=3306<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>name</code></b></td>
    <td> <b>Description:</b>The name of the database. <br/><br/> <b>Possible Values:</b> Give the name of the database. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a> </li></ul> <br/> <b>Example:</b> <code><b>name="shardDB"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>username</code></b></td>
    <td> <b>Description:</b>The user name for connecting to the database. <br/><br/> <b>Possible Values:</b> Give the name of the database. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a> </li></ul> <br/> <b>Example:</b> <code><b>username="root"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>password</code></b></td>
    <td> <b>Description:</b>The password for connecting to the database. <br/><br/> <b>Possible Values:</b> Give the name of the database. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a> </li></ul> <br/> <b>Example:</b> <code><b>password="root"<code><b> <br/> </td>
  </tr>
  <tr >
    <th colspan="2">Parameters - Connection Pool</th>
    <th ></th>
  </tr>
   <tr>
    <td><code><b>pool_options.maxWait</code></b></td>
    <td> <b>Description:</b>Descroption...... <br/><br/> <b>Possible Values:</b> This parameter can use a single value. Use "" to specify the value. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Optional for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a></li></ul> <br/> <b>Example:</b> <code><b>pool_options.maxWait="5"<code><b> <br/> </td>
  </tr>
  <tr>
    <tr>
    <td><code><b>pool_options.minIdle</code></b></td>
    <td> <b>Description:</b>Descroption...... <br/><br/> <b>Possible Values:</b> This parameter can use a single value. Use "" to specify the value. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Optional for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a> </li></ul> <br/> <b>Example:</b> <code><b>pool_options.minIdle="5"<code><b> <br/> </td>
  </tr>
</table>

# Connecting to the User Store

Use the configurations given below to connect to the product's user store.

<table style="width:100%">
  <tr>
    <th style="width:20%" colspan="2">Config Section</th>
    <th ></th>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[user_store]</b></code></td>
    <td> <b>Description:</b>The config section that groups the parameters for the user store. <br/><br/> <b>Default Behavior:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a></li></ul> </td>
  </tr>
  <tr >
    <th colspan="2">Parameters</th>
    <th ></th>
  </tr>
   <tr>
    <td><code><b>type</code></b></td>
    <td> <b>Description:</b>The type of user store. <br/><br/> <b>Possible Values:</b> <br/> <ul><li><code><b>database</code></b></li><li><code><b>ldap</code></b></li></ul> <br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a></li></ul> <br/> <b>Example:</b> <code><b>type="mysql"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>userstore.property1</code></b></td>
    <td> <b>Description:</b>This config section is added to the .toml file to define..... <br/><br/> <b>Possible Values:</b>..... <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a> </li></ul> <br/> <b>Example:</b> <code><b>hostname="localhost"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>userstore.property2</code></b></td>
    <td> <b>Description:</b>This config section is added to the .toml file to define..... <br/><br/> <b>Possible Values:</b>..... <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a> </li></ul> <br/> <b>Example:</b> <code><b>hostname="localhost"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>userstore.property3</code></b></td>
    <td> <b>Description:</b>This config section is added to the .toml file to define..... <br/><br/> <b>Possible Values:</b>..... <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a> </li></ul> <br/> <b>Example:</b> <code><b>hostname="localhost"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>userstore.property4</code></b></td>
    <td> <b>Description:</b>This config section is added to the .toml file to define..... <br/><br/> <b>Possible Values:</b>..... <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a> </li></ul> <br/> <b>Example:</b> <code><b>hostname="localhost"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>userstore.property5</code></b></td>
    <td> <b>Description:</b>This config section is added to the .toml file to define..... <br/><br/> <b>Possible Values:</b>..... <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a> </li></ul> <br/> <b>Example:</b> <code><b>hostname="localhost"<code><b> <br/> </td>
  </tr>
</table>

# Connecting to the Key Manager Profile

Use the configurations given below to connect to the APIM Key Manager node.

<table style="width:100%">
  <tr >
    <th style="width:20%">Config Section</th>
    <th ></th>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[key_mgr_node]</b></code></td>
    <td> <b>Description:</b>This section groups the configuration that are required by the APIM node (which you are currently setting up) to communicate with the key manager node in your deployment. <br/><br/> <b>Default Behavior:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Gateway Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Store Profile</a></li></ul> </td>
  </tr>
  <tr >
    <th>Parameters</th>
    <th ></th>
  </tr>
    <tr>
    <td><code><b>endpoints</code></b></td>
    <td> <b>Description:</b>The endpoint (specified using domain:port) of the key manager node in your APIM deployment. If your deployment pattern uses multiple key manager installations, they will be fronted by a load balancer. In such deployments, the endpoint should be load balancer. <br/><br/> <b>Possible Values:</b> Use the domain name and port value of your key manager node to construct the endpoint value. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Deployment Pattern 1 </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Gateway Profile</a></li><li>Optional for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Deployment Pattern 1 </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Store Profile</a></li></ul> <br/> <b>Example:</b> <code><b>endpoints = "km.wso2:9443"<code><b> <br/> </td>
  </tr>
</table>

# Connecting to the Traffic Manager Profile

Use the configurations given below to connect to the APIM Traffic Manager node.

<table style="width:100%">
  <tr >
    <th style="width:20%">Config Section</th>
    <th ></th>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[traffic_mgr_node]</b></code></td>
    <td> <b>Description:</b>This section groups the configuration that are required by the APIM node (which you are currently setting up) to communicate with the traffic manager node in your deployment. <br/><br/> <b>Default Behavior:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Gateway Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Store Profile</a></li></ul> </td>
  </tr>
  <tr >
    <th>Parameters</th>
    <th ></th>
  </tr>
   <tr>
    <td><code><b>traffic_publishers</code></b></td>
    <td> <b>Description:</b>The URLs of the traffic manager node(s) in your deployment. The current APIM node that you are configuring will publish traffic to the given nodes.... <br/><br/> <b>Possible Values:</b> Give the URL patterns of all the publisher nodes. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Deployment Pattern 1 </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Gateway Profile</a></li><li>Optional for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Deployment Pattern 1 </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Store Profile</a></li></ul> <br/> <b>Example:</b> <code><b>traffic_publishers="{tcp://tm1.node:9611},{tcp://tm2.node:9611}"<code><b> <br/> </td>
  </tr>
   <tr>
    <td><code><b>throttle_decision_endpoints</code></b></td>
    <td> <b>Description:</b>The throttle decision endpoints...... <br/><br/> <b>Possible Values:</b> Use the domain name and port value of your key manager node to construct the endpoint value. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Deployment Pattern 1 </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Gateway Profile</a></li><li>Optional for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Deployment Pattern 1 </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Store Profile</a></li></ul> <br/> <b>Example:</b> <code><b>throttle_decision_endpoints=["tm1.node:5672"]<code><b> <br/> </td>
  </tr>
    <td><code><b>mgt_endpoint</code></b></td>
    <td> <b>Description:</b>The endpoint of the management console...... <br/><br/> <b>Possible Values:</b> Use the domain name and port value of your management console to construct the endpoint value. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Deployment Pattern 1 </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Gateway Profile</a></li><li>Optional for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Deployment Pattern 1 </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Store Profile</a></li></ul> <br/> <b>Example:</b> <code><b>mgt_endpoint="tm.wso2.com:9443<code><b> <br/> </td>
  </tr>
</table>

# Connecting to the Gateway Profile

Use the configurations given below to connect to the APIM Gateway node.

<table style="width:100%">
  <tr >
    <th style="width:20%">Config Section</th>
    <th ></th>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[gateway]</b></code></td>
    <td> <b>Description:</b>This section groups the configuration that are required by the APIM node (which you are currently setting up) to communicate with the gateway node in your deployment. <br/><br/> <b>Default Behavior:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Store Profile</a></li></ul></li></ul> </td>
  </tr>
  <tr >
    <th>Parameters</th>
    <th ></th>
  </tr>
   <tr>
    <td><code><b>gateway_environments</code></b></td>
    <td> <b>Description:</b>The description of gate way environments. <br/><br/> <b>Possible Values:</b> <br/> <ul><li><code><b>dev</code></b></li><li><code><b>test</code></b></li><li><code><b>uat</code></b></li></ul>  <br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Store Profile</li></ul> <br/> <b>Example:</b> <code><b>gateway=["dev"]<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>dev.name</code></b></td>
    <td> <b>Description:</b>The name of the dev environment. <br/><br/> <b>Possible Values:</b> Give a plain text value as the environment name. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Store Profile</li></ul> <br/> <b>Example:</b> <code><b>dev.name="Sandbox"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>dev.type</code></b></td>
    <td> <b>Description:</b>The type of dev environment. <br/><br/> <b>Possible Values:</b> <br/> <ul><li><code><b>hybrid</code></b></li><li><code><b>test2</code></b></li><li><code><b>test2</code></b></li></ul> <br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Store Profile</li></ul> <br/> <b>Example:</b> <code><b>dev.type="hybrid"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>dev.display_in_api_console</code></b></td>
    <td> <b>Description:</b>This parameter specifies whether the gateway environment should display on the API console. <br/><br/> <b>Possible Values:</b> 
    <br/> <ul><li><code><b>true</code></b></li><li><code><b>false</code></b></li></ul> <br/> <b>Default Value:</b> ............. <br/><br/><b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Store Profile</li></ul> <br/> <b>Example:</b> <code><b>dev.display_in_api_console="true"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>dev.description</code></b></td>
    <td> <b>Description:</b>The description of the dev environment. <br/><br/> <b>Possible Values:</b> Give a plain text value as the description. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Store Profile</li></ul> <br/> <b>Example:</b> <code><b>dev.description="Sandbox"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>dev.isDefault</code></b></td>
    <td> <b>Description:</b>The description of the dev environment. <br/><br/> <b>Possible Values:</b> <br/> <ul><li><code><b>true</code></b></li><li><code><b>false</code></b></li></ul> <br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Store Profile</li></ul> <br/> <b>Example:</b> <code><b>dev.isDefault="true"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>dev.management_endpoint</code></b></td>
    <td> <b>Description:</b>The management endpoint of the dev environment. <br/><br/> <b>Possible Values:</b> Use the domain name and port value of your management endpoint to construct the endpoint value. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Store Profile</li></ul> <br/> <b>Example:</b> <code><b>dev.management_endpoint="mgt.gateway:443"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>dev.http_loadbalance_endpoint</code></b></td>
    <td> <b>Description:</b>The http load balancer endpoint of the dev environment. <br/><br/> <b>Possible Values:</b> Use the domain name and port value of your management endpoint to construct the endpoint value. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Store Profile</li></ul> <br/> <b>Example:</b> <code><b>dev.http_loadbalance_endpoint="api.gateway:80"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>dev.https_loadbalance_endpoint</code></b></td>
    <td> <b>Description:</b>The https load balancer endpoint of the dev environment. <br/><br/> <b>Possible Values:</b> Use the domain name and port value of your management endpoint to construct the endpoint value. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Store Profile</li></ul> <br/> <b>Example:</b> <code><b>dev.https_loadbalance_endpoint="api.gateway:443"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>dev.ws_loadbalance_endpoint</code></b></td>
    <td> <b>Description:</b>The load balancer endpoint of the dev environment. <br/><br/> <b>Possible Values:</b> Use the domain name and port value of your management endpoint to construct the endpoint value. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Store Profile</li></ul> <br/> <b>Example:</b> <code><b>dev.ws_loadbalance_endpoint="api.gateway:9091"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>dev.mark_as_revoke_endpoint</code></b></td>
    <td> <b>Description:</b>This parameter specifies whether the endpoint should be marked as revoked. <br/><br/> <b>Possible Values:</b> <br/> <ul><li><code><b>true</code></b></li><li><code><b>false</code></b></li></ul> <br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Store Profile</li></ul> <br/> <b>Example:</b> <code><b>dev.mark_as_revoke_endpoint="true<code><b> <br/> </td>
  </tr>
</table>

# Connecting to the Store

Use the configurations given below to connect to the APIM Store node.

<table style="width:100%">
  <tr >
    <th style="width:20%">Config Section</th>
    <th ></th>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[store]</b></code></td>
    <td> <b>Description:</b>This section groups the configurations that are required by the APIM node (which you are currently setting up) to communicate with the API store in your deployment.. <br/><br/> <b>Default Behavior:</b> ............. <br/><br/><b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Gateway Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a> </li></ul> </td>
  </tr>
  <tr >
    <th>Parameters</th>
    <th ></th>
  </tr>
   <tr>
    <td><code><b>load_balance_endpoiint</code></b></td>
    <td> <b>Description:</b>The load balancer endpoint. <br/><br/> <b>Possible Values:</b> Use the domain name and port value of your management endpoint to construct the endpoint value. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>,#<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Gateway Profile</a></li><li>Optional for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a></li></ul> <br/> <b>Example:</b> <code><b>load_balance_endpoiint="api.store:9443"<code><b> <br/> </td>
  </tr>
</table>

# Connecting to the Analytics Profile

Use the configurations given below to connect to the APIM Analytics node.

<table style="width:100%">
  <tr>
    <th style="width:20%">Config Section</th>
    <th ></th>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[analytics]</b></code></td>
    <td> <b>Description:</b>This section groups the configuration that are required by the APIM node (which you are currently setting up) to communicate with the analytics node in your deployment. <br/><br/> <b>Default Behavior:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Gateway Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Store Profile</a></li></ul> </td>
  </tr>
  <tr >
    <th>Parameters</th>
    <th ></th>
  </tr>
   <tr>
    <td><code><b>nodes</code></b></td>
    <td> <b>Description:</b>The URLs of the analytics node(s) in your deployment. The current APIM node that you are configuring will publish analytics data to the given nodes.... <br/><br/> <b>Possible Values:</b> Give the URL patterns of all the publisher nodes. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Deployment Pattern 1 </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Gateway Profile</a></li><li>Optional for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Deployment Pattern 1 </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Store Profile</a> </li></ul> <br/> <b>Example:</b> <code><b>nodes="{tcp://apimgt.analytics:7611}"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>load_balance_endpoint</code></b></td>
    <td> <b>Description:</b>The URLs of the analytics node(s) in your deployment. The current APIM node that you are configuring will publish analytics data to the given nodes.... <br/><br/> <b>Possible Values:</b> Give the URL patterns of all the publisher nodes. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Deployment Pattern 1 </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Gateway Profile</a></li><li>Optional for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Deployment Pattern 1 </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/config_pattern_one/">Store Profile</a> </li></ul> <br/> <b>Example:</b> <code><b>load_balance_endpoint= "apimgt.analytics:7444"<code><b> <br/> </td>
  </tr>
</table>

# Connecting to the Load Balance/Proxy Server

Use the configurations given below when you are connecting your APIM node to a load balancer/proxy server.

<table style="width:100%">
  <tr >
    <th style="width:20%">Config Section</th>
    <th ></th>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[reverse_proxy_connection]</b></code></td>
    <td> <b>Description:</b> This config header groups the parameters that define the connection from the APIM node to the load balancer.
 <br/><br/> <b>Default Behavior:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>#Deployment Pattern 1, #Gateway Profile</li><li>#Deployment Pattern 2, #Gateway Profile</li></ul> </td>
  </tr>
  <tr > 
    <th style="width:20%">Parameters</th>
    <th ></th>
  </tr>
   <tr>
    <td><code><b>enabled</code></b></td>
    <td> <b>Description:</b> Set the following property to enable load balancing. You can change the value to 'auto' based on the X-Forwarded funciton.
 <br/><br/> <b>Possible Values:</b> .... <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>#Mandatory for #Deployment Pattern 1, #Gateway Profile</li><li>#Mandatory for #Deployment Pattern 2, #Gateway Profile</li></ul> <br/> <b>Example:</b> <code><b>load_balance_endpoiint="api.store:9443"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>host</code></b></td>
    <td> <b>Description:</b> The hostname for the load balancer. If reverse proxy do not have a domain name use IP.
 <br/><br/> <b>Possible Values:</b> Use the domain name and port value of your management endpoint to construct the endpoint value. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>#Mandatory for #Deployment Pattern 1, #Gateway Profile</li><li>#Mandatory for #Deployment Pattern 2, #Gateway Profile</li></ul> <br/> <b>Example:</b> <code><b>load_balance_endpoiint="api.store:9443"<code><b> <br/> </td>
  </tr>
   <tr>
    <td><code><b>context</code></b></td>
    <td> <b>Description:</b> The context name.
 <br/><br/> <b>Possible Values:</b> .... <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>#Mandatory for #Deployment Pattern 1, #Gateway Profile</li><li>#Mandatory for #Deployment Pattern 2, #Gateway Profile</li></ul> <br/> <b>Example:</b> <code><b>load_balance_endpoiint="api.store:9443"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>regContext</code></b></td>
    <td> <b>Description:</b> If a different path is used for registy, uncomment this config. Use only if different path is used for registry.
 <br/><br/> <b>Possible Values:</b>......<br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>#Mandatory for #Deployment Pattern 1, #Gateway Profile</li><li>#Mandatory for #Deployment Pattern 2, #Gateway Profile</li></ul> <br/> <b>Example:</b> <code><b>load_balance_endpoiint="api.store:9443"<code><b> <br/> </td>
  </tr>
</table>

# Configuring the System Administrator

Use the configurations given below to set up the system administrator for the APIM server.

<table style="width:100%">
  <tr >
    <th style="width:20%">Config Section</th>
    <th ></th>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[super_admin]</b></code></td>
    <td> <b>Description:</b> This section groups the configurations that define the administrator credentials. <br/><br/> <b>Default Behavior:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a></li></ul> </td>
  </tr>
  <tr >
    <th>Parameters</th>
    <th ></th>
  </tr>
    <tr>
    <td><code><b>username</code></b></td>
    <td> <b>Description:</b>The user name of the system administrator. This user will have all permissions in the product. <br/><br/> <b>Possible Values:</b> Give a plain text user name. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a> </li></ul> <b>Example:</b> <code><b>username="admin"<code><b> <br/> </td>
  </tr>
  <tr>
    <td><code><b>password</code></b></td>
    <td><b>Description:</b> The user name of the system administrator. This user will have all permissions in the product. <br/><br/> <b>Possible Values:</b> Give a plain text password. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a> </li></ul> <br/> <b>Example:</b> <code><b>password="admin"</code></b> <br/> 
  </tr>
    <tr>
    <td><code><b>create_super_admin</code></b></td>
    <td> <b>Description:</b> This parameter specifies whether the administrator credentials should be created when the server starts the first time. <br/><br/><b>Possible Values:</b> <br/> <ul><li><code><b>true</code></b></li><li><code><b>false</code></b></li></ul> <br/> <b>Default Value:</b> ............. <br/><br/><b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Profiles</a></li></ul> <b>Example:</b> <code><b>create_super_admin="true"</code></b> <br/> </td>
  </tr>
</table>

# Configuring example

Use the configurations given below to set up the JWT feature in APIM.

<table style="width:100%">
  <tr >
    <th style="width:20%">Config Section</th>
    <th ></th>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[Config_Section]</b></code></td>
    <td> <b>Description:</b>......... <br/><br/> <b>Default Behavior:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>#Deployment Pattern 1, #Gateway Profile</li><li>#Deployment Pattern 2, #Gateway Profile</li></ul> </td>
  </tr>
  <tr > 
    <th style="width:20%">Parameters</th>
    <th ></th>
  </tr>
   <tr>
    <td><code><b>parameter</code></b></td>
    <td> <b>Description:</b>........ <br/><br/> <b>Possible Values:</b> ........ <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>#Mandatory for #Deployment Pattern 1, #Gateway Profile</li><li>#Mandatory for #Deployment Pattern 2, #Gateway Profile</li></ul> <br/> <b>Example:</b> <code><b>load_balance_endpoiint="api.store:9443"<code><b> <br/> </td>
  </tr>
</table>

# Configuring JWT

Use the configurations given below to set up the JWT feature in APIM.

<table style="width:100%">
  <tr >
    <th style="width:20%">Config Section</th>
    <th ></th>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[sampe_JWT]</b></code></td>
    <td> <b>Description:</b>......... <br/><br/> <b>Default Behavior:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>#Deployment Pattern 1, #Gateway Profile</li><li>#Deployment Pattern 2, #Gateway Profile</li></ul> </td>
  </tr>
  <tr > 
    <th style="width:20%">Parameters</th>
    <th ></th>
  </tr>
   <tr>
    <td><code><b>sample_JWT_parameter</code></b></td>
    <td> <b>Description:</b>........ <br/><br/> <b>Possible Values:</b> Use the domain name and port value of your management endpoint to construct the endpoint value. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>#Mandatory for #Deployment Pattern 1, #Gateway Profile</li><li>#Mandatory for #Deployment Pattern 2, #Gateway Profile</li></ul> <br/> <b>Example:</b> <code><b>load_balance_endpoiint="api.store:9443"<code><b> <br/> </td>
  </tr>
</table>

# Advanced Configurations

## Example Section 1

Add the following section and parameters to configure the Example 1.

<table style="width:100%">
  <tr >
    <th style="width:20%">Config Section</th>
    <th ></th>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[advanced_section]</b></code></td>
    <td> <b>Description:</b>........ <br/><br/> <b>Default Behavior:</b> ............. <br/><br/><b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Gateway Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a> </li></ul> </td>
  </tr>
  <tr >
    <th>Parameters</th>
    <th ></th>
  </tr>
   <tr>
    <td><code><b>advanced_section_param</code></b></td>
    <td> <b>Description:</b>......... <br/><br/> <b>Possible Values:</b> Use the domain name and port value of your management endpoint to construct the endpoint value. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>,#<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Gateway Profile</a></li><li>Optional for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a></li></ul> <br/> <b>Example:</b> <code><b>load_balance_endpoiint="api.store:9443"<code><b> <br/> </td>
  </tr>
</table>

## Example Section 2

Add the following section and parameters to configure the Example 2.

<table style="width:100%">
  <tr >
    <th style="width:20%">Config Section</th>
    <th ></th>
  </tr>
  <tr>
    <td style="width:20%"><code><b>[advanced_section]</b></code></td>
    <td> <b>Description:</b>........ <br/><br/> <b>Default Behavior:</b> ............. <br/><br/><b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Gateway Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Traffic Manager Profile</a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Key Manager Profile</a> </li></ul> </td>
  </tr>
  <tr >
    <th>Parameters</th>
    <th ></th>
  </tr>
   <tr>
    <td><code><b>advanced_section_param</code></b></td>
    <td> <b>Description:</b>......... <br/><br/> <b>Possible Values:</b> Use the domain name and port value of your management endpoint to construct the endpoint value. <br/><br/> <b>Default Value:</b> ............. <br/><br/> <b>Mandatory/Optional:</b> <br/> <ul><li>Mandatory for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>,#<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Gateway Profile</a></li><li>Optional for #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">All Deployment Patterns </a>, #<a href="https://apimconfigurationcatalog.netlify.com/deploy/deployment_patterns/">Publisher Profile</a></li></ul> <br/> <b>Example:</b> <code><b>load_balance_endpoiint="api.store:9443"<code><b> <br/> </td>
  </tr>
</table>
