# PKI-Client-Mutual-Auth
Sample to create PKI client authentication with validation and RBAC authorization with mutual authentication enforcement.

An Example Policy Use Case Deployment and Execution With Step-by-Step can be found here:
https://github.com/Axway-API-Management-Plus/PKI_Client_Mutual_Auth/blob/master/example/configureAndExecute.md

## Description

This policy is meant to serve as a template for customers looking to enforce certificate authentication as a login or access mechanism. This is a very basic policy to provide a framework for this process, and can be extended to provide more in depth fine grain authorization controls. 

This policy will only pass if mutual authentication is configured in the API Gateway, allowing a client certificate can be presented. This requires the API Gateway HTTPS Listener to Allow or Require Client Certificates. More information on this configuration can be found under the "Configure Mutual Authentication Settings" section of the "Configure HTTP Services" topic on the Axway public documentation site (https://docs.axway.com/bundle/APIGateway_753_PolicyDevGuide_allOS_en_HTML5/page/Content/PolicyDevTopics/general_services.htm).

The basic policy flow is as follows:

- SSL Filter: This filter, renamed "Require SSL/TLS with Client Certificate" to more accurately represent the functionality it provides, checks to ensure that a client certificate was presented as part of the SSL handshake when connecting to the HTTPS listener port, and extracts it for further processing. The certificate is stored as an attribute, which by default will be "${certificate}". 
- Extract Certificate Attributes Filter: This filter, renamed "Extract Attributes From Client Certificate", parses the client certificate to make a large number of certificate attributes available for processing. The created attributes can be easily viewed by right clicking this filter and selecting the "Show All Attributes" option in the policy configuration window.
- Compare Attribute Filter: This filter, renamed "Verify Issuer ID", compares an attribute capture from the Extract Certificate Attributes Filter against an accepted value. This filter could be used to effect contextual logic based on any captured attributes. For example, as shown in this policy you might choose to validate internal certificates via OCSP as internal CAs are registered in trust stores with precomputed signed responses, but upon failure you could execute separate logic, such as perhaps doing dynamic path discovery and validation via SCVP to the Federal Common Policy certificate where intermediate issuers are not trusted.
- Certificate Attributes Filter: This filter, renamed "Enforce Certificate Attribute Based Authorization", allows you to enforce a digital policy that provides entitlement management via Attribute Based Access Control (ABAC). This is enabled by comparing the certificate attributes that construct the client certificate DN (eg organization [O], location [L]) against accepted values.
- OCSP Client Filter: This filter, renamed "Validate Certificate with OCSP", will perform OCSP revocation checking on the client certificate. At minimum, this filter will need to be updated to reflect your environments OCSP Responder URL, but request signing and caching should be considered. Depending on your organization, other PKI validation options such as CRL, SCVP, etc. may be used instead of OCSP based validation. Certificate revocation checking is an imperative part of secure certificate authentication. More information on the value of PKI validation can be found here: http://danielwille.blogspot.com/2016/04/us-federal-pki-part-i-getting-started.html
- LDAP RBAC Filter: This filter, renamed "Perform RBAC Check Against ADFS" to illustrate its potential use with MS Active Directory, does a repository lookup to ensure the client certificate maps to a role or group as defined in the search base. Additional attribute based authentication can be configured as well to add further fine grain authorization. To use this filter, you will need to set up access to a directory in Policy Studio under 'External Connections --> LDAP Connections' and an associated repository under 'External Connections --> LDAP Repositories'. Once your repository is configured, you will need to modify the filter to update the Connection, Search Base, and Filter, as well as update or remove the attribute checks.

Once updated to fit your environment, this policy can be used in several ways:
- As a standalone authentication, authorization, and validation service for PKI by assigning it a relative path in Policy Studio where it can be reached.
- It can be called as a policy shortcut as part of other policies created in Policy Studio.
- It can be registered as a REST API via either the API Gateway Rest API Repository  configuration in Policy Studio or by creating a new API in API Manager.
- It can be registered as an Inbound Security Policy for use within API Manager via 'Server Settings --> API Manager --> Inbound Security Policies' in Policy Studio.

## API Management Version ## API Management Version Compatibility
This artefact was successfully tested for the following versions:
- V7.5.3


## Install

```
• Download the CertificateAuthSample*.xml policy file.
• Import file. The simplest way however is to open Policy Studio and select 'File --> Import --> Import Configuration Fragment'.
```

## Usage

```
• Create or configure an HTTPS Listener with Mutual Authentication configured to accept client certificates.
• Update policy relative to your environment.
• Configure policy for your environment.
• Call resource and authenticate with your certificate.
```

## Bug and Caveats

```
N/A
```

## Contributing

Please read [Contributing.md](https://github.com/Axway-API-Management/Common/blob/master/Contributing.md) for details on our code of conduct, and the process for submitting pull requests to us.


## Team

![alt text][Axwaylogo] Axway Team

[Axwaylogo]: https://github.com/Axway-API-Management/Common/blob/master/img/AxwayLogoSmall.png  "Axway logo"


## License
[Apache License 2.0](/LICENSE)
