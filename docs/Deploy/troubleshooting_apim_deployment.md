# Troubleshooting APIM Deployment

............

## Starting the Key Manager

<ul>
	<li>You may notice the following error messages when starting up the server. This occurs because some API Manager directories are not available in the Identity Server. These are not critical errors, so they can be ignored. Alternatively, you can create the listed directories in the Identity Server pack. </br>
	<code>ERROR {org.wso2.carbon.apimgt.impl.utils.APIUtil} - Custom sequence template location unavailable for custom sequence type in : repository/resources/customsequences/in </br>
 	ERROR {org.wso2.carbon.apimgt.impl.utils.APIUtil} - Custom sequence template location unavailable for custom sequence type out : repository/resources/customsequences/out </br>
 	ERROR {org.wso2.carbon.apimgt.impl.utils.APIUtil} - Custom sequence template location unavailable for custom sequence type fault : repository/resources/customsequences/fault</code>
	</li>
	<li>If you have configured the hostnames for WSO2 API Manager and WSO2 Identity Server in the server start up, you will see the following warning in the WSO2 API Manager backend logs. </br>
	<code>WARN {org.wso2.carbon.apimgt.gateway.throttling.util.BlockingConditionRetriever} -  Failed retrieving Blocking Conditions from remote endpoint: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target. Retrying after 15 seconds... {org.wso2.carbon.apimgt.gateway.throttling.util.BlockingConditionRetriever}</code> </br></br>
	The reason for this is that the default certificates that come with WSO2 Servers are created for localhost. Therefore, when WSO2 API Manager boots up, it makes an HTTP call to a webapp that is in the Key Manager (throttle data at KM_URL/throttle/data/v1/keyTemplates). Thereafter, WSO2 API Manager decides the URL of the Key Manager base on the URL that is configured in the api-manager.xml, which is localhost.</br>
	To overcome this issue, you need to create self-signed certificates for WSO2 API-M and WSO2 IS host names. Then export the public certs of WSO2 API-M to the trust-store.jks of WSO2 IS and vice versa. This should resolve the SSL handshake failure
	</li>
</ul>