# About SP Initiate Login Flow

The SP Initiate Login Flow section captures key actions (therfore tests) an SP needs to perform in order to properly communciate with the user's home IdP to initiate user login. Key Actions include:

* Consume IdP SAML metadata:
  * Locate and retrieve IdP metadata via InCommon
  * Verify metadata authenticity and integrity
  * Retrieve IdP connetion endpoints in metadata
* Generate SAML Authentication Request
  * NOTE: these are described in the Request-Generation section
* Direct user to IdP to initiate Login
  * Send SAML AuthnRequest in accodance with SAML WebBrowerSSO Profile to initiate user login 
