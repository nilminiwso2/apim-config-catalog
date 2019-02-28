Listed below are the deployment patterns you can use with your API Manager.
# 

# Pattern 1: Single node (all-in-one) deployment

In a typical production deployment, API Manager is deployed as components (Publisher, Store, Gateway, Key Manager and Traffic Manager). While this provides very high performance and a high level of scalability, it may be too complex if you want to run API Manager as a small to medium scale API Management solution. A WSO2 API-M single node deployment, which has all the API-M components in one instance, would be simple to set up and requires less resources when compared with a distributed deployment. It is ideal for any organization that wants to start small and iteratively build up a robust API Management Platform.

WSO2 provides two options for organizations that are interested in setting up a small to medium scale API Management solution.

* Setting up on WSO2 API Cloud, which is a subscription based API Management solution. You can access this service by creating an account in WSO2 API Cloud.
* If you are interested in setting up a single node API Manager instance, which has all the API-M components in one instance,  on-premise, you can download the latest version of API Manager and follow the instructions.

## Single node without HA

<table>
	<tr>
		<th>Pros</th>
		<th>Cons</th>
	</tr>
	<tr>
		<td>
			<ul>
				<li>Production support is required only for a single API Manager node (you receive 24*7 WSO2 production support).</li>
				<li>Deployment is up and running within hours.</li>
				<li>Can handle up to 43 million API calls a day (up to 500 API calls a second)</li>
				<li>Minimum hardware/cloud infrastructure requirements (only one node).</li>
				<li>Suitable for anyone new to API Management.</li>
			</ul>
		</td>
		<td>
			<li>Deployment does not provide High Availability.</li>
			<li>Not network friendly. Deploying on a demilitarized zone (DMZ) would require a Reverse Proxy.</li>
		</td>
	</tr>
</table>

<a href=""><img src="../../images/P-1_NoHA.png"></a>
## Single node with HA (Active-Active)

<table>
	<tr>
		<th>Pros</th>
		<th>Cons</th>
	</tr>
	<tr>
		<td>
			<li>The system is highly available.</li>
			<li>Production support is required for 2 API Manager nodes (you receive 24*7 WSO2 production support).</li>
			<li>Can handle up to 86 million API calls a day (up to 1000 API calls a second).</li>
			<li>Deployment is up and running within hours.</li>
		</td>
		<td>
			<li>Not network friendly.</li>
			<li>Deploying on a DMZ would require a Reverse Proxy.</li>
		</td>
	</tr>
</table>

<a href=""><img src="../../images/P-1_HA.png"></a>

# Pattern 2: Separate Gateway and separate Key Manager

<a href=""><img src="../../images/P-2.png"></a>

# Pattern 3: Fully distributed setup

<a href=""><img src="../../images/P-3.png"></a>

# Pattern 4: Internal and external (on-premise) API Management

<a href=""><img src="../../images/P-4.png"></a>

# Pattern 5: Internal and external (public and private cloud) API Management

<a href=""><img src="../../images/P-5.png"></a>

# Traffic Manager scalable deployment patterns