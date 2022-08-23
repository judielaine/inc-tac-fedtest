# SDP-MD02: Verify SAML Metadata Signature

## Statement
Consumption of metadata MUST be contingent on verification of a signature (STRONGLY RECOMMENDED) or TLS server certificate. It MUST be possible to communicate changes to the keys within the metadata without also changing the key used to establish trust in the metadata.

_In most cases, this requirement implies that a key communicated via metadata will not also be used to sign and verify the same metadata, but it is possible to construct scenarios in which this may happen if metadata verification relies on a chain of certificates signed by an ultimately trusted Certificate Authority. The details of such an approach are beyond the scope of this document._

## Testing Objective
1. An entity participating in a ~~federated~~ InCommon SAML SSO tranaction can verify that the entity metadata it retrieves from the InCommon metadata registry is properly signed by the InCommon Federation.

## Definitions
| Term | Defintion |
| ------- | ---------------------------------------------- |
| Subject | The SAML entity undergoing metadata validation. The entity may be an IdP or an SP. |
| Metadata Registry | An authorizative source of entity metadata used for InCommon Federation readiness |


## Prerequisite

1. An authorizative source of entity metadata (ie Metadata Registry) used for InCommon Federation readiness exists.

2. 6 test enttiy metadata records exists in the Metadata Registry:
  * An IdP entity metadata with a correctly signed signature.
  * An IdP entity metadata with an incorrectly signed signature.
  * An IdP entity metadata without a signature. 
  * An SP entity metadata with a correctly signed signature.
  * An SP entity metadata with an incorrectly signed signature.
  * An SP entity metadata without a signature. 

3. The Subject has successfully retrieved the appropriate testing entity metadata from the Metadata Registry. (todo: insert reference to the download test)

2. The Subject has sussessfully retrieved the metadata signing certificate from its published location. (todo: provide instruction to signing certificate loation)

## Tests
### Test A: Verifying signature when signature is correct.

todo: what does the entity do to validate signature?

#### Test A Evaluation Criteria 

This is a pass/fail evaluation. 

The Subject **passes** Test A if: 
1. it correctly calculates the signature digest from the metadata, *and*
2. it correctly recognizes the calcuated digest matches the supplied signature and accepts the metadata as a properly signed metadata.

The Subject **fails** Test A otherwise.

### Test B. Verifying signature when signature is incorrect or missing.

todo: what does the entity do to validate signature?

#### Test B Evaluation Criteria 

This is a pass/fail test. 

The Subject passes if:

1. if the signature is missing: the Subject recognizes the metadata is missing a signature and rejects the metadata. Alternatively, 
2. if the signature is present: the Subject correctly calculates the signature digest from the metadata, *and*
3. it correctly recognizes the calcuated digest does not match the supplied signature and accepts the metadata as a properly signed metadata.

## Acceptance

The Subject **passes** this test if it passes both Test A and Test B. Otherwise, it **fails** this test.


## Postcondition

N/A





