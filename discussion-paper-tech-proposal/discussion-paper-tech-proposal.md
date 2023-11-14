🚧 THIS IS A DRAFT AND MAY BE OBJECT TO CHANGES. OFFICIAL PUBLICATION ON DEC 1, 2023 🚧


# Discussion Paper: Initial technological basis for the Swiss national trust infrastructure 

## Defining a common ground for the Swiss electronic identity and other verifiable credentials



# 1 Purpose of the document
In 2021, the Swiss federal government was mandated by parliament to start laying the groundwork for an electronic Identity for inhabitants of Switzerland and Swiss people living abroad.
In parallel to the law-writing process over the last two years, the Swiss federal government built technical proof of concepts, established a broad community and developed a common vision for the future Swiss electronic identity (eID), a trust ecosystem and the underlying technical infrastructure. Now the confederation is facing an important question: What should the initial technical basis of this vision be?

By involving the broader community, a discussion on two potential and realistic scenarios described in this document shall be enabled. This document aims to create a synthesized overview of the current knowledge established within the federal government and brings up multiple aspects that may favorize the one or the other kind of technical realization for the architecture of a national trust infrastructure. The benefits and drawbacks are collected, particularly regarding communication and exchange protocols, credential and data formats, and cryptography used for verifiable credentials. 

These presented scenarios are meant to stimulate public discussion and provide the eID Team with external perspectives on potential ways forward. Even though the content of this document has a technical focus, political and societal aspects will be drivers for a decision. Depending on the public's voice and the assessments of the presented scenarios, the eID Team will take a decision for a starting point to build the technical foundation of the national trust infrastructure.

# 2 Background
## 2.1  Vision of an eID and a Trust Infrastructure for Verifiable Credentials
The main driver for an eID is the expressed political will of six parliamentary groups that Switzerland should possess a state-issued and operated eID. The rejection of the previous proposal for a national e-ID law in 2021 resulted in six [political motions](https://www.parlament.ch/de/ratsbetrieb/suche-curia-vista/geschaeft?AffairId=20213124), requiring the future eID to follow the principles of privacy by design, data minimization and decentralized data storage.

In consequence, a public discussion was held to define the scope of the eID and its system, which resulted in the following decisions:
- A new law-making project was launched.
- The law shall be technologically neutral, but the general architecture and roles shall follow the principles of self-sovereign identity (SSI).
- Ambition Level 3 shall be reached, i.e., not only state-issued credentials like the eID shall be supported, but any other kind of digital credential can be realized by third parties with the provided infrastructure.
- The state shall be responsible for the issuance of the eID and for the operation of the necessary systems needed for the trust infrastructure (base registry, trust registry, wallet, check-app, standardization of channels and formats).
The detailed steps of the law-making project and additional information are published on the FOJ website: https://www.bj.admin.ch/bj/de/home/staat/gesetzgebung/staatliche-e-id.html


## 2.2	EU eIDAS 2.0 and the Architecture Reference Framework
In June 2021, the EU published a law proposal that is similar to the ambitions of Switzerland.On the technical side, the EUDI Wallet Architecture and Reference Framework (ARF)  was published in January 2023 with the goal of developing an interoperable digital identity wallet solution in the EU. The ARF defines an initial direction for the technical implementation of the eIDAS 2.0 regulation. The current understanding of the E-ID Project Team is that the ARF is considered an initial Version and will evolve further over time. 

The ARF offers an approach based on well-known cryptography and technology that is being rapidly standardized. This enables a technically pragmatic way of offering digital identities. It is our assessment that the outlined technologies in the ARF offer a realistic pathway to providing digital identities, and implementation for integrators is transparent and fairly straightforward.

It is worth noting that the new law proposal as well as the technology proposed by the ARF do come with downsides. The legislation is currently under criticism by various members of academia, civil society and data privacy organizations ([Open Letter Pre-Trilogue Conclusion](https://epicenter.works/en/content/eidas-open-letter-pre-trilogue-conclusion-300-academics-19-ngos)), requesting that unlinkability be mandated by the legislation. The technology as proposed in the ARF offers a high conformance with existing security practices yet has very specific flaws concerning certain aspects of privacy preservation. Namely, the credentials contain a unique technical footprint that is transmitted to verifiers during each verification. This can lead to easier traceability of a holder’s actions, as well as easier collusion if verifiers combine data. 

As Switzerland is not a member of the EU, the eID project team relies on information made available to the public and provided by the broader eID community.

## 2.3	Practical experience and external Expertise (TAC) collected by the confederation
Establishing a nationwide trust infrastructure is a complex undertaking that will take several years to complete. Therefore, having a solid technical foundation is of prime importance. This foundation must be based on sound technical principles and have broad acceptance on a technical, political and societal level. To get the necessary technical insights, it was decided that the confederation would start building small-scale systems and gain hands-on experience deploying and operating products based on SSI-Technology. Hence, the projects ePerso (electronic employee ID card), the "Public Sandbox Trust Infrastructure" (implementation of a base registry), and eLFA (mobile learners' driver license) were launched. The expertise of the federal E-ID Team, as well as most of the knowledge contributing to this document, was vastly gained from these projects. 

To further benefit from the vast knowledge and network of the Swiss eID Community, a technical advisory circle was formed with the goal of further advising and informing the confederation. The project team would like to take the chance to express its deep gratitude to the members of the TAC for their involvement in the process leading up to now.  
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
The parliamentary motions - asking for a state-issued and state-operated electronic identity (eID) - instructed the federal council to provide an eID that follows the principles of privacy by design, data minimization and decentralized data storage.

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
During the knowledge gathering activities of the eID team, the following criteria helped to structure the discussion: Privacy preservation, security, maturity, ease of use, readiness for the future, interoperability & standardization, operability & integration. The intention was to bring up the main differences between the thinkable approaches and not to create a complete comparison matrix, as it was already done for the [Credential Format Comparison](https://openwallet-foundation.github.io/credential-format-comparison-sig/) by [IDunion members](https://www.linkedin.com/posts/idunion_credential-format-comparison-and-idunion-ugcPost-7008024118574895104-Cu1d/) transfered to the Open Wallet Foundation. Also knowing that not every arbitrary combination is possible, it became clear that a perfect "tech stack" there does not exist.

The discussion around the following scenarios shall help defining the trade-offs we need to make. Diverse aspects are in tension with each other, e.g. having high data security and keeping the product simple, userfriendly and convenient; having a high speed of implementation and assuring the full interoperability; protecting user's privacy at a maximum and keeping the system complexity low. Also there is no black-and-white answer, the proposed approaches point either in a more pragmatic, internationally harmonized direction or give more weight to privacy for the cost of lower maturity.


# 4 Proposed scenarios
// TODO

## 4.1 Introduction & Challenges

## 4.2 Scenario A: Following the EU's technical direction

## 4.3 Scenario B:

## 4.4 Unregarded Aspects

# 5. Public discussion of this paper and further process
Now, having reached this chapter: Your voice is needed! 

The eID team is looking for a legitimate decision, where opinions from a broad interested public are taken into account. Please take the proposed scenarios as a basis for your feedback and consider that the final decision is a starting point for the medium-term technical development. The eID team is convinced that both variants cover the principal expectations for a verifiable credential trust ecosystem. But from your point of view, what characteristics and features are the most valuable and should absolutely be achieved? Which trade-offs and risks are acceptable, why and for how long?

**Cornerstones of the public discussion**
- The public debate starts with the presentation of this paper at the participation meeting December 1, 2023.
- The written Statements have be entered into our feedback form at [www.findmind.ch/...](https://www.findmind.ch/...) until 03. January 2024. Hint: Have a look at this form before you start writing your feedback.
- Written discussion can be held on GitHub: https://github.com/e-id-admin/open-source-community/discussions

**The main questions for your feedback**
- According to you: Which scenarios fulfil your expectations?
- Which scenario would you prefer? And for what reason?
- Which "red lines" shall not be crossed, where is no compromise thinkable for you?
- What risks do you consider acceptable for the Swiss trust infrastructure?

Your can also contribute additional points in the written statements. Just consider that the current discussion does not need to name again every already mentionned aspect from the past public consultations (Zielbild E-ID, public consultation on the preliminary draft law).

For questions, shared thoughts and request for information on unclear elements use the public discussion on [GitHub](https://github.com/e-id-admin/open-source-community/discussions). The public debate is once again the time for expanding one's perspective, thinking more broadly.

The various inputs will be taken into account and allow then the eID-Program to define the initial technology stack (protocols, formats, cryptography) early 2024. The decision will be taken by the program comissioning body (Programmauftraggeber). With the decision, the fundamentals to start building the swiss trust infrastructure for verifiable credentials are set. Thereafter the definition of detail specification expected from external parties of the common future ecosystem will be adressed. And the realization of the swiss trust infrastructure can start according to the planned schedule with the aim of launching the eID in 2026. 


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
