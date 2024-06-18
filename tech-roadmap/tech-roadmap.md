# Swiss E-ID & Trust Infrastructure — Technical Roadmap (WIP)

#### Table of contents
- [Introduction](#Introduction)
- [Proposed Technical Standards](#Proposed-Technical-Standards)
- [Reasoning for proposed standards and further explanations](#Reasoning-for-proposed-standards-and-further-explanations)
- [Holder Binding Strategy and Targeted Level of Assurance](#Holder-Binding-Strategy-and-Targeted-Level-of-Assurance)
- [Privacy-Preserving Holder Binding](#Privacy-Preserving-Holder-Binding)
- [The Swiss E-ID Program](#The-Swiss-E-ID-Program)
- [Changelog](#Changelog)


## Introduction

As communicated on June 14, 2024, the E-ID Program is currently following an approach that strives to offer strong privacy-preserving capabilities as well as laying the foundation for interoperability with the European Union and others. 

The Confederation has not made its final decision on the technology used for the issuance of the E-ID itself yet — this will presumably be taken by the end of 2024 and allow for more time to assess in particular the financial impact in more detail. 
Nonetheless, the E-ID Program strives to provide a transparent insight into the current working hypothesis for the underlying trust infrastructure. 
The FDJP currently aims to provide a multi-stack architecture that supports multiple standards and enables business cases to decide what capability or format should be prioritized for their use case. 

The following table provides a reference that indicates which standards are currently in selection for the Swiss trust infrastructure. It is a working hypothesis that will be updated if perspectives within the implementing organizations change. We invite interested parties to “star” the repository to receive automatic updates in case this document undergoes changes.

The domain of digital identities is developing at a rapid pace. While the Confederation aims to provide a degree of assurance and stability for integrators even at this early stage, evolution seems inevitable. Changes will be considered, especially if they benefit privacy-protection for users, increase the security and stability of the overall system, or if standards converge to serve the purpose of fostering interoperability.


## Proposed Technical Standards


| Aspect      | Current Hypothesis   | Link   | Probability   |
| ----------- | ----------- |----------- |----------- |
| **Identifiers**       | Decentralized Identifiers (**DIDs**) v1.0 according to W3C <br> DID Method: **did:tdw**    | W3C: https://www.w3.org/TR/did-core/ <br> Method: Trust DID Web - https://bcgov.github.io/trustdidweb/ | **HIGH** |
| **Status Mechanisms**       | Statuslist & Accumulator  | Statuslist: https://www.w3.org/TR/vc-bitstring-status-list/ <br> Accumulator: Currently open | Statuslist: **HIGH** <br> Accumulator: **CANDIDATE** |
| **Trust Protocol**       | OpenID Federation or proprietary solution  | OpenID Federation: https://openid.net/specs/openid-federation-1_0.html <br> Proprietary solution: Currently open | **CANDIDATE** |
| **Communication Protocol (Issuance/Verification)**       | OID4VC/OID4VP	  | Issuance: https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0-ID1.html <br> Verification: https://openid.net/specs/openid-4-verifiable-presentations-1_0-ID2.html  | **HIGH**|
| **Payload Encryption**       | JWE as proposed by the communication protocol  | https://www.rfc-editor.org/rfc/rfc7516.html | **CANDIDATE** |
| **VC-Format/Signature-Scheme Combination**       | Option EU: SD-JWT & ECDSA/EdDSA <br> Option Privacy: JSON-LD & BBS  | Option EU: https://datatracker.ietf.org/doc/draft-ietf-oauth-sd-jwt-vc/ <br> Option Privacy: *See VC-Format & Signature Scheme for links* | Both options: **CANDIDATE** |
| **Holder Binding Scheme**       | Hardware based holder binding depending on capabilities provided by mobile devices (most likely ECDSA) | Apple: https://developer.apple.com/documentation/cryptokit/secureenclave <br> Android: https://source.android.com/docs/security/features/keystore | **HIGH** for hardware holder Binding <br> **OPEN** for concrete holder binding implementation |
| **VC appearance**       | Overlay Capture Architecture (OCA)	| https://humancolossus.foundation/overlays-capture-architecture | **HIGH**|

*Versions of the referenced specs may change.*

**Probability interpretation:** <br>
- **OPEN** = Options are being assessed, <br>
- **CANDIDATE** = Standard/Spec is in selection, <br>
- **HIGH** = Current hypothesis for implementation,

### Additional open points:
- The implementation of [privacy-preserving hardware holder binding](#privacy-preserving-holder-binding)




## Reasoning for proposed standards and further explanations 

### Identifiers / DID Method 

The Confederation intends to implement the proposed “Identifiers” (https://www.fedlex.admin.ch/eli/fga/2023/2843/de) as decentralized identifiers according to the W3C recommendation. **Did:tdw** benefits from the simplicity of did:web while adding additional security properties, in particular data integrity, that helps to operate a secure ecosystem. 
A solution is preferred that makes it impossible for a central instance to change the DID document of an ecosystem participant, and therefore ensures trustworthiness. In addition, the integrity of the DID document is verifiable on the client side. 
The pre-key-rotation feature of did:tdw allows the participants to gain back control of a DID document if they lost their active private key or if their active private key got compromised.





### Status Mechanisms  

Especially concerning the E-ID, this is still an open point that will be clarified by the E-ID Program within the next few months.
It is closely related to the question about which format and cryptography will be used for the Swiss E-ID. 

Depending on what is prioritized (EU Interoperability vs. Unlinkability), either **Status-List** (Interoperability with the EU) or **Accumulators** (Unlinkability) will be preferred. Additionally, clarifications are underway on what frequency of revocation (real-time vs. scheduled publication) is needed and if offline verification is considered a must. The current train of thought is to prioritize the real-time publication of revocation information. Furthermore, it is probable that both solutions will be offered by the confederation. In that case, issuers could be given the option to decide what mechanisms and publication schedule are most appropriate for their use case. 



### Trust Protocol 

Which trust protocol will be utilized is currently being evaluated. The primary requirements regarding the trust registry are stipulated by the proposed law and primarily consist of the information to be provided:

- verification of the identity provided by the issuers and verifiers (e.g., actor who claims to be fedpol actually is fedpol);
- information related to the secure usage of verifiable credentials (e.g., fedpol is the only legitimate issuer of the E-ID credential); 

Currently, there are two options under consideration:

#### OpenID Federation
OpenID Federation is a fairly mature standard that could be used as the trust protocol for the Swiss ecosystem. It seems flexible and therefore can be used for various use cases.
Most likely not the whole standard would be implemented but isolated functionalities in accordance with the expected legal basis would be defined by the E-ID Program.
The SSI communities' interest in this standard is currently rising, which also means that there is the probability that others will adopt OpenID Federation as their trust protocol. This could make interoperability easier in the future.
If this solution is chosen, an interoperability profile will be proposed. 

#### Swiss Solution 
If none of the existing drafts or standards turn out to be feasible, it could be deemed necessary to provide a dedicated Swiss solution. 
In this case, the Confederation would aim to reduce complexity as much as possible to make the implementation efficient and directly in line with the requirements postulated by the proposed law. It could potentially be in line with “Credential Trust Establishment” as proposed by DIF (https://identity.foundation/credential-trust-establishment/).
The solution would still be built with already proposed standards (DID, VC, DID Credential registry) and solely add additional functionality to ensure trust-mechanisms.  
If this approach is chosen, the Confederation would publish a specification of the implementation as well as integration guidelines for third-parties. 

### VC-Format & Signature Scheme 
Currently, the Delivery Teams of the E-ID Program are tasked with delivering an infrastructure that allows for a multi-format and multi-signature ecosystem. Initially, the ecosystem should support the following options:

#### Privacy-Preserving: JSON-LD & BBS 
One combination proposed is: JSON-LD Verifiable Credentials with BBS Signatures. While use of the BBS Signature Scheme is undisputed (https://identity.foundation/bbs-signature/draft-irtf-cfrg-bbs-signatures.html), the underlying curve is currently under evaluation to make sure it meets the required level of security.
The JSON-LD VC will follow the W3C VC Data Model 2.0 (https://www.w3.org/TR/vc-data-model-2.0/). In addition, VC Data integrity (https://www.w3.org/TR/vc-data-model-2.0/) will be used together with the BBS Cryptosuite (https://www.w3.org/TR/vc-di-bbs/). This allows the Confederation to easily add other signatures to the same credential format — if this is deemed necessary in the future. Parallel signatures are possible with this approach, which could be a remedy to the post quantum threat vector and be tackled by additionally signing VC's with PQC at a later stage. 
This proposal offers “unlinkability” during presentation. 

#### EU Interoperability: SD-JWT & ECDSA/EdDSA
The other combination proposed is: SD-JWT Verifiable Credentials with ECDSA/EdDSA Signatures.
For easier interoperability with the EU, the SD-JWT VC implementation should follow the OpenID4VC High Assurance Interoperability Profile with SD-JWT VC (https://openid.net/specs/openid4vc-high-assurance-interoperability-profile-sd-jwt-vc-1_0-00.html). This would mean ECDSA signatures are supported. EdDSA could be added at a later point in time.
Unless specific organizational measures are taken (namely ad-hoc issuance or bulk issuance of ephemeral credentials) this proposal does not offer “unlinkability” during presentation. 

### Communication Protocol
In order to be interoperable with other actors and to use a well-defined standard, the confederation intends to use **OID4VCI** (https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0-ID1.html) and **OID4VP** (https://openid.net/specs/openid-4-verifiable-presentations-1_0-ID2.html) as communication protocols.
OID4VCI is a well-established issuance protocol in the SSI community. It needs reasonably low effort to implement and offers a good flexibility. The same goes for OID4VP. Both protocols make interoperability on the protocol level fairly easy and are receiving widespread adoption within the SSI-Community.
A solution, offering asynchronous communication could be offered at a later stage, but in order to keep technical complexity relatively straightforward, is not being pursued with high priority. 

### Payload Encryption
As OID4VC and OID4VP are the current hypothesis for the communication protocols, the hypothesis for the payload encryption is to be in line with these standards. Both define a way to do payload encryption.
We are aware of the fact, that the proposed payload encryption does not provide a complete privacy-preserving end-to-end encryption since it still allows metadata leakage due to the unencrypted parts of messages. 
A solution, which tackles this could be offered at a later stage, but in order to keep technical complexity relatively straightforward, is not being pursued with high priority. 

### VC Appearance   
In order to offer harmonized appearance of credentials as well as more provide additional metadata about credentials, the confederation is currently looking into **OCA** (https://humancolossus.foundation/overlays-capture-architecture).
There appears to be no standard, which currently makes the credential appearance highly customizable in the topics of branding, attribute metadata and other relevant information related to UI. OCA makes it possible to define the metadata of a VC type with already a lot of standardization effort done, and it is extensible to fulfil design requirements for visual interpretation of credentials.

## Holder Binding Strategy and Targeted Level of Assurance 
During its deliberation of the E-ID Act, the National Council amended the current legal proposal to postulate that the binding of the E-ID to holders is ensured during issuance. This is in line with requirements expressed by specific sectors relying on a state-issued digital identity. Especially in the health sector (electronic patient record) as well as in identity and access management (authentication service of the Swiss authorities - AGOV). Further deliberation of these requirements has shown, that additional security measures are inevitable. They are mainly centered around ensuring the correct mode of operation for wallets (hardware-based holder binding) and measures to restrict the transferability of the E-ID to conform with applicable standards (eCH-0170 level 3). 
While initially communication on the ambition level regarding holder binding and level of assurance was indeterminate, further clarification has confirmed that an adaptation of the strategy regarding mobile wallets and devices is necessary to ensure security for users. In addition, the proposed strategy needs to keep in mind that the timely delivery of the E-ID and trust infrastructure is crucial. 
The actions identified are:
- measures to issue the E-ID exclusively to mobile devices with the federal wallet installed (initially, it's foreseen to solely support the wallet provided by the Confederation — an opening for third-party wallets is in discussion);
- measures to issue the E-ID exclusively to mobile devices with a built-in cryptographic processor;
- ensuring holder binding of the E-ID to the cryptographic processor of the mobile device (correct mode of operation);


## Privacy-Preserving Holder Binding

The Confederation is aware, that there is a potential contradiction between hardware-based holder binding, available cryptographic schemes, and the aspiration to offer unlinkability. This primarily arises due to the limited support of hardware backed cryptographic schemes available on current smartphones and the fact that none of the broadly available schemes support unlinkability. It is further compounded by the impossibility of adding alternative schemes as a third party. 
Nonetheless, the Confederation sees multiple approaches on how unlinkability might still be achieved by combing conventional cryptography (e.g., ECDSA) and more privacy-preserving schemes (e.g., BBS). Potential solutions that are being evaluated are:
- Linkable holder binding in combination with BBS: Would be based on cryptographic schemes currently available on mobile phones. This would offer a certain degree of readiness if future cryptographic processors support privacy-preserving holder binding. There is no immediate unlinkability.  
- Linkable holder binding as a claim contained in a VC and disclosed only in situations where the proof of holder binding is required: Unlinkability is reached for low-assurance use cases.
- Anonymous Attestation Service: Holder-Binding is implemented as in the previous variation (Selective Disclosure). During a verification where proof of holder binding is required, users would disclose their linkable hardware-based holder binding and verifier nonce (without any other attested information contained in the VC) towards a holder binding confirmation service provided by the federal government. The service validates the holder binding and issues an appropriate attestation, which can then be transferred to the verifier. Unlinkability is reached against verifiers. Linkability exists in relation to the federal government, but correlation is limited since only signatures are exchanged without any content-related data. The storage of potentially linkable data by the Confederation can be controlled and prevented.
- Ephemeral Credentials: This would mean bulk issuance or ad-hoc issuance of VC's from issuers to holders. Currently, this is not pursued with high priority due to additional data flows between issuer and holder and added complexity regarding VC, key management, and revocation.
- Cloud-HSM: Usage of a centralized HSM supporting hardware-based privacy-preserving holder binding. Decentralized access with hardware-based authentication using a method with higher market penetration (e.g., FIDO2 or conventional cryptographic schemes). Currently, this is not pursued with high priority due to the centralized storage aspect.

The Confederation is involved in the Open Wallet Foundation (https://openwallet.foundation/) to advance the topic at an international level. 


## The Swiss E-ID Program

The GitHub discussion forum for open source and technical topics is available here: https://github.com/e-id-admin/open-source-community/discussions <br>
More information about the Swiss Confederations E-ID Program can be found under: https://www.eid.admin.ch/ <br>
Sign up to our newsletter under: https://www.eid.admin.ch/de/warum-partizipation

## Changelog
_18.06.2024_ - Initial Publication of v1.0 (Work-in-progress)
