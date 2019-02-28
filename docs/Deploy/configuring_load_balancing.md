A load balancer or reverse proxy is required to map external traffic with ports and URLs that WSO2 API Manager (WSO2 API-M) uses internally. Follow the instructions below to configure load balancing together with reverse proxying.
#

# Creating SSL certificates for the load balancer

<p style="background-color: #C2F9DE;padding: 10px">This step is only applicable for a High Availablity (HA) setup where multiple nodes are fronted by a load balancer.
</p>

<ol>
	<li>Create the Server Key. </br>
		<code>sudo openssl genrsa -des3 -out <key_name>.key 1024</code>
	</li>
	<li>Submit the certificate signing request (CSR).</br>
		<code>sudo openssl req -new -key <key_name>.key -out server.csr</code>
	</li>
	<li>Remove the password. </br>
		<code>
			sudo cp <key_name>.key <key_name>.key.org </br> sudo openssl rsa -in <key_name>.key.org -out <key_name>.key</code>
	</li>
	<li>Sign your SSL Certificate.</br>
		<code>sudo openssl x509 -req -days 365 -in server.csr -signkey <key_name>.key -out <certificate_name>.crt</code>
	</li>
	<li>Copy the key and certificate files that you generated in the above step to the /etc/nginx/ssl/ location.</li>
</ol>

# Configuring the load balancer

<p style="background-color: #C2F9DE;padding: 10px">In the following instructions, you are instructed to use <b>NGINX</b> to handle the load balancing requirements. However, note thatyou can use any load balancer that you prefer.
</p>

Follow the steps given below.

<ol>
	<li>Install NGINX in a server that is configured in your deployment cluster. For more information on installing NGINX, see NGINX community version and NGINX Plus. </br>
	Note that the NGINX version that you need to install varies based on the WSO2 APIM components that the load balancer is fronting:
	<table>
		<tr>
			<th>Deployment</th>
			<th>APIM Nodes</th>
			<th>LB</th>
			<th>Reason</th>
		</tr>
		<tr>
			<td>Single all-in-one deployment</td>
			<td>N/A</td>
			<td>NGINX Community</td>
			<td>This deployment does not need Sticky Sessions (Session Affinity).</td>
		</tr>
		<tr>
			<td>Active-active deployment using single all-in-one nodes</td>
			<td>N/A</td>
			<td>NGINX Plus</td>
			<td>This deployment requires Sticky Sessions, but NGINX Community version does not support it. You can use ip_hash as the sticky algorithm.</td>
		</tr>
		<tr>
			<td>Distributed deployment</td>
			<td>Gateway with a single Gateway Manager</td>
			<td>NGINX Community version</td>
			<td>The Gateway node in this deployment does not need Sticky Sessions.</td>
		</tr>
		<tr>
			<td></td>
			<td>Gateway with multiple Gateway Managers</td>
			<td>NGINX Plus</td>
			<td>The Gateway Manager nodes require Sticky Sessions, but NGINX Community version does not support it. You can use ip_hash as the sticky algorithm. Sticky Sessions are needed for port 9443 in the Gateway, and not needed for the pass through ports in the Gateway (8243, 8280).</td>
		</tr>
		<tr>
			<td></td>
			<td>Store, Publisher, and Key Manager</td>
			<td>NGINX Plus</td>
			<td>Requires Sticky Sessions, but NGINX Community version does not support it. You can use ip_hash as the sticky algorithm.</td>
		</tr>
	</table>
	</li>
	<li>Configure NGINX to direct the HTTP and HTTPs requests based on your deployment.
		<ol>
			<li>Run the following command to identify the exact location of the <NGINX_HOME> directory. Inspect the output and identify the --prefix tag as it provides the location of the <NGINX_HOME> directory. </br>
            <code>nginx -V</code>
			</li>
			<li>
				Update the ngnix.conf file with the required NGINX configuration given below. If not, you can create a file with the .conf suffix and copy it to the <NGINX_HOME>/conf.d directory.
			</li>
		</ol>
	</li>
</ol>

## NGINX for a Single Node deployment
.................

## NGINX for an Active-Active deployment
.................

## NGINX for a deployment with HA for the Gateway
.................

## NGINX for a deployment with HA for the Publisher
.................

## NGINX for a deployment with HA for the API Store
.................

## NGINX for a deployment with HA for the Key Manager
.................

# Configuring the APIM nodes

Once you have set up the load balancer as require for the APIM deployment pattern that you are working with, see the instructions on how to configure the APIM node(s) in the deployment.

* [Configuring a Single Node Deployment](single_node_deployment.md)
* [Configuring an Active-Active Deployment](active_active_deployment.md)
* [Configuring a Fully Distributed Deployment](fully_distributed_deployment.md)