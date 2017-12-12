# PKI-Client-Mutual-Auth
Sample to create PKI client authentication with validation and RBAC authorization with mutual authentication enforcement. This document shows an example of the setup and testing of this policy.

## Prerequisites

To follow this example you will need the following:

- A client public/private key pair with a single depth level.
- The issuer public certificate.
- A directory to lookup customer attributes.
- The policy export file that is part of this repository.
- Basic working knowledge of the Axway API Gateway software and Policy Studio.

A sample single depth client-issuer relationship can be seen here:
![alt text](https://github.com/Axway-API-Management-Plus/PKI_Client_Mutual_Auth/blob/master/example/src/certificateChain.png "Certificate and Issuer")

## Step By Step

1. In Policy Studio, navigate to 'Environment Configuration --> Certificates and Keys --> Certificates'. Select the 'CA' tab and choose the 'Create/Import' button in the bottom right of the interface. Click "Use Subject". Click "Ok".
![alt text](https://github.com/Axway-API-Management-Plus/PKI_Client_Mutual_Auth/blob/master/example/src/cacertImport.png "Configure CA Trust")

2. Navigate to 'Environment Configuration --> Listeners --> API Gateway --> Default Services --> Ports'. Click 'Add --> HTTPS Interface'. Fill out information on the Network tab and select a CA certificate. On the Mutual Auth tab choose to 'Require Client Certificates' with a depth of "1", then search for and select the issuer CA. This will allow the API Gateway to provide a CA list during the SSL handshake to give clients browsers/applications of trusted CAs, require submission of a client certificate on calls to protected resources on the associated port, and enforce issuance of the submitted certificate only to selected trusted CAs.
![alt text](https://github.com/Axway-API-Management-Plus/PKI_Client_Mutual_Auth/blob/master/example/src/mutualAuth.png "Configure Mutual Auth")

3. Navigate to 'File --> Import --> Import Customization Fragment'. Select the policy fragment supplied in this repository. Resolve any dependency issues and complete import.
![alt text](https://github.com/Axway-API-Management-Plus/PKI_Client_Mutual_Auth/blob/master/example/src/importFrag.png "Import Policy Fragment")

4. Modify the policy to meet your environmental configuration as defined in this repo's readme. For my testing, I disabled the OCSP validation policy as my implementation did not allow external connections to an OCSP server for this example.

5. Create a new policy with a response message (Set Message Filter and Reflect Message Filter). Drop a policy shortcut filter into the policy and choose the newly imported policy fragment. Set the shortcut as the starting filter. Create relative path.
![alt text](https://github.com/Axway-API-Management-Plus/PKI_Client_Mutual_Auth/blob/master/example/src/sampleEchoPolicy.png "Sample Echo Policy")

6. Deploy.

7. Ensure your client certificate is available to your test client and execute a request against your new service. Select the appropriate client key.
![alt text](https://github.com/Axway-API-Management-Plus/PKI_Client_Mutual_Auth/blob/master/example/src/response.png "Example Request/Response")

8. Post response, log into the API Gateway Manager, navigate to the Traffic Monitor, and confirm policy execution with use of your imported policy.
![alt text](https://github.com/Axway-API-Management-Plus/PKI_Client_Mutual_Auth/blob/master/example/src/trafficMonitor.png "Traffic Monitor")

## API Management Version Compatibility
This artefact was successfully tested for the following versions:
- V7.5.3

## Contributing

Please read [Contributing.md](https://github.com/Axway-API-Management/Common/blob/master/Contributing.md) for details on our code of conduct, and the process for submitting pull requests to us.


## Team

![alt text][Axwaylogo] Axway Team

[Axwaylogo]: https://github.com/Axway-API-Management/Common/blob/master/img/AxwayLogoSmall.png  "Axway logo"


## License
[Apache License 2.0](/LICENSE)
