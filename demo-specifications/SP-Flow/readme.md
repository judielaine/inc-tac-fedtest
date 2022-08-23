# About SP-Flow

The tests within this folder measure an SAML SP's ability to initiate, navigate, and complete a SAML login flow within the InCommon Federation. These tests are designed to measure the SP's behavior via external observation. A testing client simulating a user browser agent performs the login steps in various combinations of sequences. The testing client determines the SP's ability to meet interoperability requirements by observing responses from both the SP being tested and the IdP supplied by a pre-arranged test harness.

Where applicable, SAML2Int, related SAML2 specifciations, and HTTP specifications are the reference points for conformance evaluation.

The tests are organized in three main sections:

1. [**Initiate Login**](Initiate-Login/about-init-login.md) - Given a known IdP at a single point in time, measures the SP's ability to properly consume InCommon-published SAML metadata and use the information within to initiate user login at the IdP.
2. [**Request Generation**](Request-Generation/about-request-generation.md) - Given a known IdP at a single point in time, measures the SP's ability to generate abnd deliver properly formatted SAML AuthnRequest in various scenarios.
3. [**Response Processing**](Response-Processing/about-response-processing.md) - Measures the SP's ability to process an IdP's SAML Response in various scenarios and act accordingly. 

## Related Tests

There are other SP side flows important to an SP's ability to interoperate with an InCommon IdP. These are to be documented in a separate volume. Examples of these flows (therefore tests) include:

* Automatically detect IdP metadata change and update integration behavior accordingly.
* Using an external IdP discovery service to determine a user's home IdP.
* Handling Single Logout 
* User attribute, including identifier handling
* SP metadata validation: SP has properly formatted and published SAML metadata in federation metadata registry

## References

* [SAML Test Harness Repository from Roland](https://github.com/rhoerbe/sthrep2)