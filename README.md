# Using Azure AD as an identity provider for OpenSearch Dashboards in Amazon OpenSearch Service

by AWS Editorial Team | on 18 NOV 2021 | in Amazon OpenSearch Service, Analytics, Security | [Permalink](https://aws.amazon.com/es/blogs/aws-spanish/uso-de-azure-ad-como-proveedor-de-identidad-para-opensearch-dashboards-en-amazon-opensearch-service/) |  [Share](https://aws.amazon.com/es/blogs/aws-spanish/uso-de-azure-ad-como-proveedor-de-identidad-para-opensearch-dashboards-en-amazon-opensearch-service/#)

By Luciano Bernardes, Senior Partner Solution Architect at AWS
Caio Cesar, Microsoft Specialist on AWS
Rafael Gumiero, Senior Solution Architect for Analytics at AWS
Robert da Costa, Senior Solution Architect at AWS

### What are we going to discuss in this blog?

Let's address the integration of OpenSearch Dashboards with an identity provider, using the SAML 2.0 standard, in a laboratory environment. In corporate environments, it is common for companies to already have an identity provider to centralize the authentication and authorization of their applications/service providers, with the objective of security and operability.

### Amazon OpenSearch Service

Amazon OpenSearch Service makes it easy to perform interactive log analysis, real-time application monitoring, website searching, and more. OpenSearch is an open source distributed research and analysis suite derived from Elasticsearch. Amazon OpenSearch Service offers the latest versions of OpenSearch, support for 19 versions of Elasticsearch (versions 1.5 to 7.10), and preview features provided by OpenSearch Dashboards and Kibana (versions 1.5 to 7.10).


### Identity Service Provider (IdP) and Service Provider (SP)

An identity provider (IdP) is a federation partner that evaluates and confirms the identity of a user and can use data exchange patterns, such as SAML 2.0. In this way, the IdP authenticates the user and provides an authentication token to the service provider. In the case of this article, the IdP in question is Azure AD.

And the service provider (SP) itself is a partner of the federation, which provides the services to the users. The SP depends on the IdP to prove the identity of the user trying to access its resources. In the case of this article, the SP in question is OpenSearch Dashboards.

### OpenSearch Dashboards and SAML 2.0

SAML authentication for OpenSearch Dashboards allows you to use your existing identity provider to provide single sign-on (SSO) on Amazon OpenSearch Service domains running OpenSearch or Elasticsearch 6.7 or later. To use SAML authentication, we must enable detailed access control. You can't enable detailed access control on existing domains, only on new domains. By extension, this means that you can only enable SAML authentication on new domains or existing domains that already have detailed access control enabled.


### Requirements

This blog describes the tasks to configure the integration of OpenSearch Dashboards with Azure AD, so the following points have already been provisioned in our laboratory:

Amazon OpenSearch Service domain (note mentioned in OpenSearch Dashboards and SAML 2.0)
Detailed Access Control (FGAC) enabled
Azure Active Directory directory

## Tasks to perform the integration

En el dominio de Amazon OpenSearch Service, activamos el estándar SAML 2.0. En el dominio ya creado, fuimos a la pestaña «Configuración de seguridad» y hacemos clic en «Editar»:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-2.jpg)


In the “SAML Authentication for OpenSearch Dashboards/Kibana” area, check the “Enable SAML Authentication” option and take note of the three URLs indicated in the “Configure Identity Provider (IdP)” area:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-3.jpg)


On the Azure AD side of our lab, we created a custom business application. Therefore, on the IdP page we click on “Business Applications” and then on “All Applications\ New Application”:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-4.jpg)

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-5.jpg)

In the gallery with the available applications, click on “Create your own application”:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-5.png)

On the open slide on the right side, fill the application with an easily identifiable name, check the option “Integrate any other application that you don't find in the gallery (not in the gallery)” and click “Create”:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-6.png)

Once the application was created, it was necessary to make some permission settings for users and/or groups and recognize OpenSearch Dashboards as a service provider. So, we opened the application and went to “Users and Groups\ Add User/Group”:

Note: For groups to be used in this integration, evaluate the features of Azure AD Premium.

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-7.png)

On the next screen, we selected users and groups who should have access to the application and, consequently, could perform single sign-on for OpenSearch Dashboards. Then we click on “Assign”:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-8.png)

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-9.png)

Once the user and/or group was assigned, we were able to see the available application by logging in with them at https://myapplications.microsoft.com:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-8.jpg)

Returning to the configuration of the application in Azure AD, it was necessary to make adjustments to the SAML 2.0 standard, which allowed integration with the OpenSearch panels. So, we went to “Single Sign-On” and clicked “SAML”:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-11.png)

On the next screen, we click “Edit” in the “SAML Basic Configuration” section:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-12.png)

With the notes that were taken at the beginning of the procedure, available in the Amazon OpenSearch Service console, fill in the fields available on the next screen as shown below and click “Save”:

| Field in Azure AD                               | Value of the Amazon OpenSearch Service console        |
|-------------------------------------------------|-------------------------------------------------------|
| Identifier (Entity ID)                          | Service Provider Entity ID                            |
| Response URL (Affirmation consumer service URL) | SSO URL initiated by SP                               |
| Log in to the URL                               | OpenSearch panel URLs or OpenSearch custom panel URLs |

The configuration was similar to the one in the image below:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-13.png)

Once this step is complete, we download the federation metadata XML file to complete the configuration on the Amazon OpenSearch Service side:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-14.png)

In the Amazon OpenSearch Service console, we went back to the section for editing SAML attributes and did “Import from XML file”, pointing to the file that was downloaded in the previous step:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-13.jpg)

When importing files, the “IdP Entity ID” field was automatically populated (the XML format may vary):

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-16.png)

On this same screen, we have two settings indicated as “SAML master username” and “SAML master backend role”. We complete these fields with an Azure AD user and group, which gives the OpenSearch panels master access through single sign-on, effectively being a service administrator. Once configured, you can use this user/group to perform object management in Amazon OpenSearch Service, creating other functions or other users linked to Azure AD.

Note: The “master user” has all the permissions to administer the Amazon OpenSearch Service domain. This is the first user created when creating an Amazon OpenSearch Service domain.
To do this, we add the User Principal Name (UPN) attribute value of a user and the Group Name of a group (it can be one or the other) that has permission in the Azure Active Directory business application in our laboratory, as shown below:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-17.png)

As a test, we left the Access Policy of our Amazon OpenSearch Service domain as a published domain level for any authenticated object. To do this, we added the User Principal Name (UPN) attribute value of a user and the Group Name of a group (it can be one or the other) that has permission in the Azure Active Directory business application in our laboratory, as shown below:

Note: The security of Amazon OpenSearch Service has three main layers:
Red
The first layer of security is the network, which determines if requests can reach an Amazon OpenSearch Service domain. You can choose to provision your Amazon OpenSearch Service domain with public access or only connections that originate from a VPC.
Domain Access Policy
The second layer of security is the domain access policy. Once a request reaches a domain endpoint, the resource-based access policy allows or denies the request access to a particular URI. The access policy accepts or rejects requests at the “edge” of the domain before they reach the Amazon OpenSearch service.
Detailed access control — FGAC
The third and final layer of security is refined access control, the FGAC. After a resource-based access policy allows a request to reach a domain endpoint, the FGAC begins the user authentication process. If FGCA successfully authenticates the user, it retrieves all the permissions assigned to that user.

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-18.png)

At the end of these steps, we click “Save Changes” at the bottom of the page:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-17.jpg)

With these configurations made, using the user or the member of the group, it was possible to access the OpenSearch panels through SSO, with the Azure AD credential. To do this, it can be accessed through the application at https://myapplications.microsoft.com, as stated above, or through the “URL of OpenSearch panels”/“URL of custom OpenSearch panels” (for example, https://myopensearch.mydomain.com). In both cases, the authentication request was directed to Azure AD using the SAML standard.

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-20.png)

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-21.png)

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-22.png)

Once we logged in, we were with an authorized user to perform other relevant configurations, such as registering new Azure AD users and/or groups with roles in OpenSearch Dashboards, thus managing authorization according to their needs.

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-21.jpg)

Note: The OpenSearch Dashboards Tenants are spaces for storing index patterns, visualizations, panels, and other OpenSearch Dashboards objects. By default, all OpenSearch Dashboards users have access to two Tenants: Private and Global. The Global Tenant is shared among all OpenSearch Dashboards users. The Private Tenant is unique for each user and cannot be shared.
Tenants are useful for securely sharing their work with other OpenSearch Dashboards users. You can control which functions have access to a Tenant and whether those functions have read or write access.
You can use the Private Tenant for exploratory work, create detailed visualizations with your team in an Analyst Tenant, and maintain a summary panel for corporate leadership in an Executive Tenant.
Ready! We integrated OpenSearch Dashboards with the Azure Active Directory identity provider.

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-24.png)

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-25.png)

To manage the objects, simply access the Security\ Roles menu:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-26.png)

We created a new role:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-27.png)

After creation, we select the “Assigned Users” tab, “Map Users” option:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-28.png)

We added the user's UPN to the Users (or to the group under “Backend Features”):

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-27.jpg)

The login with the new user was added:

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/Utilizando-o-Azure-AD-como-provedor-de-identidade-para-o-OpenSearch-Dashboards-no-Amazon-OpenSearch-Service-30.png)

In this blog post, we discuss the integration of OpenSearch Dashboards with Azure AD through the SAML 2.0 standard, highlighting an alternative for companies to track the credentials of that service through the identity provider of their choice.

## References

[What is Amazon OpenSearch Service?](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/what-is.html)

[SAML authentication for OpenSearch dashboards](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/saml.html)

[Amazon Elasticsearch Service is now Amazon OpenSearch Service and is compatible with OpenSearch 1.0](https://aws.amazon.com/blogs/aws/announcing-amazon-opensearch-service-which-supports-opensearch-10/)

[Detailed access control in Amazon OpenSearch Service](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/fgac.html)

This article was translated from the [AWS Blog in Portuguese](https://aws.amazon.com/pt/blogs/aws-brasil/utilizando-o-azure-ad-como-provedor-de-identidade-para-o-opensearch-dashboards-no-amazon-opensearch-service/)

---

![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/lucianob.png)

Luciano Bernardes is currently working as Mr. Partner Solutions Architect (PSA) at AWS, specializing in Microsoft workloads. With 15 years of experience in the market, he worked mainly in specialized technical consulting at Microsoft, for clients from various verticals, with demands focused on local infrastructure and the cloud. As a PSA, he works closely with LATAM consulting partners to help them leverage and scale Microsoft technologies in the AWS cloud.


![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2020/09/01/caiorc2.png)

Caio Ribeiro César is currently working as a sales specialist specializing in Microsoft technology in the AWS cloud. He began his professional career as a systems administrator, which he continued for more than 15 years in areas such as Information Security, Online Identity and Corporate Email Platforms. He recently became a fan of AWS cloud computing and helps customers harness the power of Microsoft technology on AWS.


![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/costrob.png)

Robert Costa is a Senior Solution Architect who has worked for more than 20 years in the area of technology in companies in the financial sector, having participated in several modernization projects. Today he is dedicated to improving customers seeking innovation through the use of AWS cloud services. He also loves traveling with family and friends to enjoy his free time by the sea.


![](https://d2908q01vomqb2.cloudfront.net/4d134bc072212ace2df385dae143139da74ec0ef/2021/11/18/rgumiero.png)

Rafael Gumiero is a senior architect of analytical solutions at Amazon Web Services. An enthusiast of distributed and open source systems provides guidance to customers who develop their solutions with AWS Analytics services, helping them to optimize the value of their solutions.
