# Introduction to APIM Deployment

WSO2 API Manager includes five main components as the Publisher, Store, Gateway, Traffic Manager and Key Manager. In a typical production environment, you set up the different WSO2 API Manager (WSO2 API-M) components (API Publisher, Store, Gateway, Key Manager, and Traffic Manager) in separate servers so that you can scale them independently. Installing and configuring each or selected component/s in different servers is called a distributed setup. You also install multiple instances of a component in a cluster to ensure proper load balancing. When one node becomes unavailable or is experiencing high traffic, another node handles the requests. In a stand-alone APIM setup, these components are deployed in a single server. However, in a typical production setup, they need to be deployed in separate servers for better performance. 

## Main components of APIM

The following diagram illustrates the main components of an API Manager distributed deployment.

<a href=""><img src="../../images/DDComponents.png"></a>

Let's take a look at each component in the above diagram. For more information on these components, see Architecture.

<table>
	<tr>
		<th>Component</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>API Gateway</td>
		<td>This component is responsible for securing, protecting, managing, and scaling API calls. The API gateway is a simple API proxy that intercepts API requests and applies policies such as throttling and security checks. It is also instrumental in gathering API usage statistics. We use a set of handlers for security validation and throttling purposes in the API Gateway. Upon validation, it will pass Web service calls to the actual back end. If it is a token request call, it will then directly pass the call to the Key Manager Server to handle it.</td>
	</tr>
	<tr>
		<td>API Store</td>
		<td>This component provides a space for consumers to self-register, discover API functionality, subscribe to APIs, evaluate them, and interact with API publishers. Users can view existing APIs and create their own application by bundling multiple APIs together into one application.</td>
	</tr>
	<tr>
		<td>API Publisher</td>
		<td>This component enables API providers to easily publish their APIs, share documentation, provision API keys, and gather feedback on API features, quality, and usage. You can create new APIs by pointing to the actual back end service and also define rate-limiting policies available for the API.</td>
	</tr>
	<tr>
		<td>API Key Manager Server</td>
		<td>This component is responsible for all security and key-related operations. When an API call is sent to the Gateway, it calls the Key Manager server and verifies the validity of the token provided with the API call. If the Gateway gets a call requesting a fresh access token, it forwards the username, password, consumer key, and consumer secret key obtained when originally subscribing to the API to the Key Manager. All tokens used for validation are based on OAuth 2.0.0 protocol. All secure authorization of APIs is provided using the OAuth 2.0 standard for Key Management. The API Gateway supports API authentication with OAuth 2.0, and it enables IT organizations to enforce rate limits and throttling policies for APIs by consumer.</br>
		Login/Token API in the Gateway node should point to the token endpoint of Key Manager node. The token endpoint of the Key Manager node is at https://localhost:9443/oauth2endpoints/token. In a distributed setup, it should be https://<IP of key manager node>:<port-with-offset-of-keymanager-node>/oauth2endpoints/token.
        <p style="background-color: #C2F9DE;padding: 10px">In a clustered environment, you use session affinity to ensure that requests from the same client always get routed to the same server. </br> Session affinity is not mandatory when configuring a Key Manager cluster with a load balancer. However, authentication via session ID fails when session affinity is disabled in the load balancer. <br/>The Key Manager first tries to authenticate the request via the session ID. If it fails, the Key Manager tries to authenticate via basic authentication.</p>
		</td>
	</tr>
	<tr>
		<td>API Traffic Manager</td>
		<td>The Traffic Manager helps users to regulate API traffic, make APIs and applications available to consumers at different service levels, and secure APIs against security attacks. The Traffic Manager features a dynamic throttling engine to process throttling policies in real-time, including rate limiting of API requests.</td>
	</tr>
	<tr>
		<td>LB (load balancers)</td>
		<td>The distributed deployment setup depicted above requires two load balancers. We set up the first load balancer, which is  an instance of NGINX Plus, internally to manage the cluster. The second load balancer is set up externally to handle the requests sent to the clustered server nodes, and to provide failover and autoscaling. As the second load balancer, you can use an instance of NGINX Plus or any other third-party product.</td>
	</tr>
	<tr>
		<td>RDBMS (shared databases)</td>
		<td>The distributed deployment setup depicted above shares the following databases among the APIM components set up in separate server nodes.</br>
			<ul>
				<li><b>User Manager Database</b>: Stores information related to users and user roles. This information is shared among the Key Manager Server, Store, and Publisher. Users can access the Publisher for API creation and the Store for consuming the APIs.</li>
				<li><b>API Manager Database</b>: Stores information related to the APIs along with the API subscription details. The Key Manager Server uses this database to store user access tokens required for verification of API calls.</li>
			</ul>
		</td>
	</tr>
</table>

## Databases used by APIM components

Additionally, API Manager uses the following databases, which are shared among the server nodes.

* **User Manager database** - Stores information related to users and user roles. This information is shared among the Key Manager Server, Store, and Publisher. Users can access the Publisher for API creation and the Store for consuming the APIs. The User Manager database is also referred to as WSO2UM_DB and userdb.
* **API Manager database** - Stores information related to the APIs along with the API subscription details. The Key Manager Server uses this database to store user access tokens that are used for verification of API calls. The API Manager database is also referred to as WSO2_AM_DB and apimgtdb.
* **Registry database** - Shares information between the Publisher and Store. When an API is published through the Publisher, it is made available in the Store via the shared registry database. Although you would normally share information between the Publisher and Store components only, if you are planning to create this setup for a multi-tenanted environment (create and work with tenants), it is required to share the information in this database between the Gateway and Key Manager components as well. The Registry database is also referred to as WSO2REG_DB and regdb.
* **Statistics database** - Stores information related to API statistics. After you configure API-M analytics, it writes summarized data to this database. The Publisher and Store can then query this database to display the statistics data. The Statistics database is also referred to as WSO2_STAT_DB and statdb.
* **Message Broker database** - Traffic Manager uses this database as the message store for broker when advanced throttling is used. The Message Broker DB is also referred to as WSO2_MB_STORE_DB and  mbstoredb.

WSO2 API Manager components use the databases as follows:
...........

## Message flows    
The three main use cases of API Manager are API publishing, subscribing and invoking. Described below is how the message flow happens in these use cases. 

* **Publishing APIs**

    A user assigned to the publisher role can publish APIs. This is done via the Publisher server. When an API is published in the API Publisher, it will be available in the API Store. Furthermore, the API Gateway must be updated with this API so that users can invoke it. As we are using a clustered Gateway, all Gateway server nodes in the cluster are updated with the published API details, enabling any of these Gateway nodes to serve API calls that are received.  When an API is published, it is also pushed to the registry database, so that it can be made available on the store via the shared database. 

* **Subscribing to APIs**

    A user with the subscriber role logs into the API Store and subscribes to an API. The user must then generate an access token to be able to invoke the API. When the subscriber requests to generate the token, a request is sent to the Key Manager Server cluster. The token is then generated, and the access token details are displayed to the subscriber via the Store.

* **Invoking APIs**

    Subscribed users can invoke an API to which they have subscribed. When the API is invoked, the request is sent to the API Gateway server cluster. The Gateway server then forwards the request to the Key Manager server cluster for verification. Once the request is verified, the Gateway connects to the back-end implementation and obtains the response, which is sent back to the subscriber via the Gateway server.

The diagram below summarizes these message flows where the Publisher is referred to as the publishing portal and the Store is referred to as the developer portal.

<a href=""><img src="../../images/APIMDeployment.png"></a>
