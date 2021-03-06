.. meta::
   :description: Aviatrix User SSL VPN Okta SAML Configuration
   :keywords: Okta, SAML, user vpn, okta saml, Aviatrix, OpenVPN

==============================================================================
OpenVPN® with SAML Client on Okta IDP with Optional Multifactor Authentication 
==============================================================================



1.  Overview
------------

Aviatrix user VPN is the only OpenVPN® based remote VPN solution that provides a vpn client that supports SAML authentication. 

This guide provides an example on how to use Aviatrix SAML client to authenticate Okta IDP. When SAML client is used, Aviatrix controller acts as the identity service provider (ISP) that redirects browser traffic from client to IDP, in this case, Okta, for authentication. 

2. Pre-Deployment Checklist
-----------------------------
Before configuring the SAML integration between Aviatrix and Okta, make sure the following is completed.

Pre Installation Check List

	1.	Aviatrix Controller is setup and running.
	2.	Have a valid Okta account with admin access.
	3.	Download and install the Aviatrix SAML client These prerequisites are explained in detail below.


2.1 Aviatrix Controller
------------------------

If you haven’t already deployed the Aviatrix controller, follow the below instructions on how to deploy the Aviatrix controller.
`Instructions here.  <http://docs.aviatrix.com>`_

2.2 Okta Account
----------------

A valid Okta account with admin access is required to configure the integration. If you don’t already have an Okta account, 
please create one with the following link from Okta.
`Okta create account <https://www.okta.com/start-with-okta/>`_

2.3 Aviatrix VPN Client
-----------------------

All users must use the Aviatrix VPN client to connect to the system.  Download the client for your OS 
`here. <http://docs.aviatrix.com/Downloads/samlclient.html>`_


3. Configuration
----------------

The integration configuration consists of 4 parts.

	1.	Create an Okta SAML App for Aviatrix
	2.	Retrieve OKta IDP metadata
	3.	Launch Aviatrix Gateway
	4.	Create Aviatrix SAML SP
	5.	Create Aviatrix VPN User

Please complete the configuration in the following order.

3.1 Create an Okta SAML App for Aviatrix
-----------------------------------------

This step is usually done by the Okta Admin.

	1.	Login to the Okta Admin portal
	2.	Click “Admin”
	3.	Click “Applications”
	4.	Click “Add Application”
	5.	Click “Create New App”
	
		a.	Platform = Web
		b.	Sign on method = SAML 2.0

|image0|
	
	6.	General Settings
	
		a.	App Name = Aviatrix Dev (arbitrary)

|image1|

	7.  SAML Settings
		a.	Single sign on URL* = https://aviatrix_controller_hostname/flask/saml/sso/aviatrix_sp_name
		b.	Audience URI(Entity ID)* = https://aviatrix_controller_hostname/
		c.	Default RelayState* = 
		d.	Name ID format = Unspecified
		e.	Application username = Okta username

		These values are also available in the controller OpenVPN®->Users page after step 3.4

		|image2|
		
		The aviatrix_controller_hostname is the hostname of the Aviatrix controller(If no DNS is used, this is the public IP). The aviatrix_sp_name
		is an arbitrary identifier. Note this value as it will be needed when configuring SAML from the Aviatrix controller. 
		Please contact your Aviatrix admin if you do not have the Aviatrix controller’s public IP address.
		
		f.	Attribute Statements
		
			i.	FirstName -> Unspecified -> user.firstName
			ii.	LastName -> Unspecified -> user.lastName
			iii.	Email -> Unspecified -> user.email

|image3|		
			
	8.  Done		
	
	
3.2  Retrieve Okta IDP metadata
--------------------------------
This step is usually completed by the Okta admin.

After the above application is created, click on “Sign On” and then “View Setup Instructions”.

|image4|

Look for the section titled “IDP metadata to your SP provider”.

|image5|
Note this information. This information will be used to configure the SAML configuration on the Aviatrix controller.

3.3	Launch Aviatrix Gateway
---------------------------------------------

This step is usually completed by the Aviatrix admin.

	1.	Login to the Aviatrix controller
	2.	Click Gateway -> Add New
	3.	Select the appropriate Account, region, vpc, subnet and gateway size
	4.	Check “VPN Access” and then “Enable SAML”

	|image6|
	
	5.	Default settings for everything else.
	
	6.	Click “OK” to launch the gateway.
	
	
3.4	Create Aviatrix SAML SP (Endpoint)
------------------------------------------

This step is usually completed by the Aviatrix admin.

1.	Login to the Aviatrix Controller
2.	Click OpenVPN® -> VPN Users -> Advanced -> SAML -> Add New
3.	Select the VPC where the above gateway was launched
4.	Name = aviatrix_sp_name (this is the username that you choose during the Okta SAML configuration)
5.	IPD Metadata type = Text
6.	IDP Metadata Text = paste in the IDP metadata from the Okta configuration



3.5	Test the integration
----------------------------

1.	Have an instance of the VPN client running, else it might throw a warning
2.	Click Test from OpenVPN® -> VPN Users -> Advanced -> SAML -> aviatrix_sp_name
3.	You should be redirected to the IDP, now you can log in and should be redirected back to the controller
	

3.5	Create a VPN User
-------------------------

1.	Select the VPC where the above gateway was launched
2.	Username = Name of the VPN user
3.	User Email = any valid email address (this is where the cert file will be sent). ALternatively you can download the cert if you dont enter email
4.	Load the VPN user certificate and try connecting to the VPN. Note that SAML only supports shared certificates. You can share the certificate among VPN users or create more VPN users

|image7|


3.6     Configure Okta for Multifactor Authentication (OPTIONAL)
----------------------------------------------------------------

Once you have successfully configured Okta IDP with Aviatrix SP, you can configure Okta for Multifactor Authentication. 

Please read this `article <https://support.okta.com/help/Documentation/Knowledge_Article/Multifactor-Authentication-1320134400>`__ from Okta on Multifactor setup.  

See this `article <https://support.okta.com/help/Documentation/Knowledge_Article/Configuring-Duo-Security-734413457>`__ if you're interested in using DUO in particular.


OpenVPN is a registered trademark of OpenVPN Inc.


.. |image0| image:: SSL_VPN_Okta_SAML_media/image0.png

.. |image1| image:: SSL_VPN_Okta_SAML_media/image1.png

.. |image2| image:: SSL_VPN_Okta_SAML_media/image2.png

.. |image3| image:: SSL_VPN_Okta_SAML_media/image3.png

.. |image4| image:: SSL_VPN_Okta_SAML_media/image4.png

.. |image5| image:: SSL_VPN_Okta_SAML_media/image5.png

.. |image6| image:: SSL_VPN_Okta_SAML_media/image6.png

.. |image7| image:: SSL_VPN_Okta_SAML_media/image7.png


.. disqus::
