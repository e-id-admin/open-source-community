🚧 THIS IS A DRAFT AND MAY BE OBJECT TO CHANGES. OFFICIAL PUBLICATION ON DEC 1, 2023 🚧


# Discussion Paper: Initial technological basis for the Swiss national trust infrastructure 

## Defining a common ground for the Swiss electronic identity and other verifiable credentials

# 1 Purpose of the document
In 2021, the Swiss federal government was mandated by parliament to start laying the groundwork for an electronic Identity for inhabitants of Switzerland and Swiss people living abroad.
In parallel to the law-writing process over the last two years, the Swiss federal government built technical proof of concepts, established a broad community and developed a common vision for the future Swiss electronic identity (eID), a trust ecosystem and the underlying technical infrastructure. Now the confederation is facing an important question: What should the initial technical basis of this vision be?

By involving the broader community, a discussion on two potential and realistic scenarios described in this document shall be enabled. This document aims to create a synthesized overview of the current knowledge established within the federal government and brings up multiple aspects that may favorize the one or the other kind of technical realization for the architecture of a national trust infrastructure. The benefits and drawbacks are collected, particularly regarding communication and exchange protocols, credential and data formats, and cryptography used for verifiable credentials. 

These presented scenarios are meant to stimulate public discussion and provide the eID Team with external perspectives on potential ways forward. Even though the content of this document has a technical focus, political and societal aspects will be drivers for a decision. Depending on the public's voice and the assessments of the presented scenarios, the eID Team will take a decision for a starting point to build the technical foundation of the national trust infrastructure.

# 2 Background
## 2.1  Vision of an eID and a Trust Infrastructure for Verifiable Credentials
The main driver for an eID is the expressed political will of six parliamentary groups that Switzerland should possess a state-issued and operated eID. The rejection of the previous proposal for a national e-ID law in 2021 resulted in six [political motions](https://www.parlament.ch/de/ratsbetrieb/suche-curia-vista/geschaeft?AffairId=20213124), requiring the future eID to follow the principles of privacy by design, data minimization and decentralized data storage.

In consequence, a public discussion was held to define the scope of the eID and its system, which resulted in the following decisions:
- A new law-making project was launched.
- The law shall be technologically neutral, but the general architecture and roles shall follow the principles of self-sovereign identity (SSI).
- Ambition Level 3 shall be reached, i.e., not only state-issued credentials like the eID shall be supported, but any other kind of digital credential can be realized by third parties with the provided infrastructure.
- The state shall be responsible for the issuance of the eID and for the operation of the necessary systems needed for the trust infrastructure (base registry, trust registry, wallet, check-app, standardization of channels and formats).
The detailed steps of the law-making project and additional information are published on the FOJ website: https://www.bj.admin.ch/bj/de/home/staat/gesetzgebung/staatliche-e-id.html


## 2.2	EU eIDAS 2.0 and the Architecture Reference Framework
In June 2021, the EU published a law proposal that is similar to the ambitions of Switzerland.On the technical side, the EUDI Wallet Architecture and Reference Framework (ARF)  was published in January 2023 with the goal of developing an interoperable digital identity wallet solution in the EU. The ARF defines an initial direction for the technical implementation of the eIDAS 2.0 regulation. The current understanding of the E-ID Project Team is that the ARF is considered an initial Version and will evolve further over time. 

The ARF offers an approach based on well-known cryptography and technology that is being rapidly standardized. This enables a technically pragmatic way of offering digital identities. It is our assessment that the outlined technologies in the ARF offer a realistic pathway to providing digital identities, and implementation for integrators is transparent and fairly straightforward.

It is worth noting that the new law proposal as well as the technology proposed by the ARF do come with downsides. The legislation is currently under criticism by various members of academia, civil society and data privacy organizations ([Open Letter Pre-Trilogue Conclusion](https://epicenter.works/en/content/eidas-open-letter-pre-trilogue-conclusion-300-academics-19-ngos)), requesting that unlinkability be mandated by the legislation. The technology as proposed in the ARF offers a high conformance with existing security practices yet has very specific flaws concerning certain aspects of privacy preservation. Namely, the credentials contain a unique technical footprint that is transmitted to verifiers during each verification. This can lead to easier traceability of a holder’s actions, as well as easier collusion if verifiers combine data. 

As Switzerland is not a member of the EU, the eID project team relies on information made available to the public and provided by the broader eID community.

## 2.3	Practical experience and external expertise collected by the confederation
Establishing a nationwide trust infrastructure is a complex undertaking that will take several years to complete. Therefore, having a solid technical foundation is of prime importance. This foundation must be based on sound technical principles and have broad acceptance on a technical, political and societal level. To get the necessary technical insights, it was decided that the confederation would start building small-scale systems and gain hands-on experience deploying and operating products based on SSI-Technology. Hence, the projects ePerso (electronic employee ID card), the "Public Sandbox Trust Infrastructure" (implementation of a base registry), and eLFA (mobile learners' driver license) were launched. The expertise of the federal E-ID Team, as well as most of the knowledge contributing to this document, was vastly gained from these projects. 

To further benefit from the vast knowledge and network of the Swiss eID Community, a Technical Advisory Circle (TAC) was formed with the goal of further advising and informing the confederation. The project team would like to take the chance to express its deep gratitude to the members of the TAC for their involvement in the process leading up to now.  
In the run-up to the publication of this document, the TAC held three meetings and discussed various points regarding general challenges, answers to specific questions, potential solutions, and scenarios to provide the Swiss eID. It's noteworthy that the TAC members have a diverse background, and discussions do not always lead to consensus. Nonetheless, the meetings offer an excellent gauge of the current state of the SSI field.

Noteworthy points mentioned during the TAC meetings:
- Discussions about maturity of technological solutions
- Lack of in-depth scientific proof regarding some technologies
- Challenges associated with the decentralized storage of credentials
- Future readiness of utilized cryptography (PQC)
- Privacy Considerations regarding the traceability of verifiable credentials (unlinkability)
- Privacy Considerations regarding the traceability related to credential revocation
- Secure Storage of credentials and cryptographic keys
- Potential technological scenarios and a multi-technology approach

An overview of the slides utilized as a basis for discussion and the meeting minutes of the TAC meetings are published here: 
https://github.com/e-id-admin/general/tree/main/meetings


# 3	Design Principles  
## 3.1	Design Principles
The parliamentary motions – asking for a state-issued and state-operated electronic identity (eID) – instructed the federal council to provide an eID that follows the principles of privacy by design, data minimization and decentralized data storage.

A basic architecture was defined and the essential roles and principles has been brought into the preliminary draft law.

![Trust_Diamond_SSI_Switzerland](https://github.com/admin-ch-ssi/discussion-paper/assets/12694135/499aa2d1-3538-4b49-9b09-96615994e526)

**Privacy by design** is addressed by different means:
- The eID law follows the SSI principles with three different roles: issuers, holders, and verifiers. These roles communicate directly with each other. 
- The issuer shall have any knowledge about the use of verifiable credentials issued by himself. The eID law defines this privacy-preserving principle.
- The system should protect privacy by default. As the eID law is technology-agnostic, it does not elaborate on what this means precisely.

**Data minimization** is addressed by different means:
- Any kind of verifiable credential can be presented in its entirety or only partially (selective disclosure);
- If feasible, information can be presented as a derivation (predicate proof; e.g., “is older than 18”) or as a "zero knowledge proof".
- The necessary central base registry only contains the minimal data concerning registered issuers and verifiers as well as on revocation. There are no central traces regarding issued verifiable credentials. The eID law defines what information is contained in the base registry.
- Unnecessary data flows are avoided through the SSI paradigm (e.g., by utilizing direct communication between the concerned actors). 

**Decentralized data storage** is achieved by storing the eID and other verifiable credentials solely in the user’s wallet. From there, it is presented to a verifier. As a consequence, every transmission of data is under control of the user, the presentation of data to a verifier always demands an active consent of the user.

**Further requirements** were strongly requested during the public consultation:
- International compatibility, especially within the European Union
- The launch of the Swiss eID shall be as soon as possible

## 3.2	Criteria
To structure discussions during the knowledge-gathering activities of the eID team, the following criteria were used: privacy preservation, security, maturity, ease of use, readiness for the future, interoperability & standardization, operability & integration. The intention was to bring up the main differences between the possible approaches and not to create a complete comparison matrix. These already exist, for instance, as a [Credential Format Comparison](https://openwallet-foundation.github.io/credential-format-comparison-sig/) by [IDunion members](https://www.linkedin.com/posts/idunion_credential-format-comparison-and-idunion-ugcPost-7008024118574895104-Cu1d/)  and was transferred to the Open Wallet Foundation. Given the fact that not every arbitrary combination of technologies is feasible, it became evident that a perfect “tech stack” does not exist.

The discussion around the following scenarios in Chapter 4 shall help define the trade-offs we need to make. We can detect various aspects that are in tension with each other, e.g., ensuring the highest levels of data security vs. keeping the product simple and user-friendly; having a high speed of implementation vs. ensuring full interoperability; protecting the user's privacy at its maximum vs. keeping system complexity down. 
There are no black-and-white answers to these questions, and a certain level of compromise in specific areas will be necessary.  To offer a pathway forward, the proposed approaches point either in a more pragmatic, internationally harmonized direction or give more weight to privacy at the cost of utilizing technologies that have lower familiarity and maturity.

# 4 Decision on technological scenarios
## 4.1 Introduction & Challenges

In the field of self-sovereign identities, there are many initiatives developing different solutions. There are currently few globally acknowledged and utilized standards and systems; a dominant design has yet to establish itself. To enable the aspiration of an open, publicly used trust infrastructure and use cases that utilize the eID and extend it with further value-adding services and credentials, it is necessary to define an initial technological framework for Switzerland.

These defined technologies shall offer a certain degree of stability for the development teams providing the basic underlying infrastructure. Furthermore, the wider public, which would like to offer systems to consume the eID and issue further credentials in the future, gains more stable ground to start implementing.
To satisfy this, the federal eID team wants to take a decision on the core technologies used for credential transmission and credential-related communication, as well as the format of credentials, in early 2024.  

It is important to state that the field regarding digital identities based on SSI is still young, and dynamic changes and further evolution will occure. The decision that will be taken should be understood as a starting point that will likely evolve.
The eID will not be a system developed once and then operated indefinitely without changes. Continual development and adaptation to new circumstances will be required to ensure long-term service continuity, security, and interoperability. 

Based on the various learnings made by the eID project and development teams, as well as implicitly confirmed by the TAC, it is our assessment that none of the current technological frameworks or stacks fully comply with all requirements, which were stated in:
- the parliamentarian motions
- the feedbacks on the public consultation 
- the requirements of the eID team relating to the operation of a national infrastructure

The main points relating to this assessment were mentioned in Chapter 3.2.
In summary, they surround the technical areas of:
- Privacy preservation (in particular how much tracking of a user's actions is prevented by the core technology)
- Interoperability (will the selected technology be compatible with systems provided in other countries)
- Maturity of the systems (in particular how much effort needs to be invested to run the system in a safe, stable, and usable manner for millions of users)

Nonetheless, by accepting certain compromises on these points, it will be feasible to provide an eID solution to the Swiss public. To come up with a pragmatic way forward, in this discussion paper two realistic scenarios are illustrated and allow to display their benefits and drawbacks. Subsequently, the public discussion should provide the confederation with an outside view of which scenario is preferred. 

## 4.2 Scenario A: Following the EU's technical direction
_Scenario A_ would mean utilizing a subset of the technologies as proposed in the European Union's Architecture Reference Framework [ARF](https://digital-strategy.ec.europa.eu/en/library/european-digital-identity-wallet-architecture-and-reference-framework), with the ambition to align closely with the EU's proposal for implementation. 

Namely, SD-JWTs for credentials and OID4VC/VP as an issuing and presentation protocol. The ARF provides a pragmatic technical approach to implementing digital identities and digital identity wallets. It is based on well-known and proven technologies and cryptography, and while the proposed communication protocols are new, they offer easy integration for ecosystem participants and transparency for software developers.

The EU’s indication to use specific technologies has accelerated efforts in areas of technical standardization. Given the independence of standardization bodies and the ARFs status as a work-in-progress, a certain level of uncertainty regarding the final outcome of the technical architecture will remain. As the ARF already contains multiple standards (SD-JWT and ISO-MDL), it is also possible that further technological fragmentation might occur.    

Regarding privacy preservation, there have been concerns raised by technical communities, data protection officers, academia, and civil society since the publication of the ARF.  Viewed in the context of Switzerland, the analysis of the eID team indicates that this criticism mainly relates to the topic of “unlinkability”. Without workaround implementations, such as mass-issuance of credentials for one-time use, the unlinkability of the used credentials cannot be guaranteed at the current time (see Annex). This is especially sensitive regarding credentials that are used regularly or depict the identity of a person.

Selective disclosure is possible, and users can consent to share subsets of the content of a verifiable credential. Although hashes of all the data contained in a credential are always transmitted, representing a kind of fingerprint.

The proposed solution to manage the revocation of credentials (Status List) is simple to implement and can enable offline verification. It can be argued, that it has certain flaws regarding privacy preservation. Specific actors within the ecosystem can memorize a position in the list provided for revocation and then track any status changes related to the credential (e.g., when it gets revoked). No information relating to the content of a credential is stored within the revocation list nor the reason for the revocation. This tracking can only occur once the actor knows the position of the credential within the status list (disclosed during the presentation). 

It’s worth noting that Switzerland has an independent legal process, and _Scenario A_ does not mean a direct adoption of EIDAS 2.0 legislation or compatibility. The Architecture Reference Framework is considered a work in progress, and an evolution towards a more privacy-preserving manner is possible. An adoption of _Scenario A_ might lead to easier compatibility with the EU down the line. 

**Main benefits for a Swiss Trust Ecosystem:**
- Potentially easier technical compatibility with the EU 
- Mutual legal recognition might be easier
- Common technology and standardized cryptography
- Rapid standardization and further public knowledge gain due to EU efforts
- Easy understanding and integration for service providers
- Broad support for hardware based cryptography

**Main drawbacks for a Swiss Trust Ecosystem:**
- Potential linkability of the holder's credential use 
- Potential traceability through revocation 
- No predicates or zero knowledge proofs
- Selective disclosure is only partially data sparing

**Technologies**
- Credential Format: SD-JWT
- Signatures: ECDSA or RSA
- Communication Standards: OID4VC/VP
- Revocation Service: Status List

**Risks & uncertainties**
- The EU ARF is not considered to be final yet. To fully comply with the final technology utilized by the EU, Switzerland might need to delay its national eID undertaking by an unknown amount of time. 
- Concerns about privacy and traceability are present, but have not been addressed yet. Based on the publics acceptance of this fact, trust in the system might be undermined. 


## 4.3 Scenario B
_Scenario B_ would utilize a technology stack that aspires to offer a higher level of privacy for holders in the ecosystem than that provided in the aforementioned _Scenario A_.  At the current time, this would mean choosing an independent technological path for the Swiss trust infrastructure and the national eID.

The current hypothesis is that credentials would be issued based on JSON-LD-verifiable credentials utilizing BBS+ signatures. Concerning the communication protocol, the verdict remains uncertain in this particular scenario. Both commonly utilized protocols in the SSI field (DIDcomm and OID4VP/VC) are compatible with the proposed credential format. DIDcomm boasts the benefit of end-to-end encryption, while OID4VC/VP offers easier integration and is based on standards to which various implementers are accustomed.

By utilizing alternative but less common cryptography, credentials can be issued that enable holders to navigate the future trust ecosystem while leaving fewer digital traces behind. The properties regarding “unlinkability” (see Annex) are met by this technology stack. Credentials would utilize signature schemes that allow wallets to generate unique proofs for each verification. These proofs contain no constants and therefore don't link back to the originally issued credential. Given that the generation of the proofs is unique per verification, correlation between verifiers is made harder. 
Nonetheless, it is noteworthy that the disclosed content itself may allow linking and correlation.

As with the technology utilized in _Scenario A_, selective disclosure is also supported. It seems possible to achieve predicate proofs in BBS+ by utilizing additional sub-protocols. The information available regarding this subject is comparatively limited. 

Following the path of non-correlation and unlinkability further challenges arise that go beyond the presentation of credentials. In the context of _Scenario B_ privacy preserving revocation is another topic which would need to be addressed. 
Different possibilities could be implemented in order to reduce traceability of credential usage through revocation services. These pivot around storing revocation data in a government service, delegating the generation of non-revocation proofs to holders, or utilizing validity credentials. Without these efforts, actors can potentially gain knowledge about when a credential was used or when the status of a credential changed (e.g., became revoked). It's important to state, that there is not one solution which fits the needs of all credentials or ecosystem participants. Certain use cases might benefit from being able to track changes in credential status. Hence in _Scenario B_ multiple revocation mechanisms may come in to play. 

Standardization of [JSON-LD Credentials](https://www.w3.org/TR/vc-data-model-2.0/) as well as [BBS+](https://datatracker.ietf.org/doc/draft-irtf-cfrg-bbs-signatures/) is ongoing. No prediction can be made about when these standards will be considered final. 

Deciding on _Scenario B_ initially would mean accepting a gap between the Swiss and EU implementation.
Nonetheless, the credential format and signature scheme proposed in this chapter might be additional acceptable implementations in the context of eIDAS 2.0 (relating to credentials that don't represent a person). The proposed _Scenario B_ might therefore still achieve EU conformity at a later point.

The team responsible for eID has ascertained that the technology employed in _Scenario B_, despite promising improved privacy preservation, exhibits a lower level of maturity compared to _Scenario A_. 
A decision for _Scenario B_ can result in more effort and resources required to provide the eID.    

**Main benefits for a Swiss Trust Ecosystem:**
- Unlinkability for holders is provided
- Correlation of holder data is made harder for colluding verifiers  
- Selective disclosure is done in a data sparing manner
- Privacy preserving revocation might be achieved
- Zero knowledge proofs and predicate proofs seem attainable  
- JSON-LD capabilities are gained (Automatization & User Experience)
- Fallback to Scenario A is possible

**Main Drawbacks for a Swiss Trust Ecosystem:**
- Initially no technical compatibility or additional effort needed (e.g., converter service) to reach interoperability with the EU digital Identity
- More complex integration and less known cryptography (full understanding of function only attainable by experts)
- Security of proposed Scenario less researched
- Smaller development community
- Unclear support for hardware based cryptography (Some providers claim to support BLS12-381)

**Technologies**
- Credential Format: JSON-LD
- Signatures: BBS+
- Communication Standards: OID4VC/VP or DIDCOMM
- Revocation Service: Open

**Risks & uncertainties**
- Utilizing less common technologies (e.g., a reduced number of libraries) as well as the requirement to implement more complex mechanisms to enhance privacy could have an impact on the development timeline.
- International adoption and therefore potential interoperability for Swiss eID holders remain unclear.
- The privacy-preserving capabilities gained by utilizing BBS+ signatures might be lost in the future due to post-quantum cryptography that may not support these.

## 4.4 Summary

In summary, we see that there are two potential paths forward that Switzerland can follow to offer a national eID. Both have their unique advantages and disadvantages. In a simplified manner, they can be summed up in the following way:

_Scenario A_: Commonly known cryptography is utilized. Technical interoperability with the EU should be achievable in a straightforward manner. A broad community, existing libraries and approved cryptography should simplify development. Regarding privacy, holders have full control over what content of a credential (selective disclosure) they share with which actor. Given that the utilized credentials have a constant footprint and the system for revocation does not offer full privacy preservation, holders can be tracked in an easier manner after presenting a credential to a verifier. Relying heavily on the EU ARF, especially since it is not in a finalized version, might not offer the stability desired.

_Scenario B_: The cryptography utilized is less common and extensively researched. Technical interoperability in the EU could prove more challenging. Nontheless, it could still be achievable by converting the system or additionally issuing an eIDAS 2.0-conforming credential at a later stage. Given a higher aspiration for privacy preservation, implementations (e.g., revocation service) have the potential to become more complex. More effort might have to be invested in the creation of libraries and security attestation. Regarding privacy, holders have full control over what content of a credential (selective disclosure) they share with which actor. Given that more sophisticated cryptography is utilized and credentials have a dynamic footprint during each presentation, it should be harder to track a holder's actions after verification. If implementation fails, a fallback to scenario A is viable.


## 4.5 Unregarded Aspects

By nature of the complexity, which is related to the SSI topic and the underlying technology, open questions remain that are not covered by these scenarios. That being said, for at least some of them, it is required to have a trend-setting decision in place that establishes principles for further system design. 

Currently unregarded aspects in the scenarios:
- hardware/crypto-chip based wallet security	
- base registery design and implementation
- trust registry design and implementation
- decision on DID usage and DID Methods
- the eID issuance process
- backup of verifiable credentials

# 5. Public discussion of this paper and further process
Now that we've reached this chapter, your voice is needed! 

The eID team is looking to make a legitimate decision, considering opinions from a broad, interested public. Please take the proposed scenarios as a basis for your feedback and consider that the final decision is a starting point for the medium-term technical development. The eID team is convinced that both variants cover the principal expectations for a verifiable credential trust ecosystem. From your standpoint, what characteristics and features are most valuable and should absolutely be achieved? Which trade-offs and risks are acceptable, why, and for how long?

**Cornerstones of the public discussion**
- The public debate starts with the presentation of this paper at the participation meeting on December 1st, 2023.
- The written statements have to be entered into our feedback form at [www.findmind.ch/...](https://www.findmind.ch/...) until January 3rd, 2024. Hint: Take a look at the form before you start writing your feedback.
- Written discussion can be held on GitHub: https://github.com/e-id-admin/open-source-community/discussions

**The main questions for your feedback**
- According to you, which scenarios fulfill your expectations?
- Which scenario would you prefer? And for what reason?
- Which “red lines” shall not be crossed? Where is no compromise thinkable for you?
- What risks do you consider acceptable for the Swiss trust infrastructure?

You can also contribute additional points in the written statements. Just consider that the current discussion does not need to cover the same topics mentioned in past public consultations (Zielbild E-ID, public consultation on the preliminary draft law).

For questions, shared thoughts, and requests for information on unclarity, use the public discussion on [GitHub](https://github.com/e-id-admin/open-source-community/discussions). We value this public debate once again as a time for  everybody (the eID team as well as the community) to expand their perspectives.

The various inputs will be considered, allowing the eID Program to define the initial technology stack (protocols, formats, and cryptography) early in 2024. The decision will be taken by the program commissioning body (Programmauftraggeber). With the decision, the foundation to start building the Swiss trust infrastructure for verifiable credentials will be laid. The definition of detailed specifications expected from external parties for the future ecosystem can then be addressed. Realization of the Swiss trust infrastructure will commence, with the aim of launching the eID in 2026. 


# Annex
// todo

# A. Lessons learned
## A.1	Hands-on Experience

### Public Trust Infrastructure Sandbox

### eLFA


## A.2 Specific learnings related to technologies
### Hyperledger Indy Stack

### OpenID 

### Other Technical Stacks
#### KERI 
#### ACDC
#### MDL/mDoc

## A.3	Unlinkability
### SD-JWT
### BBS Signatures
### Summary on unlinkability in potential credential formats 

## A.4	Holder Binding

## A.5	Revocation

## A.6	Presentation Fingerprints and Dataflows
