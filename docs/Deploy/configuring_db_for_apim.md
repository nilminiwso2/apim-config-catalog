# Configuring Databases for API Manager

WSO2 API Manager is shipped with an H2 database. This embedded H2 database is suitable for development and testing environments. However, for production environments, it is recommended to use an industry-standard RDBMS such as Oracle, PostgreSQL, MySQL, MS SQL, etc. The following steps describe how to download and install an RDBMS (which in this case is a MySQL Server), create the databases, and configure the API Manager servers to connect to them. 

<p style="background-color: #C2F9DE;padding: 10px">
	Although the following section instructs you to use a MySQL Server,  you can use any RDBMS in your deployment based on your preference.
</p>

## Setting up MySQL

<ol>
	<li>Unzip the WSO2 API Manager pack. Let's call it <API-M_HOME>.</li>
	<li>Download and install MySQL Server.</li>
	<li>Download the MySQL JDBC driver.</li>
	<li>Unzip the downloaded MySQL driver archive, and copy the MySQL JDBC driver JAR (mysql-connector-java-x.x.xx-bin.jar) into the <API-M_HOME>/repository/components/lib directory in all the nodes in the cluster.</li>
	<li><b>Do this step only if your database is not on your local machine and on a separate server.</b>
	Define the hostname for configuring permissions for the new database by opening the /etc/hosts file and adding the following: </br>
	<code><MYSQL-DB-SERVER-IP> carbondb.mysql-wso2.com</code>
	</li>
	<li>Install mysql-client in each of the API-M servers in which WSO2 API-M is deployed. You need to do this in order to check if the servers can access the MySQL database. </br>
	<code>sudo apt install mysql-client <br> mysql -h <mysqldb_host_ip> -u username -p</code>	
	</li>
	<li>Enter the following command in a command prompt, where username is the username that you used to access the databases. </br>
	<code>mysql -u username -p</code>
	</li>
	<li>When prompted, specify the password that will be used to access the databases with the username you specified.
	</li>
</ol>

## Creating the databases

Create the databases using the following commands, where <API-M_HOME> is the path to any of the API Manager instances you installed, and username and password are the same as those you specified in the previous steps.

<div style="background-color: #eee">
		<code>
			mysql> create database apimgtdb; </br>
			mysql> use apimgtdb;</br>
			mysql> source <API-M_HOME>/dbscripts/apimgt/mysql.sql;</br>
			mysql> grant all on apimgtdb.* TO '<username>'@'%' identified by '<password>';</br>	 
			mysql> create database userdb;</br>
			mysql> use userdb;</br>
			mysql> source <API-M_HOME>/dbscripts/mysql.sql;</br>
			mysql> grant all on userdb.* TO '<username>'@'%' identified by '<password>';
		</code>
</div>

## Connecting the databases to the APIM node(s)

Depending on the deployment pattern you are setting up, you may have one or several APIM nodes installed in your deployment. Follow the instructions for connecting to the databases from your deployment pattern:

* [Configuring a Single Node Deployment](single_node_deployment.md)
* [Configuring an Active-Active Deployment](active_active_deployment.md)
* [Configuring a Fully Distributed Deployment](fully_distributed_deployment.md)