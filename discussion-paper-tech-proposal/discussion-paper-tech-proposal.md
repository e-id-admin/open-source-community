# Discussion Paper: Initial technological basis for the Swiss trust infrastructure

### Defining common ground for the Swiss electronic identity and other verifiable credentials

#### Table of contents
- [Purpose of the document](#1-Purpose-of-the-document)
- [Background](#2-Background)
- [Design Principles and Criteria](#3Design-principles-and-Criteria)
- [Decision on technological scenarios](#4-Decision-on-technological-scenarios)
- [Public discussion of this paper and further process](#5-Public-discussion-of-this-paper-and-further-process)
- [Annex](#Annex)
- [References](#References)
- [Changelog](#Changelog)

# 1 Purpose of the document
In 2021, the Swiss federal government was mandated by parliament to start laying the groundwork for an electronic identity for people living in Switzerland and Swiss people living abroad. In parallel to drafting the Act over the last two years, the Swiss federal government obtained technical proof of concepts, established a broad community and developed a common vision for the future Swiss electronic identity (e-ID), a trust ecosystem enabling other third-party credentials and the underlying technical infrastructure.  

> Now the Confederation is facing an important question:<br> _What should the __initial__ technical basis for this vision be?_

By involving the broader community, it is intended to hold a discussion on two potential and realistic scenarios described in this document. This document aims to create a synthesised overview of the current knowledge established within the federal government and highlight multiple aspects that may favour the one or other type of technical solution for the architecture of a national trust infrastructure. The benefits and drawbacks are set out, particularly regarding communication and exchange protocols, credential and data formats, and cryptography used for verifiable credentials.

The scenarios presented are meant to stimulate public discussion and provide the e-ID Team with external perspectives on potential ways forward. Even though the content of this document has a technical focus, political and societal aspects will be drivers for a decision. Based on the public reaction and the assessments of the scenarios presented, the e-ID Team will take a decision for a starting point to build the technical foundations for the national trust infrastructure.

# 2 Background
## 2.1  Vision of an e-ID and a Trust Infrastructure for Verifiable Credentials

The main driver for an e-ID is the political will expressed by six parliamentary groups that Switzerland should have a state-issued and operated e-ID. The rejection of the previous proposal for a national e-ID Act in 2021 resulted in six [political motions](https://www.parlament.ch/de/ratsbetrieb/suche-curia-vista/geschaeft?AffairId=20213124), requiring the future e-ID to follow the principles of privacy by design, data minimisation and decentralised data storage.

As a consequence, a public discussion was held to define the scope of the e-ID and its system, which resulted in the following decisions:

-	A new legislative project was launched.
-	The Act has to be technologically neutral, but the general architecture and roles will follow the principles of self-sovereign identity (SSI).
-	Ambition Level 3 has to be reached, i.e., not only state-issued credentials like the e-ID are supported, but any other kind of digital credential can be realised by third parties with the infrastructure provided.
-	The state is responsible for issuing the e-ID and for operating the systems needed for the trust infrastructure (base registry, trust registry, wallet, check-app, standardisation of channels and formats).
- The detailed steps of the legislative project and additional information are published on the FOJ website: https://www.bj.admin.ch/bj/de/home/staat/gesetzgebung/staatliche-e-id.html


## 2.2	EU eIDAS 2.0 and the Architecture Reference Framework
In June 2021, the EU published a draft Act that is similar in its ambitions to those of Switzerland. The EUDI Wallet Architecture and Reference Framework (ARF) was published in January 2023 with the goal of developing an interoperable digital identity wallet solution in the EU. The ARF defines an initial direction for the technical implementation of the eIDAS 2.0 Regulation. The current understanding of the E-ID Project Team is that the ARF is considered an initial version and will evolve further over time.

The ARF offers an approach based on well-known cryptography and technology that is being rapidly standardised. This allows a technically pragmatic way of offering digital identities. It is our assessment that the outlined technologies in the ARF offer a realistic pathway to providing digital identities, and implementation for integrators is transparent and fairly straightforward.

It is worth noting that the draft Act and the technology proposed by the EU ARF do come with downsides. The legislation has been criticised by various experts from academia, civil society and data privacy organisations ([Open Letter Pre-Trilogue Conclusion](https://epicenter.works/en/content/eidas-open-letter-pre-trilogue-conclusion-300-academics-19-ngos)), who have requested that the legislation require unlinkability. The technology as proposed in the ARF offers a high level of conformity with existing security practices, yet has very specific flaws concerning certain aspects of privacy preservation. In particular, the credentials contain a unique technical footprint that is transmitted to verifiers during each verification. This can make it easier to trace a holder’s actions, and to collude if verifiers combine data.

As Switzerland is not a member of the EU, the e-ID project team relies on information made available to the public and provided by the broader e-ID community.

## 2.3	Practical experience and external expertise collected by the Confederation
Establishing a nationwide trust infrastructure is a complex undertaking that will take several years to complete. As a result, having a solid technical foundation is of prime importance. This foundation must be based on sound technical principles and be broadly accepted in technical and political terms and by the public. To develop the necessary technical capabilities, it was decided that the Confederation would start building small-scale systems and gain hands-on experience deploying and operating products based on SSI-Technology. Hence, the projects ePerso (electronic employee ID card), the "Public Sandbox Trust Infrastructure" (implementation of a base registry), and eLFA (mobile learners' driver license) were launched. The expertise of the federal E-ID Team, as well as most of the knowledge contributing to this document, was largely gained from these projects.

To further benefit from the vast knowledge and network of the Swiss e-ID Community, a Technical Advisory Circle (TAC) was formed with the goal of advising the Confederation. The project team would like to take this opportunity to express its sincere thanks to the members of the TAC for their involvement in the process up to now.

In the run-up to the publication of this document, the TAC held three meetings, discussing various aspects of general challenges, answering to specific questions, and considering potential solutions, and scenarios for providing the Swiss e-ID. It is worth noting that the TAC members have diverse backgrounds, and discussions do not always lead to consensus. Nonetheless, the meetings offer an excellent gauge of the current state of the SSI field.

Noteworthy points mentioned during the TAC meetings:
-	Discussions about maturity of technological solutions
-	Lack of in-depth scientific proof regarding some technologies
-	Challenges associated with the decentralised storage of credentials
-	Future readiness of cryptography used (post-quantum cryptography)
-	Privacy considerations regarding the traceability of verifiable credentials (unlinkability)
-	Privacy considerations regarding traceability related to credential revocation
-	Secure storage of credentials and cryptographic keys
-	Potential technological scenarios and a multi-technology approach

The slides utilised as a basis for discussion and the meeting minutes of the TAC meetings are published here:
https://github.com/e-id-admin/general/tree/main/meetings


# 3	Design Principles and Criteria
## 3.1	Design Principles
The parliamentary motions – asking for a state-issued and state-operated electronic identity (e-ID) – instructed the Federal Council to provide an e-ID that follows the principles of privacy by design, data minimisation and decentralised data storage.

A basic architecture was defined and the essential roles and principles have been brought into the [draft Act](https://www.newsd.admin.ch/newsd/message/attachments/84263.pdf) and have been explained in the [dispatch](https://www.newsd.admin.ch/newsd/message/attachments/84260.pdf).

![swiss_trust_diamond](https://github.com/e-id-admin/open-source-community/assets/12694135/3fe1b206-7563-403a-ac92-be9e28d582e6)
*Illustration 1: Swiss Trust Infrastructure*

**Privacy by design** is addressed by different means:
-	The e-ID Act follows the SSI principles with three different roles: issuers, holders, and verifiers. These roles communicate directly with each other.
-	The issuer has no knowledge if or how the verifiable credentials that it issues are used. The e-ID Act defines this privacy-preserving principle.
-	The system should protect privacy by default. As the e-ID Act is technology-agnostic, it does not elaborate on what this means precisely.

**Data minimisation** is addressed by different means:
-	A verifiable credential can be presented in its entirety or only partially (selective disclosure);
-	If feasible, information can be presented as a derivation (predicate proof; e.g., “is older than 18”) or as a "zero knowledge proof".
-	The necessary central base registry only contains the minimum of data concerning registered issuers and verifiers or related to revoked credentials. There are no traces of data related to individually issued credentials stored centrally. The e-ID Act defines what information is contained in the base registry.
-	Unnecessary data flows are avoided through the SSI paradigm (e.g. by using direct communication between the concerned actors).

**Decentralised data storage** is achieved by storing the e-ID and other verifiable credentials solely in the user’s wallet. From there, it is presented to a verifier. As a consequence, every transmission of data is under the control of the user, and the presentation of data to a verifier always requires the active consent of the user.

**Further requirements** were strongly requested during the public consultation:
-	International compatibility, especially within the European Union
- The Swiss e-ID should be launched as soon as possible


## 3.2	Criteria
To structure discussions during the e-ID team’s knowledge-gathering activities, the following criteria were used: privacy preservation, security, maturity, ease of use, readiness for the future, interoperability & standardisation, operability & integration. The intention was to focus on the main differences between the possible approaches and not to create a complete comparison matrix. This already exists, for instance, as a [Credential Format Comparison](https://openwallet-foundation.github.io/credential-format-comparison-sig/) by [IDunion members](https://www.linkedin.com/posts/idunion_credential-format-comparison-and-idunion-ugcPost-7008024118574895104-Cu1d/) and was transferred to the Open Wallet Foundation. Given the fact that not every arbitrary combination of technologies is feasible, it became evident that a perfect “tech stack” does not exist.

The discussion around the following scenarios in Chapter 4 should help define the trade-offs we need to make. We can detect various aspects that are in conflict with each other, e.g., ensuring the highest levels of data security vs. keeping the product simple and user-friendly; having a high speed of implementation vs. ensuring full interoperability; protecting the user's privacy at its maximum vs. keeping system complexity down. 

There are no black-or-white answers to these questions, and a certain level of compromise in specific areas will be necessary. To offer a pathway forward, the proposed approaches point either in a more pragmatic, internationally harmonised direction or give more weight to privacy at the cost of using technologies that have lower familiarity and maturity in the domain of verifiable credentials.

# 4 Decision on technological scenarios
## 4.1 Introduction & challenges

In the field of self-sovereign identities, there are many initiatives developing different solutions. There are currently few globally acknowledged standards and systems in use; a dominant design has yet to establish itself. To realise the aspiration of an open, publicly used trust infrastructure and scenarios that use the e-ID and extend it with further value-adding services and credentials, it is necessary to define an initial technological framework for Switzerland.

The decision on the technologies will offer a certain degree of stability for the development teams providing the basic underlying infrastructure. Furthermore, those who intend to use the e-ID and its underlying trust infrastructure to independently verify and issue credentials will attain a certain degree of stability for their own software development. 
To satisfy this, the federal e-ID team wants to take a decision on the core technologies used for credential transmission and credential-related communication, as well as on the format of credentials, in early 2024.    

It is important to state that the concept of digital identities based on SSI is still young, and dynamic changes and further evolution will occur. The decision that will be taken should be understood as a **starting point** that is likely to **evolve**. The e-ID will not be a system developed once and then operated indefinitely without changes. Continual development and adaptation to new circumstances will be required to ensure long-term service continuity, security, and interoperability.

Based on what the e-ID project and development teams have learned, and what has implicitly been confirmed by the TAC, it is our assessment that none of the current technological frameworks or stacks fully comply with all the requirements set out in:
-	The parliamentarian motions
-	The feedback on the public consultations
-	The requirements of the e-ID team relating to the operation of a national infrastructure

The main points relating to this assessment were mentioned in Chapter 3.2. In summary, they relate to the technical areas of:
-	Privacy preservation (in particular how much tracking of a user's actions is prevented by the core technology)
-	Interoperability (will the selected technology be compatible with systems provided in other countries)
-	Maturity of the systems (in particular how much effort needs to be invested to run the system in a safe, stable, and usable manner for millions of users)

Nonetheless, by accepting certain compromises on these points, it will be feasible to provide an e-ID solution to the Swiss public. To come up with a pragmatic way forward, this discussion paper sets out two realistic scenarios and their benefits and drawbacks. Subsequently, the public discussion should provide the Confederation with an outside view of which scenario is initially preferred.

## 4.2 Scenario A: Following the EU's technical direction
_Scenario A_ would mean using a subset of the technologies proposed in the European Union's Architecture Reference Framework [ARF](https://digital-strategy.ec.europa.eu/en/library/european-digital-identity-wallet-architecture-and-reference-framework), with the aim of aligning closely with the EU's proposal for implementation. The following technologies would be used in particular: SD-JWTs for credentials and OID4VC/VP as an issuing and presentation protocol. 

The ARF provides a pragmatic technical approach to implementing digital identities and digital identity wallets. It is based on well-known and proven technologies and cryptography, and while the proposed communication protocols are new, they offer easy integration for ecosystem participants and transparency for software developers.

The EU’s indication that it wishes to use specific technologies has accelerated efforts in areas of technical standardisation. Given the independence of standardisation bodies and the ARF’s status as a work-in-progress, a certain level of uncertainty regarding the final outcome of the technical architecture will remain. As the ARF already contains multiple standards (SD-JWT and ISO-MDL), it is also possible that further technological fragmentation might occur.

Regarding privacy preservation, there have been concerns raised by technical communities, data protection officers, academia, and civil society since the publication of the ARF. Viewed in the context of Switzerland, the e-ID team’s analysis indicates that this criticism mainly relates to the topic of “unlinkability”. Without workaround implementations, such as the mass issuing of credentials for one-time use, the unlinkability of the used credentials cannot be guaranteed at present (see - [Annex Unlinkability](#a3unlinkability)). This is especially sensitive regarding credentials that are used regularly or are used to indicate the identity of a person.

Selective disclosure is possible, and users can consent to sharing subsets of the content of a verifiable credential. However, hashes of all underlying data contained in the credential are always transmitted during verification. This represents a kind of fingerprint.

The proposed solution for managing the revocation of credentials (Status List) is simple to implement and can enable offline verification (see -[Annex Status List](#status-list)). It can be argued that it has certain flaws regarding privacy preservation. Specific actors within the ecosystem can memorise a position in the list provided for revocation and then track any status change related to the credential (e.g. when it is revoked). No information relating to the content of a credential is stored within the revocation list, nor is the reason for the revocation. This tracking can only occur once the actor knows the position of the credential within the status list (disclosed during the presentation). In theory, alternative revocation mechanisms providing a higher level of privacy preservation could be used in _Scenario A_. However, since unlinkability of the credentials cannot be achieved, further examination to determine how large the gains in data protection are, seem appropriate. 

It is worth noting that Switzerland has an independent legal process, and _Scenario A_ does not mean a direct adoption of EIDAS 2.0 legislation or compatibility. The Architecture Reference Framework is considered a work in progress, and an evolution towards a more privacy-preserving approach is possible. An adoption of _Scenario A_ might lead to easier compatibility with the EU down the line. 

![Scenario_A](https://github.com/e-id-admin/open-source-community/assets/12694135/5dcce8b1-85d6-4fd4-a096-8da9d5d9842a)
*Illustration 2: Scenario A*

**Main benefits for a Swiss Trust Ecosystem:**
-	Potentially easier technical compatibility with the EU
-	Common technology and standardised cryptography
-	Rapid standardisation and further public knowledge gain due to EU efforts
-	Easy understanding and integration for service providers
-	Broad support for hardware-based cryptography

**Main drawbacks for a Swiss Trust Ecosystem:**
-	Potential linkability of the holder's credential use
-	Potential traceability through revocation
-	No predicates or zero knowledge proofs
-	Selective disclosure is only achieved in a partially data saving manner

**Technologies**
-	Credential Format: SD-JWT
-	Signatures: ECDSA or RSA
-	Communication Standards: OID4VC/VP
-	Revocation Service: Status List


**Risks & uncertainties**
-	The EU ARF is not considered to be final yet. To fully comply with the final technology used by the EU, Switzerland might need to delay its national e-ID project by an unknown amount of time.
-	There are concerns about privacy and traceability, but they have not been addressed yet. If public stakeholders do not accept this fact, trust in the system might be undermined.


## 4.3 Scenario B
_Scenario B_ would use a technology stack that aims to offer a higher level of privacy for holders in the ecosystem than that provided in _Scenario A_. At the current time, this would mean choosing an independent technological path for the Swiss trust infrastructure and the national e-ID.

The current hypothesis is that credentials would be issued based on JSON-LD-verifiable credentials using BBS+ signatures. With regard to the communication protocol, the verdict remains uncertain in this particular scenario. Both commonly used protocols in the SSI field (DIDcomm and OID4VP/VC) are compatible with the proposed credential format. DIDcomm boasts the benefit of end-to-end encryption, while OID4VC/VP offers easier integration and is based on standards to which various implementers are accustomed.

By using alternative but less common cryptography, credentials can be issued that enable holders to navigate the future trust ecosystem while leaving fewer digital traces behind. The properties regarding “unlinkability” (see - [Annex Unlinkability](#a3unlinkability)) are met by this technology stack. Credentials would use signature schemes that allow wallets to generate unique proofs for each verification. These proofs contain no constants and therefore do not link back to the originally issued credential. Given that the generation of the proofs is unique per verification, correlation between verifiers is made harder. Nonetheless, it is noteworthy that the disclosed content itself may allow linking and correlation.

As with the technology used in _Scenario A_, selective disclosure is also supported. It seems possible to achieve predicate proofs in BBS+ by using additional sub-protocols. The information available regarding this subject is comparatively limited and would need further scientific research.

When following _Scenario B_ with the goal of achieving non-correlation and unlinkability, further challenges arise that go beyond the presentation of credentials. In the context of _Scenario B_, privacy preserving revocation is another topic which would need to be addressed. Various possibilities could be implemented in order to reduce the traceability of credential usage through revocation services. These pivot around storing revocation data in a government service, delegating the generation of non-revocation proofs to holders, or using validity credentials. Without these efforts, actors can potentially gain knowledge about when a credential was used or when the status of a credential changed (e.g. became revoked). It is important to mention that there is not one solution which fits the needs of all credentials or ecosystem participants. Certain use cases might benefit from being able to track changes in credential status. Hence in _Scenario B_, multiple revocation mechanisms may come in to play.

Standardisation of [JSON-LD Credentials](https://www.w3.org/TR/vc-data-model-2.0/) and [BBS+](https://datatracker.ietf.org/doc/draft-irtf-cfrg-bbs-signatures/) is ongoing. No prediction can be made about when these standards will be considered final.

Deciding on _Scenario B_ initially would mean accepting a gap between the Swiss and EU implementation. Nonetheless in the future, the credential format and signature scheme proposed in this chapter, might be accepted by the EU, in the context of eIDAS 2.0 (relating to credentials that do not represent a person). The proposed _Scenario B_ might therefore still achieve EU conformity at a later point.

The team responsible for e-ID has ascertained that the technology employed in _Scenario B_, despite promising improved privacy preservation, exhibits a lower level of maturity compared to _Scenario A_, which could result in more effort and resources being required to provide the e-ID.      

![Scenario_BII](https://github.com/e-id-admin/open-source-community/assets/12694135/7de2c66d-dd7e-4351-acbd-6e68d5a9364e)
*Illustration 3: Scenario B*

**Main benefits for a Swiss Trust Ecosystem:**
-	Unlinkability for holders is provided
-	Correlation of holder data is made harder for colluding verifiers
-	Selective disclosure is achieved in a data sparing manner
-	Privacy preserving revocation might be achieved
-	Zero knowledge proofs and predicate proofs seem attainable
-	JSON-LD capabilities are gained (Automatization & User Experience)
-	Fallback to _Scenario A_ is possible

**Main drawbacks for a Swiss Trust Ecosystem:**
-	Initially no technical compatibility or additional effort needed to reach interoperability with the EU digital Identity
-	More complex integration and less known cryptography (full understanding of function only attainable by experts)
-	Security of proposed Scenario less researched
-	Smaller development community
-	Unclear support for hardware-based cryptography (Some providers claim to support BLS12-381)

**Technologies**
-	Credential Format: JSON-LD
-	Signatures: BBS+
-	Communication Standards: OID4VC/VP or DIDCOMM
-	Revocation Service: Open

**Risks & uncertainties**
-	Using less common technologies (e.g. a reduced number of available libraries) and the requirement to implement more complex mechanisms to enhance privacy could have an impact on the development timeline.
-	International adoption and therefore potential interoperability for Swiss e-ID holders remain unclear.
-	The privacy-preserving capabilities gained by using BBS+ signatures might be lost in the future due to post-quantum cryptography that may not support these.


## 4.4 Summary

In conclusion, we see that there are two potential paths forward that Switzerland can follow to offer a national e-ID. Both have their unique advantages and disadvantages. In a simplified manner, they can be summed up in the following way:

_Scenario A_: Commonly known cryptography is used. Technical interoperability with the EU should be achievable in a straightforward manner. A broad community, existing libraries and approved cryptography should simplify development. Regarding privacy, holders have full control over what content of a credential (selective disclosure) they share with which actor. Given that the credentials used have a constant footprint and the system for revocation does not offer full privacy preservation, holders can be tracked in an easier manner after presenting a credential to a verifier. Relying heavily on the EU ARF, especially since it is not a final version, might not offer the stability desired.

_Scenario B_: The cryptography used is less common and less extensively researched. Technical interoperability in the EU could prove more challenging. Nonetheless, it could still be achievable by converting the system or additionally issuing an eIDAS 2.0-conforming credential at a later stage. Given the higher aspiration for privacy preservation, implementations (e.g. revocation service) have the potential to become more complex. More effort might have to be invested in creating libraries and security attestation. Regarding privacy, holders have full control over what content of a credential (selective disclosure) they share with which actor. Given that more sophisticated cryptography is used and credentials have a dynamic footprint during each presentation, it should be harder to track a holder's actions after verification. If implementation fails, a fallback to _Scenario A_ is viable.

In order to provide a product which fulfils user expectations (privacy and interoperability), there is a high probability that in its final state the e-ID and the underlying infrastructure will have to cover both of the presented scenarios.

A practical approach could constitute initially implementing one of the aforementioned scenarios and, after successful introduction, extending the system to use additional technologies in parallel. This reaffirms that the decision should be viewed as the starting point for the productive infrastructure.

## 4.5 Unconsidered aspects

Because of the complexity of the SSI topic and the underlying technology, certain questions remain that are not covered by the proposed scenarios. While some of them can be tackled independently, others will require the decision to be taken to opt for one of the presented scenarios in order to allow a proper examination.

Aspects not currently considered in the scenarios:
-	Hardware/crypto-chip based wallet security
-	Base registry design and implementation
-	Trust registry design and implementation
-	Organisational identities and the process leading to registration in the base and trust registry
-	Decision on DID usage and DID Methods
-	The e-ID issuance process
-	Backup of verifiable credentials

# 5 Public discussion of this paper and further process
Now that we have reached this chapter, your opinion is needed!

To arrive at a well-informed decision, the e-ID team is seeking input and perspectives from a diverse array of public stakeholders. Please take the proposed scenarios as a basis for your feedback and consider that the final decision is a starting point for the medium-term technical development. The e-ID team is convinced that both variants cover the principal expectations for a verifiable credential trust ecosystem. From your standpoint, what characteristics and features are most valuable and must be achieved? Which trade-offs and risks are acceptable, why, and for how long?

**Cornerstones of the public discussion**
-	The public debate starts with the presentation of this paper at the “e-ID participation” meeting on 1 December 2023.
-	The written statements have to be entered in our feedback form at https://findmind.ch/c/XFvu-a7c6 by 3 January 2024. Please take a look at the form _before_ you start writing your feedback.
-	Written discussion can be held on GitHub: https://github.com/e-id-admin/open-source-community/discussions

**The main questions for your feedback**
-	Which scenario would you prefer? And for what reason?
-	Do both scenarios fulfil your expectations?
-	What major risks do you foresee?
-	Which “red lines” should not be crossed? Where is no compromise conceivable for you?

You can also contribute additional points in the written statements. Just consider that the current discussion does not need to cover the same topics mentioned in past public consultations (Zielbild E-ID, public consultation on the preliminary draft Act).
All responses will be published after the consultation period has expired, including indication of the official sender. 

For questions, shared thoughts, and requests for information in the case of any unclarity, use the public forum on [GitHub](https://github.com/e-id-admin/open-source-community/discussions). We value this public debate once again as a time for everybody (the e-ID team as well as the community) to expand their perspectives.

The various inputs will be considered, allowing the Confederation to define the initial technology stack (protocols, formats, and cryptography) early in 2024. With the decision, the foundations will be laid for building the Swiss trust infrastructure for verifiable credentials. The definition of detailed specifications expected from external parties for the future ecosystem can then be addressed. Realisation of the Swiss trust infrastructure will commence, with the aim of launching the e-ID in 2026.



# Annex

## A.1	Hands-on experience by the e-ID Team
### Proof of Concept ePerso
The ePerso is the first SSI proof of concept realized by the federal government. It is a digital employee card that states that the holder of the credential is an employee of the federal government. This proof of concept is built on the Hyperledger Indy Stack.

It was done to gain initial insights into the SSI ecosystem and to have a practical demonstration to show to external parties. It was observable that the participants in the proof of concept were able to understand the concept of verifiable credentials. The usability of the system was good, and most people were able to use it by themselves.  

There were findings related to the performance of the used cryptographic algorithms. Especially on the holder side, learnings were made about the performance of algorithms, e.g., mobile device hardware can be significantly slower than a server. It can be crucial that attributes (e.g., images) in a credential are optimized to be as small as possible, which can lead to certain restrictions regarding potential use cases for VCs. 

While the functionality Hyperledger Indy offers is good, it was detected that there are gaps regarding documentation and reproducibility for developers. 

### Public Trust Infrastructure Sandbox
The Public Trust Infrastructure Sandbox is a playground run by the federal government for organizations to test out SSI use cases on a system run by the Confederation. The sandbox included a base registry and an initial implementation of governance rules regulating write access to the system. The federal government did this project to support organizations to explore the SSI world and gain some first-hand insights about how the operation of a future trust infrastructure could work. The Public Trust Infrastructure Sandbox was also built with Hyperledger Indy.

A lesson learned from this initiative was that it is difficult to achieve interoperability with other implementations. Many standards, specifications, and implementations in the SSI field are still in a draft state, and many integrators do things differently. Additionaly it can be challenging to reach semantic interoperability. This can lead to incompatibility between the systems, even if they use the same underlying technology.
Another lesson learned was that the base registry most likely should only handle use case-agnostic governance. When aiming for a rapidly expanding trust ecosystem, the implementation of pre-emptive restrictions based on rules established by distinct use cases or sectors may be detrimental in this regard. 
Removing these requirements from base registries  simplifies implementation, as well as initial sign-up for ecosystem participants. It, however, emphasizes the significance of trust registries and sectorial governance. It will be crucial to offer users of the trust infrastructure a transparent trust registry implementation, aiding them in the assessment of the trustworthiness of their digital transactions.

Even though the base technology is not based on newer technological initiatives, there was interest in the Public Trust Infrastructure Sandbox. The Swiss SSI community was willing to invest effort to make their systems compatible with the sandbox. Open discussions were established (ecosystem roundtables) to experience how exchanges between system operators and ecosystem participants could work in the future.

### eLFA
The eLFA is the pilot project for the electronic learners' driver licence. This project will be the first developed by the confederation that will be used by users in the public. 

The primary goal is to explore how unbiased users adopt SSI-based use cases. The technologies SD-JWTs, OID4VC/VP and Status List are utilized, this reflects the mainstream ARF setup, as proposed in _Scenario A_. As this pilot is not live yet, the lessons learned are from the development stage of the project.

The complexity of the utilized technologies is lower compared to Hyperledger Indy. Additionally, reproducibility for developers is greatly increased. Given the technologies closer alignment to existing solutions in corporate IT environments, reaching conformance with internal governance is simplified.

## A.2 Specific learnings related to technologies
### Hyperledger Indy Stack
Hyperledger Indy was utilized by the federal e-ID Team for the pilot projects ePerso and the Public Sandbox Trust Infrastructure. 

The “privacy by design” features, especially regarding unlinkability and non-correlation, are rated highly. The technology is considered to be very advanced regarding the implementation of these principles. Zero-knowledge proofs as well as zero-knowledge predicates are supported.

A major benefit of Hyperledger Indy is that it offers bundles that allow developers to easily provide solutions for most of the roles required in a trust ecosystem (issuer, holder, verifier, and system provider). The underlying communication protocol supports asynchronous communication.

Since Hyperledger Indy is an “integrated stack” that also benefits from relatively easy setup and deployment, it constitutes a kind of “vendor” lock-in and does not offer a lot of modularity. These aspects were also reasons for its large adoption for pilot projects and test setups.

While the functionality Hyperledger Indy offers is good, it was detected that there are gaps regarding reproducibility for developers. The documentation of workflows is good, but how specific software components function is not always clear; encryption and signing protocols are not simple and not well known by the broader technical community. Furthermore, relating to performance and scalability of specific components for a national infrastructure, there were issues observed. The lack of crypto-agility is a further downside.  

Given the fact that DLT and blockchain-based solutions have grown out of favour in the public, a decline in the activity of the Hyperledger community can be detected, and effort is being directed towards alternative solutions.
The public consultation of the first e-ID pre-draft Act showed that a distributed operating model (utilizing nodes and trust anchors distributed over multiple organizations) was not requested. Based on these various findings, it was decided to exclude Hyperledger Indy from the presented scenarios.

### OpenID4VC/VP

This section contains the current experiences of the e-ID Team regarding the use of OID4VC/VP. It is important to note that the team has not implemented the entire standard but used specific components. Overall, it's an emerging standard, with multiple international stakeholders driving further evolution.

**Issuing:**
The credential is created and the content known by the issuer. It is then transmitted to the holder and can then be shared with verifiers by the holder. No other third parties are needed or involved in this process.
Credentials were issued in two formats by the development team:
* JWT (JSON Web Token): All the data is transparent and fully visible to the verifier upon presentation.
* SD-JWT (Selective Disclosure JWT): In the credential, the claims are represented exclusively as a hash. To transmit and disclose the information from holder to verifier, the holder needs to additionally send the plain text of the claims, they would like to disclose. The hash is utilized to check whether the plain text provided corresponds to the credential originally issued by the issuer. This disclosure mechanism enhances privacy since not all claims need to be sent in plain.

By default, credentials are immutable. Any alteration would invalidate the signature, making the credential void. Consequently, any modification in the data necessitates issuing a new credential, This requires a connection between the holder and the issuer. This is not specific to SD-JWTs, but can be considered in all situations where credentials are signed. Contrary to other communication protocols that support asynchronous messaging, the current OID implementations do not offer this capability. As a consequence, relying solely on protocols, that require strong holder initiation, might make use cases like, automatic renewal, re-issuance, etc. more difficult.

**Credential Structure**: Each credential comprises three distinct parts:
- **Technical Fields** which define whether a credential is valid or not (`nonce`, `issued_at`, `valid_until`, etc.)
- **Credential Body:** This section contains the actual subject related data of the credential.
- **Status List:** A reference to the component that indicates if a credential has been revoked.

**OpenID4VP (OpenID for Verifiable Presentations):** The implementation includes an enhanced security feature where hashed content is further secured by salting it with a random string. This approach renders rainbow tables ineffective, making it practicably impossible to deduce or guess the content, especially relevant if it's partially known or follows a predictable structure (e.g., the OASI/AHV-Number).

**Status lists:** The standards foresee that any entity can offer a status list service and the choice of service provider is at the discretion of the issuer. This can lead to scenarios where the issuer becomes aware of when and how often credentials are verified. From a data privacy perspective, this is not desirable in the Swiss trust infrastructure. Therefore, it is foreseen that a service will be provided by the Confederation.
A status list can be used for other purposes, not only revocation. For example, suspension of a credential is also supported.
The lists are an efficient method for storing and distributing information, as they can encompass numerous credentials, with each occupying just one _bit_ in the list.

**Trust registries:** The team detected no currently established standards or practical implementations for trust registries. 

###	Other Candidates
The following is a list of further technologies related to the field of SSI, which could turn out to be candidates for technical implementations and were reviewed by the e-ID Team. They merit mentioning, even if they do not play a role in proposed scenarios.

#### KERI 
KERI (Key Event Receipt Infrastructure) is a distributed key and identity management system that is sometimes proposed as a possible base layer for SSI systems. It does not rely on distributed ledgers but claims to provide similar security guarantees. KERI has not been used a lot in practice so far. GLEIF, the Global Legal Entity Identifier Foundation, is the first organization to have started using KERI.

An advantage of KERI is its clean slate approach, which does not rely on distributed ledgers or third-party software. While this is desirable from a design perspective, it may be troublesome when it comes to interoperability with other systems. An interesting concept in KERI is the pre-rotation of keys. When a private key is compromised, the controller simply starts using the next key to which it had cryptographically committed before. 

There are certain reasons why KERI currently is not an ideal match for the Swiss SSI endeavour. Community efforts are not yet distributed broadly, even though KERI was recently adopted by a Trust over IP (ToIP) working group. There is no distributed ledger (e.g., a blockchain) required to operate KERI, yet there is a complex network of witnesses, watchers, jurors, and judges that must play their roles to have secure and validated key events propagated from controllers to validators. The security guarantees of these networks currently remain unclear. Scientific research has not put KERI and its protocols under scrutiny yet. The scalability of the network with numerous event generators remains unclear.

KERI is agnostic to credential formats, signature schemas and communication protocols. The concepts of KERI might be considered while conceiving the base registry.

#### ACDC
ACDC (Authentic Chained Data Container) is a Verifiable Credential (VC) variant, designed to be used in the KERI ecosystem. ACDC provides proof-of-authorship for contained data through a linked chain/tree of credentials. Complex use cases around delegation of authority can be solved elegantly by the underlying data structure.

It addresses the topic of including disclaimers or “contracts” (not to be confused with smart contracts) in the credentials, formally binding parties to legal terms. ACDC is based on KERI identifiers. The first large-scale deployment using KERI/ACDC is GLEIF's vLEI.

##### GLEIF
The Global Legal Entity Identifier Foundation (GLEIF) is a supra-national not-for-profit organization headquartered in Basel, Switzerland. Established in June 2014, GLEIF is tasked to support the implementation and use of the Legal Entity Identifier (LEI). The foundation is backed and overseen by the Regulatory Oversight Committee, representing public authorities from around the globe.

LEIs are a mandatory identification means in the global financial industry. GLEIF ensures the operational integrity of the Global LEI System and manages a network of partners, known as the LEI issuing organizations. GLEIF targets automated authentication and verification of legal entities across a range of industries with _verifiable LEI_ (vLEI). vLEIs are a kind of verifiable credential identifying legal entities as well as their directors and officers with the possibility to perform cryptographically assured transactions.

GLEIFs ecosystem governance frameworks for LEI and vLEI are assets which could potentially be leveraged for the Swiss trust ecosystem. For example, the identification of organizations participating in the Swiss trust ecosystem could be done by using LEIs and vLEIs. GLEIF entertains an active project to map DIDs to LEIs, so that any public DID, e.g. used for credential issuance, can be tracked back to a legal entity identified by an LEI. It is planned to offer this as a service which can be used by other organizations.

Questions surrounding organisational identities (OID) and their trust anchors within the Swiss trust ecosystem will be tackled in future by the e-ID program. 

#### ISO/IEC 18013-5:2021, mDL, mDoc
ISO/IEC Standard 18013-5:2021 or commonly called mDL/mDoc is a further standard which is discussed as a potential solution surrounding digital identities and credentials. It was primarily developed for mobile Driver licences and released in 2021. It is mentioned as one of the formats for personal identification data in the EU ARF. There are already multiple internation implementations in production based on the standard. Smartphone operating systems have begun adopting the standard and integrating it in their platforms. For the realization of the Swiss mobile driver's this standard will be looked at in more detail.

## A.3	Unlinkability
As mentioned in chapter 3, “Design Principles” _privacy by design_ is a requirement that the confederation has been tasked with implementing in the future Swiss digital identity. The solution Switzerland adopts must adhere to high privacy standards in its architecture and the core technology used. Consultation with the Technical Advisory Circle as well as the hands-on experience gained by the development teams within the confederation have shown that different technologies lead to more or less traceability of holders within the ecosystem.

Linkability is a crucial concern in systems designed following the principle of “privacy by design”, which seek to embed privacy considerations directly into the architecture implemented and the core technology utilized. When discussing linkability, we refer to the potential for different sets of data, provided to an actor during separate events within the SSI ecosystem, to be connected to one another. 
Such connections are made possible through constant unique identifiers or patterns within the data that are not related to the actual content of the credentials (like name, surname, date of birth, or passport number) but are intrinsic to the data format itself.

The risk of linkability arises from the inclusion of certain fields or metadata that are essential for maintaining the integrity of a credential or enabling the verifier to fulfil their verification duties. However, even when these fields do not directly disclose personal information, they can still make an identity traceable if they remain constant across multiple presentations. It can be argued that this goes against the principles of privacy by design, as it creates a digital footprint that can be used to track and profile individuals. The optimum goal is to ensure that the verification process does not compromise an individual’s privacy by enabling their activities to be linked over time and across different contexts.

### SD-JWT
SD-JWTs (Selective Disclosure JSON Web Tokens) contain multiple fields which can make a credential trackable. 
Fields like `sub` (subject identifier), `jti` (JWT ID), `vc.id` (Verifiable Credential Identifier), selective disclosure hases `sd`, and a combination of `credentialStatus.id` with `credentialStatus.statusListIndex` are directly trackable unique identifiers.
The `sub` is a principal identifier for the subject of the JWT.
The `jti` is a unique identifier for the token, ensuring it's not reused.
The `vc.id` uniquely identifies the verifiable credential itself.
The `sd` the hashes of all selective disclosure values.
`credentialStatus.id` paired with `credentialStatus.statusListIndex` form a unique id as well that can identify one credential.
The packed JWT contains a non-changing signature, which is also trackable without even looking into the unpacked JWT.

#### Raw, packed SD-JWT

```json
{"format": "jwt_vc", "credential": "eyJhbGciOiAiRVM1MTIiLCAidHlwIjogInZjK3NkLWp3dCJ9.eyJpc3MiOiAiaHR0cHM6Ly9yZWdpc3RyeV9iYXNlL2lzc3Vlci9hMDY1Y2RiYi1hNTUxLTQyZWEtYTJlNy04NjI2OGI4NjQ2ZGQiLCAic3ViIjogImRpZDpqd2s6ZXlKamNuWWlPaUFpVUMwMU1qRWlMQ0FpZUNJNklDSldjR05DYmxkRk1HWmlibkJ4U21sMFkzZzVUVFp6YjJKTmVHNDRiMjEzWkRGdVlscGFlRTU0VldkNVdUWklhelJHV21WclptOWlRMU5JTjJScGFrWXhPWE5EWW1WSFIyRndiM1IwZEV4RU1GZEVaVXB3Ym5NaUxDQWllU0k2SUNKQlVYcE9jRkUwVTFsd2EzcERRMlpXTjNsZlNWVlJSbTFIY1ZkelRrSnROVkpoUjBSMWJqRlFlVWhCZGpoTVUzWmljRzl1VUZwSVdrVlNZMUpCZDBOUFNFSTRiR3M1VEc5M1IyRXhWV3RmU2t0MllVeFVTMWR0SWl3Z0ltdDBlU0k2SUNKRlF5SXNJQ0pyYVdRaU9pQWlVM3BHTlcxcFNHOWxSRzFMZDA5UFIzZDBOR0ZtWjA5RVJrZElUMkpwWDJsbWVuUTFjWFZGWjBaa2R5SjkiLCAiZXhwIjogMTcyOTI0MTM1NSwgImlhdCI6IDE2OTkwMDEzNTUsICJqdGkiOiAiNmU3NWM2MDctNTA5MS00NGNlLTkwYTMtMGYyOGE1Y2E2MTgzIiwgImNuZiI6IHsiandrIjogeyJrdHkiOiAiRUMiLCAia2lkIjogIlN6RjVtaUhvZURtS3dPT0d3dDRhZmdPREZHSE9iaV9pZnp0NXF1RWdGZHciLCAiY3J2IjogIlAtNTIxIiwgIngiOiAiVnBjQm5XRTBmYm5wcUppdGN4OU02c29iTXhuOG9td2QxbmJaWnhOeFVneVk2SGs0Rlpla2ZvYkNTSDdkaWpGMTlzQ2JlR0dhcG90dHRMRDBXRGVKcG5zIiwgInkiOiAiQVF6TnBRNFNZcGt6Q0NmVjd5X0lVUUZtR3FXc05CbTVSYUdEdW4xUHlIQXY4TFN2YnBvblBaSFpFUmNSQXdDT0hCOGxrOUxvd0dhMVVrX0pLdmFMVEtXbSJ9fSwgInZjIjogeyJpZCI6ICI2ZTc1YzYwNy01MDkxLTQ0Y2UtOTBhMy0wZjI4YTVjYTYxODMiLCAidHlwZSI6IFsiVmVyaWZpYWJsZUNyZWRlbnRpYWwiLCAiVW5pdmVyc2l0eURlZ3JlZUNyZWRlbnRpYWwiXSwgInZhbGlkRnJvbSI6ICIyMDIzLTExLTAzVDA4OjQ5OjE0LjgzMDM1NSIsICJ2YWxpZFVudGlsIjogIjIwMjQtMTAtMThUMDg6NDk6MTQuODMwMzY0IiwgImlzc3VlciI6ICJodHRwczovL3JlZ2lzdHJ5X2Jhc2UvaXNzdWVyL2EwNjVjZGJiLWE1NTEtNDJlYS1hMmU3LTg2MjY4Yjg2NDZkZCIsICJjcmVkZW50aWFsU3ViamVjdCI6IHsiZGVncmVlIjogeyJfc2QiOiBbIklBTXNBR3V0MlZVX0JnWkI2YzhJYUt1MHZJd3c5UnZuQjZfMENmLW1TUXciLCAiV01vVEZrWUluakZWcjZRREx6M3dBY09qdUdqby1OZXpIa0c4Mmc4SGxHdyIsICJ0Zkx1Y1FuSkJyY1N6M1FMNm9UQURWT096S1ZBcDJjWGJwWmVORlhjVVJnIl19fSwgImNyZWRlbnRpYWxTdGF0dXMiOiBbeyJpZCI6ICJodHRwczovL3JlZ2lzdHJ5X3Jldm9jYXRpb24vaXNzdWVyL2EwNjVjZGJiLWE1NTEtNDJlYS1hMmU3LTg2MjY4Yjg2NDZkZC9zdGF0dXMtbGlzdC82M2U5NGUzMy1hZTI2LTQ3MzYtODE5YS1mNDUxN2YwNzM1YWEjNiIsICJ0eXBlIjogIlN0YXR1c0xpc3QyMDIxRW50cnkiLCAic3RhdHVzUHVycG9zZSI6ICJyZXZvY2F0aW9uIiwgInN0YXR1c0xpc3RJbmRleCI6ICI2IiwgInN0YXR1c0xpc3RDcmVkZW50aWFsIjogImh0dHBzOi8vcmVnaXN0cnlfYmFzZS9pc3N1ZXIvYTA2NWNkYmItYTU1MS00MmVhLWEyZTctODYyNjhiODY0NmRkIn0sIHsiaWQiOiAiaHR0cHM6Ly9yZWdpc3RyeV9yZXZvY2F0aW9uL2lzc3Vlci9hMDY1Y2RiYi1hNTUxLTQyZWEtYTJlNy04NjI2OGI4NjQ2ZGQvc3RhdHVzLWxpc3QvYzAwOTBjNWItZmU5MC00Njc0LWJjMzQtMzE0MGNlZjJkNGU3IzYiLCAidHlwZSI6ICJTdGF0dXNMaXN0MjAyMUVudHJ5IiwgInN0YXR1c1B1cnBvc2UiOiAic3VzcGVuc2lvbiIsICJzdGF0dXNMaXN0SW5kZXgiOiAiNiIsICJzdGF0dXNMaXN0Q3JlZGVudGlhbCI6ICJodHRwczovL3JlZ2lzdHJ5X2Jhc2UvaXNzdWVyL2EwNjVjZGJiLWE1NTEtNDJlYS1hMmU3LTg2MjY4Yjg2NDZkZCJ9XX0sICJfc2RfYWxnIjogInNoYS0yNTYifQ.AHV0LUIjWFlq0lrv5Xc26MdHPkH9BYs55IhmPwyyS94x-glPQzaI97_roRyJUqGEzAJSYX0xOwDu9h-jU5pOnx1UAYBRy4Q3RTrIx1H92MTT_BfhQfUFW_HbVFVW-_qDv2mGROygha21B66Dav89ptw6wcAesPH4jLxvljg6bLlzkGYO~WyJ3UWFLNlgxWndfdGExdnp6Rk5ESF9nIiwgInR5cGUiLCAiTWFzdGVyRGVncmVlIl0~WyJBMHcyanpYeFRPWTUtT0Z6blNOWmFnIiwgIm5hbWUiLCAiTWFzdGVyIG9mIFNjaWVuY2UiXQ~WyJQcUEtaFp0Q3pNaTBGR3FadlNyVE93IiwgImF2ZXJhZ2VfZ3JhZGUiLCA0LjU5XQ~"}
```

#### Signature part of above packed SD-JWT

```
AHV0LUIjWFlq0lrv5Xc26MdHPkH9BYs55IhmPwyyS94x-glPQzaI97_roRyJUqGEzAJSYX0xOwDu9h-jU5pOnx1UAYBRy4Q3RTrIx1H92MTT_BfhQfUFW_HbVFVW-_qDv2mGROygha21B66Dav89ptw6wcAesPH4jLxvljg6bLlzkGYO~WyJ3UWFLNlgxWndfdGExdnp6Rk5ESF9nIiwgInR5cGUiLCAiTWFzdGVyRGVncmVlIl0~WyJBMHcyanpYeFRPWTUtT0Z6blNOWmFnIiwgIm5hbWUiLCAiTWFzdGVyIG9mIFNjaWVuY2UiXQ~WyJQcUEtaFp0Q3pNaTBGR3FadlNyVE93IiwgImF2ZXJhZ2VfZ3JhZGUiLCA0LjU5XQ~
```

#### Unpacked SD-JWT

```json
{
  "iss": "https://registry_base/issuer/a065cdbb-a551-42ea-a2e7-86268b8646dd",
  "sub": "did:jwk:eyJjcnYiOiAiUC01MjEiLCAieCI6ICJWcGNCbldFMGZibnBxSml0Y3g5TTZzb2JNeG44b213ZDFuYlpaeE54VWd5WTZIazRGWmVrZm9iQ1NIN2RpakYxOXNDYmVHR2Fwb3R0dExEMFdEZUpwbnMiLCAieSI6ICJBUXpOcFE0U1lwa3pDQ2ZWN3lfSVVRRm1HcVdzTkJtNVJhR0R1bjFQeUhBdjhMU3ZicG9uUFpIWkVSY1JBd0NPSEI4bGs5TG93R2ExVWtfSkt2YUxUS1dtIiwgImt0eSI6ICJFQyIsICJraWQiOiAiU3pGNW1pSG9lRG1Ld09PR3d0NGFmZ09ERkdIT2JpX2lmenQ1cXVFZ0ZkdyJ9",
  "exp": 1729241355,
  "iat": 1699001355,
  "jti": "6e75c607-5091-44ce-90a3-0f28a5ca6183",
  "cnf": {
    "jwk": {
      "kty": "EC",
      "kid": "SzF5miHoeDmKwOOGwt4afgODFGHObi_ifzt5quEgFdw",
      "crv": "P-521",
      "x": "VpcBnWE0fbnpqJitcx9M6sobMxn8omwd1nbZZxNxUgyY6Hk4FZekfobCSH7dijF19sCbeGGapotttLD0WDeJpns",
      "y": "AQzNpQ4SYpkzCCfV7y_IUQFmGqWsNBm5RaGDun1PyHAv8LSvbponPZHZERcRAwCOHB8lk9LowGa1Uk_JKvaLTKWm"
    }
  },
  "vc": {
    "id": "6e75c607-5091-44ce-90a3-0f28a5ca6183",
    "type": [
      "VerifiableCredential",
      "UniversityDegreeCredential"
    ],
    "validFrom": "2023-11-03T08:49:14.830355",
    "validUntil": "2024-10-18T08:49:14.830364",
    "issuer": "https://registry_base/issuer/a065cdbb-a551-42ea-a2e7-86268b8646dd",
    "credentialSubject": {
      "degree": {
        "_sd": [
          "IAMsAGut2VU_BgZB6c8IaKu0vIww9RvnB6_0Cf-mSQw",
          "WMoTFkYInjFVr6QDLz3wAcOjuGjo-NezHkG82g8HlGw",
          "tfLucQnJBrcSz3QL6oTADVOOzKVAp2cXbpZeNFXcURg"
        ]
      }
    },
    "credentialStatus": [
      {
        "id": "https://registry_revocation/issuer/a065cdbb-a551-42ea-a2e7-86268b8646dd/status-list/63e94e33-ae26-4736-819a-f4517f0735aa#6",
        "type": "StatusList2021Entry",
        "statusPurpose": "revocation",
        "statusListIndex": "6",
        "statusListCredential": "https://registry_base/issuer/a065cdbb-a551-42ea-a2e7-86268b8646dd"
      },
      {
        "id": "https://registry_revocation/issuer/a065cdbb-a551-42ea-a2e7-86268b8646dd/status-list/c0090c5b-fe90-4674-bc34-3140cef2d4e7#6",
        "type": "StatusList2021Entry",
        "statusPurpose": "suspension",
        "statusListIndex": "6",
        "statusListCredential": "https://registry_base/issuer/a065cdbb-a551-42ea-a2e7-86268b8646dd"
      }
    ]
  },
  "_sd_alg": "sha-256"
}
```

### BBS Signatures in W3C Verifiable Credentials Format 2.0
BBS+ is a signature scheme that allows for the generation of unlimited, unlinkable proofs from a single signed credential, enhancing privacy and preventing tracking. It achieves this through zero knowledge proofs, allowing users to prove they possess a credential without revealing the credential itself. It supports selective disclosure in a manner, where holders can choose which claims to disclose while revealing no-information about the undisclosed claims.

In the W3C verifiable credentials format, there are no trackable fields by default. In examples, the fields `id` and `identifier` often appear, which can be trackable, but they are not required for the credential to work. The content of `credentialSubject` is subject to selective disclosure and not covered by this assessment. The privacy preserving revocation mechanisms outlined in _Scenario B_ are not covered by this example, resulting in an identifiable `statusListIndex`.

#### W3C Verifiable Credentials Format 2.0 with BBS+ proof, original credential

```json
{
  "@context":[
    "https://www.w3.org/ns/credentials/v2",
    "https://w3id.org/citizenship/v1",
    "https://w3id.org/security/bbs/v1"
  ],
  "type":[
    "VerifiableCredential",
    "PermanentResidentCard"
  ],
  "name":"Permanent Resident Card",
  "description":"Government of Example Permanent Resident Card.",
  "issuer":"did:example:123456",
  "validFrom":"2019-12-03T12:19:52Z",
  "validUntil":"2029-12-03T12:19:52Z",
  "credentialSubject":{
    "id":"did:example:b34ca6cd37bbf23",
    "type":[
      "PermanentResident",
      "Person"
    ],
    "givenName":"Test",
    "familyName":"Test",
    "gender":"Male",
    "image":"data:image/png;base64,iVBORw0KGgokJggg==",
    "residentSince":"2015-01-01",
    "lprCategory":"C09",
    "lprNumber":"999-999-999",
    "commuterClassification":"C1",
    "birthCountry":"Test",
    "birthDate":"1999-12-31"
  },
  "credentialStatus":{
    "id":"https://example.com/credentials/status/3#94567",
    "type":"StatusList2021Entry",
    "statusPurpose":"revocation",
    "statusListIndex":"94567",
    "statusListCredential":"https://example.com/credentials/status/3"
  },
  "proof":{
    "type":"BbsBlsSignature2020",
    "created":"2023-11-03T13:38:41Z",
    "proofPurpose":"assertionMethod",
    "proofValue":"rTIKPje1bAPQizkqiBOcY0MsDFYMnIN8NPnR6FelI/l92GkZyRxI6BUS6U6X6mQNLMLCe4mnBgIFTTl4wfBUQLQHMT1lLG+TmH/s5XrAvj4AqXfqkY7p2DuXJQypP2ISqIFZwyOsELsV+4FAditp6w==",
    "verificationMethod":"did:example:123456#test"
  }
}
```

#### W3C Verifiable Credentials Format 2.0 with BBS+ proof on first presentation

```json
{
  "@context":[
    "https://www.w3.org/ns/credentials/v2",
    "https://w3id.org/citizenship/v1",
    "https://w3id.org/security/bbs/v1"
  ],
  "type":[
    "VerifiableCredential",
    "PermanentResidentCard"
  ],
  "issuer":"did:example:123456",
  "validFrom":"2019-12-03T12:19:52Z",
  "validUntil":"2029-12-03T12:19:52Z",
  "credentialSubject":{
    "id":"did:example:b34ca6cd37bbf23",
    "type":[
      "Person",
      "PermanentResident"
    ],
    "familyName":"Test",
    "givenName":"Test"
  },
  "credentialStatus":{
    "id":"https://example.com/credentials/status/3#94567",
    "type":"StatusList2021Entry",
    "statusPurpose":"revocation",
    "statusListIndex":"94567",
    "statusListCredential":"https://example.com/credentials/status/3"
  },
  "proof":{
    "type":"BbsBlsSignatureProof2020",
    "created":"2023-11-03T13:38:41Z",
    "nonce":"SR23bfW5BBuY71q7+k/VxZTxH4MKm7MtIFBRuYxOH+AwtQ9i8bBFZ0caSxRjP+Vzxg4=",
    "proofPurpose":"assertionMethod",
    "proofValue":"ABkB/wavqHfqsAGvain30kiAwA4tb90GkTM+YiX8eIXpCWQR2Jd1guTWTkNmVFa2NEmKBalwg2nRy5Q84rhRkHJtnEw//sAAcrzVYnNHRGqbnyZVuQ+KAIMoLExMDdj1t7KEGPOcoecn5TjWW4hRJ9UvulZD3yL2qacJHq2eacnxdXgRaQh/pOB33Ga5d4WOnon9fvASAAAAdIZQW1fL5ymfMX0BiuqewlJFAzl/hVyMHIjTobhfTBnOCPffhGYN8iyHPHSgsuTxpwAAAAJm/SbyQwF7uTsbk4JGMKY4UopPtaNhylK5h7e9UYiux134sWKbrurBdy+t5uBSsRVJrGiIx+soN1JQ5db8IiJ8lcSyS2jYFnm1/FefCUpaNmoZj9IyqZjge5NsDRb1gd7FRPxAo0iGQpLCFoZwi/YGAAAACkPGr0KC7p4umXcMATuMyPTL1M9e3yJOqrh9G4qxIHA0E6+fn6xevkCK4T9ErReSx3uxtKkzToCZ5wmm4JmP1MJRoiMRwdTeYmMbG+Z1tKgJs9LuEBKo4knibyEYnNoQCgb0r9PympTAvkhr2DrJCESJqRUtf9NtlFImJufCQ+r1VkXjtuBKI0e9ti2VANMxvXJ9FnOwTzLC/ZrCodVCZY8kAj1obH1LaFIrJor9akQ180TzVtSOr8+5VQmk4UC9k0Uf4JzqNK3fHYFP+mqQZ88S50FIqFdZxZldVVR/RQoHNPxuQ8sc23GEZSQvKLcXMgb0xXEEGT+sfL3qMY72rq4aTqpnsssn8fn6GjSXwjYOmgdeOdcGg7J4g+1E7HtAYy8XxKzsz+StuIgiLfMYRBZcrDHHixZ5mR92qu0kxlJC",
    "verificationMethod":"did:example:123456#test"
  }
}
```

#### W3C Verifiable Credentials Format 2.0 with BBS+ proof on second presentation

```json
{
  "@context":[
    "https://www.w3.org/ns/credentials/v2",
    "https://w3id.org/citizenship/v1",
    "https://w3id.org/security/bbs/v1"
  ],
  "type":[
    "VerifiableCredential",
    "PermanentResidentCard"
  ],
  "issuer":"did:example:123456",
  "validFrom":"2019-12-03T12:19:52Z",
  "validUntil":"2029-12-03T12:19:52Z",
  "credentialSubject":{
    "id":"did:example:b34ca6cd37bbf23",
    "type":[
      "Person",
      "PermanentResident"
    ],
    "familyName":"Test",
    "givenName":"Test"
  },
  "credentialStatus":{
    "id":"https://example.com/credentials/status/3#94567",
    "type":"StatusList2021Entry",
    "statusPurpose":"revocation",
    "statusListIndex":"94567",
    "statusListCredential":"https://example.com/credentials/status/3"
  },
  "proof":{
    "type":"BbsBlsSignatureProof2020",
    "created":"2023-11-03T13:38:41Z",
    "nonce":"6x/a6TQf0gmwazgP+EzU1+75Tl5tKxG2pzSvNJKm33pFs5uH3FofzchH1yzT4+qVqpU=",
    "proofPurpose":"assertionMethod",
    "proofValue":"ABkB/wavjuU/xA5CzTvmUYAE0FkYoYU/6ox3XZZfhPJsihw2zvuCsM0O8Ll2t/2AhIp95bwKjubtXSG3/6MtQYbI9H9jsfS+xeb8h5tKdX2sO/raA8zM1d/s72b/Zhy4sbkqkBvjhYpLDm65FbzJ4FEU0leZ5thyppeb8AUKpdkrKnoQ+IyomrttUr1pMHdp/xGgcoJmAAAAdKRqiPJIzdezmz8Th6lzTJ60nT+tNTJaqfV8W/EgiqsUhkacrKtF9MoCBUJbNinWPQAAAAJR2enEoMG8DUg7zuyhqW7UT7eBtENUQjY7dSNe/FRnOE2+/nQCynFbO7er1Mq0dA5QyvfzHuwJWSYmegL3j0hwkctJkI6MTjy79UGTcITx//9EE9ed25LJLvxeYU7yPsiSHExKW3Qa8JtqZyXWRxVeAAAACm2U5Fx2BrBYpTU6QGC4dz2Gfa6bTMKW9vg68dGA3vLnL61u2qCGhzE2gRPN8/4z4Dbku/nigvtRPrfCuwfvXudpgsEJmsbtQQ5wHSLWKlLGTs5pB7OrwE9jT52CLev08mAOP8J5jKBmsb/uCikapJwK/tBgkl3b9HpKivdECeoHDvduXAarh3j4xFsEh6/FFfQ3GVWE6pcqZ9YXCqWJwRBFDr26FXhiGXvc1kPE4vTaQfe/c2D0t5uuZom9Gz1q8F2CmUU7Hu+nbrOdZatUKHb24vLZM/Tghl3DbZ5GHbwjHZh+bqfTeZHoH2TewpgTR1911qONNcAexk6T/yVZ3G05jKWZW9WWSfNhp7CVDx/KqZe5zQOAtApOVP15SACh+gMW4fnhoO28B+XViuUzgZzkoeJeJnG04M9FuNhHocN2",
    "verificationMethod":"did:example:123456#test"
  }
}
```

### Summary on unlinkability in potential credential formats 

In summary, we can observe the following:
Looking at the different credential formats and the signature schemes they use (Anoncreds based on CL-Signatures, SD-JWTs based on ECDSA, and JsonLD VCs based on BBS+), we detect a difference regarding the digital traces generated while presenting credentials to verifiers. 
Simplified, we can currently categorize credential formats into two categories:

* **Linkable Credentials:** While presenting a credential to a verifier, a proof directly based on the initially issued credential is generated. This results in the same persistent technical footprint that is generated during each verification. This allows the linking or correlation of credential use over time, regardless of the actual credential data.

* **Unlinkable Credentials:** While presenting a credential to a verifier, by using more sophisticated cryptography, a unique proof per verification is generated. This results in a unique technical fingerprint that is generated dynamically for each verification. This means linking or correlating based on the signatures presented by the holders is impossible.

There are mitigation strategies for linkable credentials.
* **One-Use-Token:** There is the option to issue one-use-credentials, which holders only present once and then discard. This would require the issuer to create a new credential for each verification. While the issuer doesn't know the verifier, they would gain some information about the verification which is in direct conflict with the draft of the Act (issuer has no knowledge about the verification). This can itself be mitigated to a degree by issuing multiple tokens at once and replenishing them when needed, outside of verifications. This would be in conformance with the proposed Swiss Act. 

* **Cryptographic Blinding:** This is a generic method that can be employed with linkable credentials to conceal specific attributes or portions of the credential during the verification process. By selectively obscuring certain values, it ensures that each verification instance leaves a distinct, non-linkable technical trace. This approach could effectively prevent the correlation of credential usage over time, but would require further investigation before rollout in the productive system. However, caution is necessary to maintain a balance with holder binding protocols; over-blinding could mask critical elements required for affirming the authenticity of the credential holder's identity.

## A.4	Holder Binding
Within the field of digital identities, there are multiple trains of thought that aim at “connecting” the physical existence of a person to the artefact (e.g., a credential) representing them in the digital universe.

In the context of verifiable credentials and the e-ID, there are multiple questions that can be pondered when exploring the topic:
1. Is the person presenting the credential also the subject mentioned in it?
2. Is the person presenting the credential also the initial recipient of the credential?

While, in theory, encoding biometric data in credentials and then necessitating biometric matching during verification seems like a straight-forward solution to the first question, in practice there are many obstacles (security and implementation-related) as well as privacy considerations that make this an undesirable scenario. Additionally, enforcing this as a hard requirement in the ecosystem related to digital identities might not always be useful. Parents, acting on behalf of their children, might be legitimate holders of digital identities, not necessitating the subject of the credential (their child) to be involved in a digital transaction. The subject of credentials is also not always person-related, further negating a biometry-based mechanism. It therefore seems inevitable to forego this question and revisit it at a later stage if the need arises. 

More applicable approaches aim to solve the second question cryptographically. In the context of the Swiss e-ID project, the term “holder or device binding” is appropriate. In short, holder binding is a mechanism that aims to ensure that during verifications, the presenter of the credential is the same person who initially received the credential.

Holder binding is achieved by generating a private-public key pair (by the holder) and placing the public key information in the credential during the issuing process (done by the issuer). 
By utilizing this key pair during verification, verifiers can perform a “cryptographic challenge,” where the holder proves that they are also in possession of the corresponding private key referenced in the credential. 

In an ideal world, this is enough to prove the holder presenting the credential is also the initial recipient (since the public key was transmitted from the holder to the issuer during the initial issuing process). But, depending on where private keys are stored, they can be moved, copied to other devices, or stolen. To mitigate this, thought has to be put into how and where these key pairs are generated and stored.

In the current top-of-the-line smartphones, there is increasingly a so-called “secure element” (a separate microchip) designed specifically for security-relevant purposes. Generating and storing key material in these areas of phones ensures that portability becomes virtually impossible. The risk of theft by digital means is greatly reduced.

It is predictable that credentials with holder bindings, that are stored on a dedicated hardware chip, will lose their value after being restored on a different device (backup). The necessary private keys to prove legitimate possession will no longer be available because they were stored permanently on the first device. That being said, not all people eligible for an e-ID possess a phone with the mentioned capabilities. During the further development process, the implications regarding this will need to be examined in more detail.

The mechanism described aims to specifically identify the legitimate holder of a credential and can act as an additional data point for user tracing and therefore simplify correlation. If unlinkability is considered a critical requirement for the e-ID these two approaches might be in conflict with each other.

The e-ID team considers privacy preserving holder binding fairly uncharted terrain, which has not been researched extensively yet. To achieve privacy preserving holder binding, two implementations are imaginable. The proof of possession (private public key challenge) is generated dynamically, while still allowing cryptographic proof of the original key. Alternatively, selective disclosure could be utilized, and the necessary information only disclosed when a use case requires this.

Holder binding can be an important element to ensure trust in when an e-ID is used remotely. However, there are many situations where this is not necessary, neither for the e-ID nor for other credentials. Situations where this could occur are, when verifiers rely on other mechanisms to check if the holder is the legitimate subject of a credential. For instance, an in-person age check, where identification of the person is done visually.

## A.5	Revocation
Revocation is the process of invalidating a credential. It's not the same as a _valid until_ date in a credential that's past its expiration date. Not all credentials support revocation; it is up to the issuer to decide if a credential is revocable.

Revocation is a one-way process that cannot be reversed. It is worth mentioning here that this is an abstract definition (related to governance), not a technical one. Once revoked, a credential is considered invalid. The only way for holders to obtain a valid credential after that is to get a new one issued. The draft of the e-ID Act stipulates that issuers themselves are responsible for revoking their issued credentials.

To obtain information regarding the revocation state of a credential, a secondary source of information always needs to be consulted. 
There are various functional implementations (e.g. accumulators, status lists, validity credentials) and proving mechanisms (e.g. holder-initiated or verifier-initiated) to implement revocation. In addition, there are multiple approaches to where the information regarding revocation can be stored in an ecosystem: decentralized at the issuer or centrally at the entity operating  the trust infrastructure. 

The e-ID draft Act, postulates that issuers should not know  about the use of credentials issued by them, the Confederation will supply a system providing information related to revocation. By offering this, “calling home” to issuers during revocation checks can be prevented. No information relating to the content of a credential will be stored within this system, nor the reason for the revocation.

Revocation is an important element when considering the privacy preservation in trust ecosystems. As mentioned above, there are different storage locations within the ecosystem, as well as mechanisms to prove or check the revocation status of a credential. 

Depending on the flow of information different actors gain knowledge about the use of a credential.  

Use case-specific requirements will also come in to play. Verifiers can provide services which require recurring checks if credentials are still valid. In these situations, requiring holders to recurrently prove the validity of their credentials can be cumbersome and undesirable. 

### Status List
A status list is a file with a _bit_ per credential. Issuers can decide, which status lists they use to revoke credentials issued by them. Links to the corresponding status lists are directly encoded in credentials by the issuers. Issuers are free to use one status list for various credentials and credential types. Credentials issued, which use status lists, contain the information where revocation information is located and have an index at what position in the file the _bit_ for the credential is. A simplified explanation is: 0 means not revoked, 1 means revoked. 
Credentials can contain links to multiple status lists, enabling further capabilities regarding the validity status of a credential (e.g. suspension).
```
byte             0                  1           
bit       7 6 5 4 3 2 1 0    7 6 5 4 3 2 1 0   
         +-+-+-+-+-+-+-+-+  +-+-+-+-+-+-+-+-+  
values   |0|0|1|1|1|1|0|0|  |1|0|1|1|1|0|1|1|  
         +-+-+-+-+-+-+-+-+  +-+-+-+-+-+-+-+-+ 
index     7 6 5 4 3 2 1 0   15   ...  10 9 8  
```
To conceal the number of currently issued credentials by an issuer, a status list can be initialized with a random set of values already in place.

Verifier-initiated mechanism: Verifiers can download the concerned status list and then look up the concerned _index_ to perform a revocation check. The system providing the list does not gain any data from the verification process, apart from knowing a verifier downloaded a specific list. This mechanism enables caching- and offline capabilities. How often verifiers will want to retrieve these lists will depend on individual requirements/tolerance towards risks.
As soon as disclosed, the position within the list becomes a linkable element, allowing verifiers to trace changes in the status of a credential.

Holder-proven mechanism: Holders can consult a revocation service (e.g. a base registry), that stores the information relating to the status of the credential. If the credential is still considered valid, the system issues a “non-revocation proof”. During verification, in addition to the credential, the “non-revocation proof” is transmitted to the verifier. Holder-initiated checks avoid disclosing the position within the list to the verifier. However, the base registry gains information about the holders' use of a credential. In case this mechanism comes in to play, the type of credential, its content, who the verifier is, as well as the potential presentation scenario must remain unknown. Depending on how current verifiers require a “non revocation proof” to be, offline use is also conceivable.

### Accumulator
An Accumulator is a further mechanism, that enables a privacy protecting way to store revocation information. They're based on cryptographic accumulators, which enable the generation of non-revocation proofs based on arithmetic calculations. Instead of using a specific index/position in a status list, calculations are performed to generate the accumulator value (by verifiers) as well as the non revocation proof (by holders). 
While by their nature of not utilizing a static index, they offer potential for higher untraceability, they are also more complex than other revocation mechanisms and open questions remain regarding performance.
If _Scenario B_ is chosen, accumulators will need to be revisited in the design of a privacy preserving revocation mechanism. 

### Validity Credentials
Validity credentials are specifically issued credentials by the same party, that issued the initial credential, that prove the initial credentials' validity. The concept seems promising regarding data privacy, since the information can be stored in a decentralized manner by the holder. The issuer issues these on a recurring basis without knowledge of a presentation, no central revocation service is needed and the verifier does not receive any linkable information. Further exploration is needed to determine if it is a viable solution in the context of the e-ID, especially in _Scenario B_. Although additional traffic appears limited, the concept does require additional regular data flows from issuers to holders. During verification, both credentials would need to be transmitted form the holder to the verifier.

# References
- EU-Architectural Reference Framework (ARF):
https://digital-strategy.ec.europa.eu/en/library/european-digital-identity-wallet-architecture-and-reference-framework

- A GitHub thread that raised privacy concerns with regard to the EU-ARF:
https://github.com/eu-digital-identity-wallet/eudi-doc-architecture-and-reference-framework/issues/66

- Credential format comparison:
https://openwallet-foundation.github.io/credential-format-comparison-sig
And its preceding work:
https://docs.google.com/spreadsheets/d/1Z4cYfjbbE-rABcfC-xab8miocKLomivYMUFibOh9BVo
https://hackmd.io/ikhimdHLRy247YgRhNIXmw?both

- ETSI technical report comparing selective disclosure mechanisms, including unlinkability.
https://portal.etsi.org/webapp/WorkProgram/Report_WorkItem.asp?WKI_ID=67975

- OIX, mentioned in a TAC meeting, having a white paper for "interoperability across trust frameworks"
https://openidentityexchange.org/papers

- (Some recent) papers related to credentials and BBS+ Signatures published in the Cryptology ePrint Archive:
https://eprint.iacr.org/2023/275
https://eprint.iacr.org/2023/602
https://eprint.iacr.org/2023/1076

- Validity Credentials
https://eprint.iacr.org/2022/1658.pdf

- ToIP: Simplifying Protocol Selection
https://trustoverip.org/blog/2023/10/10/simplifying-the-selection-process-for-credential-issuance-and-presentation-protocols/

- Italy's EUDI Wallet documentation:
https://italia.github.io/eudi-wallet-it-docs/versione-corrente/en/index.html

- Architecture concept for German EUDI-Wallet (Bundesministeriums des Innern und für Heimat)
https://gitlab.opencode.de/bmi/eudi-wallet/eidas-2.0-architekturkonzept-v1

- Slides and meeting minutes from all technical advisory circle meetings
  - First meeting <br/>
https://github.com/e-id-admin/general/blob/main/meetings/20230921_TAC_Technical_Advisory_Circle_1.pdf <br/>
https://github.com/e-id-admin/general/blob/main/meetings/20230921_TAC_Meetingminutes.pdf
  - Second meeting <br/>
https://github.com/e-id-admin/general/blob/main/meetings/20231016_TAC_Technical_Advisory_Circle_2.pdf <br/>
https://github.com/e-id-admin/general/blob/main/meetings/20231016_TAC_Meetingminutes.pdf
  - Third meeting <br/>
https://github.com/e-id-admin/general/blob/main/meetings/20231109_TAC_Technical_Advisory_Circle_3.pdf <br/>
https://github.com/e-id-admin/general/blob/main/meetings/20231109_TAC_Meetingminutes.pdf

- Draft Act e-ID
  - German: https://www.newsd.admin.ch/newsd/message/attachments/84263.pdf
  - French: https://www.newsd.admin.ch/newsd/message/attachments/84264.pdf
  - Italian: https://www.newsd.admin.ch/newsd/message/attachments/84265.pdf

- Dispatch to the draft Act e-ID
  - German: https://www.newsd.admin.ch/newsd/message/attachments/84260.pdf
  - French: https://www.newsd.admin.ch/newsd/message/attachments/84261.pdf
  - Italian: https://www.newsd.admin.ch/newsd/message/attachments/84262.pdf

---


# Changelog
It is foreseen that the document remains as static as possible during the consultation period. To use the full potential of GitHub, amendments may be made during the consultation period. If new information or errors are discovered, the document can be modified. Any changes will be noted here accordingly. The changes will be visible as commits.

_01.12.2023_ - Initial Publication

_01.12.2023_ - Scenario B, illustration 3: fix wrong label in diagram for credential format
