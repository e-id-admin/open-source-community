# Swiss e-ID and trust infrastructure: Roadmap (work in progress)

#### Table of contents
- [Introduction](#Introduction)
- [Supported Technical Standards](#Supported-Technical-Standards)
- [Reasoning for proposed standards and further explanations](#Reasoning-for-proposed-standards-and-further-explanations)
- [Device Binding Strategy and Targeted Level of Assurance](#Device-Binding-Strategy-and-Targeted-Level-of-Assurance)
- [Privacy-Preserving Device Binding](#Privacy-Preserving-Device-Binding)
- [The Swiss e-ID Program](#The-Swiss-e-ID-Program)
- [Changelog](#Changelog)


## Introduction

An initial selection which technology will be utilized for the issuance of the e-ID and underlying trust infrastructure has been made. A more detailed explanation regarding this choice can be found here: [Initial Technology](https://github.com/e-id-admin/open-source-community/blob/main/tech-roadmap/initial-technology.md) 

The e-ID Program strives to provide a transparent insight into the current working hypothesis for issuance of the e-ID and the underlying trust infrastructure. 
As mentioned in [Initial Technology](https://github.com/e-id-admin/open-source-community/blob/main/tech-roadmap/initial-technology.md), the FDJP aims to provide a architecture, that in the long term, has the ability to support multiple standards and will allow ecosystem participants to decide what capability or format should be prioritized for their use case. The future evolution regarding multi-stack capabilities will also be published in this document.

The following table provides a reference that indicates which standards are currently in selection for the Swiss trust infrastructure. It is a working hypothesis that will be updated if perspectives within the implementing organizations change. We invite interested parties to “star” the repository to receive automatic updates in case this document undergoes changes. 
We currently differentiate between support for [Public-Beta](https://www.eid.admin.ch/en/public-beta-e) and the initial go live support.

The domain of digital identities is developing at a rapid pace. While the Confederation aims to provide a degree of assurance and stability for integrators even at this early stage, evolution seems inevitable. Changes will be considered, especially if they benefit privacy-protection for users, increase the security and stability of the overall system, or if standards converge to serve the purpose of fostering interoperability.

## Supported Technical Standards

| Aspect      | Current Hypothesis   | Link   | Public Beta Support   | Initial Go Live Support |  
| ----------- | ----------- |----------- |----------- |----------- |
| **Identifiers**       | Decentralized Identifiers (**DIDs**) v1.0 according to W3C <br> DID Method: **did:tdw/did:webvh**    | W3C: https://www.w3.org/TR/did-core/ <br> Method: Trust DID Web - https://identity.foundation/trustdidweb/ | **SELECTED** <br> Hosted on central base registry provided by Confederation | **CANDIDATE** Final decision for productive system pending |
| **Status Mechanisms**       | Statuslist | Statuslist: https://datatracker.ietf.org/doc/draft-ietf-oauth-status-list/ | **SELECTED** | **HIGH** |
| **Trust Protocol**       | Trust Protocol based on VCs |  [Trust protocol based on VCs](https://github.com/e-id-admin/open-source-community/blob/main/tech-roadmap/rfcs/trust-protocol/trust-protocol.md)  | **SELECTED** <br> Initial support of the "identity" trust statement by Confederation | **HIGH** <br> Additional support of issuer & verifier legitimacy (per VC schema) |
| **Communication Protocol (Issuance/Verification)**       | OID4VC/OID4VP	  | Issuance: https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0-ID1.html <br> Verification: https://openid.net/specs/openid-4-verifiable-presentations-1_0-ID2.html  | **SELECTED** <br> In accordance with [Swiss profile](https://github.com/admin-ch-ssi/specifications-to-publish/blob/main/swiss_profile.md) | **HIGH** |
| **Payload Encryption**       | JWE as proposed by the communication protocol  | https://www.rfc-editor.org/rfc/rfc7516.html | **CANDIDATE** | **CANDIDATE** |
| **VC-Format/Signature-Scheme Combination**       | SD-JWT VC & ECDSA  | SD-JWT VC: https://datatracker.ietf.org/doc/draft-ietf-oauth-sd-jwt-vc/ | **SELECTED** <br> In accordance with [Swiss profile](https://github.com/e-id-admin/open-source-community/blob/main/tech-roadmap/swiss-profile.md) | **HIGH** |
| **Device Binding Scheme**       | **Hardware** based device binding depending on capabilities provided by mobile devices <br> **Software** based device binding implemented by wallets  | Apple: https://developer.apple.com/documentation/cryptokit/secureenclave <br> Android: https://source.android.com/docs/security/features/keystore | Hardware  **SELECTED** <br> Software **UNSUPPORTED**  | Hardware **HIGH** <br> Software **HIGH** |
| **VC appearance**       | Visualization of Verifiable Credential with OCA	| https://github.com/e-id-admin/open-source-community/blob/main/tech-roadmap/rfcs/oca/spec.md | **UNSUPPORTED** | **HIGH**|

*Versions of the referenced specs may change.*

**Probability interpretation:** <br>
- **UNSUPPORTED** = No Solution provided, <br>
- **OPEN** = Options are being assessed, <br>
- **CANDIDATE** = Standard/Spec is in selection, <br>
- **HIGH** = Current hypothesis for implementation, <br>
- **SELECTED** = Chosen for system,

### Additional open points:
The delivery of all open points noted here is currently undated.

- The implementation of [privacy-preserving hardware device binding](#privacy-preserving-device-binding)
- The implementation of [batch-issuance](#privacy-preserving-device-binding)

## Reasoning for proposed standards and further explanations 

### Identifiers / DID Method 

The Confederation intends to implement the proposed “Identifiers” in the e-ID Act (https://www.fedlex.admin.ch/eli/fga/2023/2843/de) as decentralized identifiers according to the W3C specification. 

For the Public Beta system the DID Method **did:tdw** has been chosen. It benefits from similar simplicity to did:web while adding additional security properties, in particular data integrity, which helps to operate a trustworthy ecosystem. This version of the specification refers to the OLD name for the [DID Method V0.3](https://identity.foundation/didwebvh/v0.3/). The DID Method name has since changed to **did:webvh** (“did:web + Verifiable History”).

Given the centralized role of the base registry in the Swiss trust infrastructures a solution is necessary that makes it impossible for the central instance to change the DID document of an ecosystem participant and therefore ensures trustworthiness in the data provided. In addition, the integrity of the DID document is verifiable on the client side. 

Before the final decision regarding the productive DID Method is be taken, additional clarifications are necessary.


### Status Mechanisms  

**Token Status Lists** will be prioritized because they consist a simple, lightweight and scalable approach to support revocation.

Potential observability and unlinkability concerns associated with Status Lists can partially be mitigated by the architecture of the Swiss trust infrastructure.
Given the central storage of revocation lists (on a system provided by the Confederation), issuers will not be able to monitor the activity related to a specific VC. The Confederation on the other hand will only know, which status list was accessed, without gaining information about the individual status checked within the list. 

Regarding unlinkability: once a verifier knows the index within a specific status list, they gain a point to potentially correlate and monitor the status of the credential. This topic can be mitigated, for example with batch issuance of ephemeral credentials. For certain use cases the capability to monitor the status of a VC could be deemed a useful capability.

### Trust Protocol 

The primary requirements regarding the trust registry are stipulated by the proposed e-ID Act and primarily consist of the information to be provided:

- verification of the identity provided by the issuers and verifiers (e.g., actor who claims to be fedpol actually is fedpol);
- information related to the secure usage of verifiable credentials (e.g., fedpol is the only legitimate issuer of the e-ID credential); 

After an evaluation of the previously published options (incl. OpenID Federation), it was decided to start with a specific Swiss implementation. This allows a minimal implementation reducing as much complexity as possible, is in line with the current timeline and can be a straight-forward implementation of the legal requirements.

The trust protocol could change in the future depending on technological adoption and maturity of other standards. 

#### Trust Protocol based on VCs
The trust protocol will be a independent specification published by the Confederation which uses signed SD-JWT VCs as containers to express trust statements about ecosystem actors.

There are currently three trust statements being considered:
- **Identity** trust statements containing validated data about an actor like their name,  their logo (if applicable) and other information related to the actor. This information and the binding between associated DID and actor will be vetted by the Confederation. 
- **Issuance** trust statements containing a reference to the schemas that the actor is authorized to issue.
- **Verification** trust statements containing a reference to the schemas that the actor is authorized to verify.

### VC-Format & Signature Scheme

The e-ID Program is currently focussing on SD-JWT VC (VC-Format) with ECDSA signatures for the initial product delivery. When possible, this shall happen in accordance of keeping a multi-stack approach (e.g. independence between DID Methods and credential format / cryptographic schemes). 

#### SD-JWT VC & ECDSA

For possible future interoperability with the EU, the e-ID delivery teams are following the evolution of the OpenID4VC High Assurance Interoperability Profile (HAIP) (https://openid.net/specs/openid4vc-high-assurance-interoperability-profile-sd-jwt-vc-1_0-00.html) and mapping differences.

The currently published Swiss Profile for the Public Beta has similar restrictions like the HAIP Profile. However, there are a few significant differences especially in regards to the use of identifiers.

This proposal does not offer “unlinkability” during presentation but this could be mitigated with batch issuance of ephemeral credentials. 

### Communication Protocol
In order to be interoperable with other actors and to use a well-defined standard, the confederation intends to use **OID4VCI** (https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0-ID1.html) and **OID4VP** (https://openid.net/specs/openid-4-verifiable-presentations-1_0-ID2.html) as communication protocols.
OID4VCI is a well-established issuance protocol in the SSI community. It requires reasonably low effort to implement and offers a good flexibility. The same goes for OID4VP. Both protocols make interoperability on the protocol level fairly easy and are receiving widespread adoption within the SSI-Community.
A solution, offering asynchronous communication could be offered at a later stage, but in order to keep technical complexity relatively straightforward, is currently not being pursued with high priority. 

### Payload Encryption
As OID4VC and OID4VP are the current hypothesis for the communication protocols, the hypothesis for the payload encryption is to be in line with these standards. Both define a way to do payload encryption.
This protects personally identifiable information between the Wallet and an Issuer or Verifier, even if the transport encryption is broken during a part of the transmission.

### VC Appearance

In order to offer harmonized appearance of credentials as well as additional metadata about credentials, the confederation decided to start an initial technical specification based on **OCA** (https://humancolossus.foundation/overlays-capture-architecture).
There currently appears to be no standard with high market penetration, that offers the capability to make the credential appearance highly customizable. Specifically in the topics of visual representation, attribute metadata and other relevant information related to UI. OCA enables the possability to define the metadata of a VC type with already a lot of standardization effort done, and it is extensible to fulfill the design requirements for visual interpretation of credentials.

To standardize the use of OCA for VC appearances, the confederation proposes a [technical specification](https://github.com/e-id-admin/open-source-community/blob/main/tech-roadmap/rfcs/oca/spec.md). The technical specification adds restrictions to certain aspects which were not fully clarified in OCA and adds additions necessary to implement the visual ambition of the federal Wallet.

## Device Binding Strategy and Targeted Level of Assurance 
The e-ID Act is currently in deliberation by the the Swiss National Parliament. Amendends to the current bill were made, that postulate that the binding of the e-ID to holders is ensured during issuance. The National Council has recently foreseen, that third party wallets will be supported if holder binding can be assured. This binding is in line with requirements expressed by specific sectors relying on a state-issued digital identity. Especially in the health sector (electronic patient record) as well as in identity and access management (authentication service of the Swiss authorities - AGOV). Further deliberation of these requirements has shown, that additional security measures are inevitable. They are mainly centered around ensuring the correct mode of operation for wallets (hardware-based device binding) and measures to restrict the transferability of the e-ID to conform with applicable standards (e.g. eCH-0170 level 3). While initially communication on the ambition level regarding device binding and level of assurance was indeterminate, further clarification has confirmed that an adaptation of the strategy regarding mobile wallets and devices is necessary to ensure security for users. 
In summary these actions are:
- measures to ensure that the online identity verification process exclusively happens in the federal wallet
- measures to issue the e-ID exclusively to wallets installed on mobile devices with a built-in cryptographic processor; (either by using key attestation or by wallet attestation, which requires certification and the enactment of specific ordinances by the confederation)
  

## Privacy-Preserving Device Binding and Unlinkability

The Confederation is aware, that there is potentially a current contradiction between hardware-based device binding, available cryptographic schemes, and the aspiration to offer unlinkability. This primarily arises due to the limited support of hardware backed cryptographic schemes available on current smartphones and the fact that none of the broadly available schemes support unlinkability. It is further compounded by the impossibility of adding alternative schemes as a third party. 
Nonetheless, the Confederation sees multiple approaches on how unlinkability might still be achieved by combining conventional cryptography (e.g., ECDSA) and more privacy-preserving schemes. Potential solutions that are being evaluated are:
- Ephemeral Credentials: This would mean batch issuance or ad-hoc issuance of VC's from issuers to holders. Currently, this is deemed to be the most feasible solution, to offer unlinkability in the short-term to mid-term.
- ZKP mechanisms based on novel cryptographic methods. Confirming conventional hardware holder binding (based on existing schemes) as part of a  process/proving interaction between holder and verifier.
- Linkable device binding in combination with BBS: Would be based on cryptographic schemes currently available on mobile phones. This would offer a certain degree of readiness if future cryptographic processors support privacy-preserving device binding. There is no immediate unlinkability.  
- Anonymous Attestation Service: Device-Binding is implemented as in the previous variation (Selective Disclosure). During a verification where proof of device binding is required, users would disclose their linkable hardware-based device binding and verifier nonce (without any other attested information contained in the VC) towards a device binding confirmation service provided by the federal government. The service validates the device binding and issues an appropriate attestation, which can then be transferred to the verifier. Unlinkability is reached against verifiers. Linkability exists in relation to the federal government, but correlation is limited since only signatures are exchanged without any content-related data. The storage of potentially linkable data by the Confederation can be controlled and prevented.
- Cloud-HSM: Usage of a centralized HSM supporting hardware-based privacy-preserving device binding. Decentralized access with hardware-based authentication using a method with higher market penetration (e.g., FIDO2 or conventional cryptographic schemes). Currently, this is not pursued with high priority due to the centralized storage aspect.

The Confederation is involved in the Open Wallet Foundation (https://openwallet.foundation/) to advance the topic at an international level. In addition a dedicated team with specific resources for research will be allocated. The e-ID Program intends to collaborate with the existing e-ID community to advance the topic.

## The Swiss e-ID Program

The GitHub discussion forum for open source and technical topics is available here: https://github.com/e-id-admin/open-source-community/discussions <br>
More information about the Swiss Confederations e-ID Program can be found under: https://www.eid.admin.ch/ <br>
Sign up to our newsletter under: https://www.eid.admin.ch/de/warum-partizipation

## Changelog
_06.12.2024_
- Update after decision on initial technology by the Confederation

_18.06.2024_
- Initial Publication of v1.0 (Work-in-progress)
