# Swiss e-ID and trust infrastructure: Initial implementation

#### Table of contents
- [Current Situation](#Current-Situation)
- [Initial Technology](#Initial-Technology)
- [What happened leading up to this decision](#What-happened-leading-up-to-this-decision)
- [Changelog](#Changelog)

## Current Situation

In 2021, the Swiss federal government was mandated by parliament to start laying the groundwork for an electronic identity for people living in Switzerland and Swiss people living abroad. 

In parallel to drafting the e-ID Act, the Swiss federal government developed technical proof of concepts, established a broad community and co-developed a common vision for the future Swiss electronic identity (e-ID), a trust ecosystem enabling other third-party credentials and the underlying technical infrastructure. 

In case the e-ID act is adopted a timely introduction of the system is desired. To ensure this a decision on the technology initially utilized has to be taken now. 
As mentioned in previous publications by the Confederation, the field of digital identities based on concepts surrounding SSI is still young. 
Dynamic changes will occur, and further evolution of the technology used is expected.

## Initial Technology

As published in the [press release](https://www.admin.ch/gov/de/start/dokumentation/medienmitteilungen.msg-id-102922.html) on the 6th of December 2024 a selection has been made, which technology shall initially be used for the introduction of the e-ID and underlying trust infrastructure in Switzerland. 

It’s noteworthy, that this is to be understood as a starting point for the introduction of government issued digital identities and a trust ecosystem. Continuous evolution is expected and will be necessary to ensure long-term service continuity, security and interoperability. 

For the first release the following technologies will be supported:

| Category | Selection |  
| ---------|----------|
| Credential format | SD-JWT (used by fedpol for e-ID issuing and supported by the federal wallet) |
| Credential signature scheme | ECDSA – P256 |
| Hardware based holder binding enforcement | Yes |
| Identifiers as proposed by the e-ID Act | DID-Documents |
| Revocation mechanisms | Token Status Lists |
| Communication protocols | OID4VCI/VP |

*An updated more detailed list of supported technologies and references to more detailed specifications can be found under:*
[Tech Roadmap](./tech-roadmap.md)

The following subject areas were the main drivers in this choice:

### Timely delivery
A multitude of Swiss services await a government issued digital identity (e.g. in the areas of public sector login, digitalization in the healthcare, protection of youth and minors). Therefore, a delay in providing the e-ID is highly unfortunate. It is the assessment of the e-ID development program, that by using the selected technologies timely delivery is most feasible.

### Hardware holder binding and availability of cryptographic devices
Amendments made by the Swiss parliament to the  [e-ID Act](https://www.parlament.ch/de/ratsbetrieb/suche-curia-vista/ratsunterlagen?AffairId=20230073#Default=%7B%22k%22%3A%22PdAffairId%3A20230073%22%2C%22r%22%3A%5B%7B%22n%22%3A%22PdDoctypeDe%22%2C%22t%22%3A%5B%22%5C%22%C7%82%C7%824661686e65%5C%22%22%5D%2C%22o%22%3A%22and%22%2C%22k%22%3Afalse%2C%22m%22%3Anull%7D%5D%7D), postulate that during issuance a binding between holders and their e-ID must be ensured. This will be achieved by binding the digital credential to a device (with a cryptoprocessor) in control of the user during the issuance process. Given the current landscape of mobile devices, hardware binding functionalities are limited. Only ECDSA signatures are commonly available as hardware-backed keys on Android and iOS devices. 
While available in theory, currently the Confederation is not in possession of implementations, which combine existing hardware backed keys and unlinkable proof mechanisms that have been independently vetted and have a proven secure and performant usage over significant numbers of people. 

### Unlinkability
The Confederation recognizes the legitimate demand for unlinkability and aims to provide an e-ID  with this capability as soon as it is safely possible. To achieve this, a dedicated team with specific resources for research will be allocated to foster advancements in the specific area. In addition the e-ID Program intends to collaborate with the existing e-ID community to advance the topic.  

### Multi-Stack
The Confederation remains committed to providing a trust infrastructure which can support multiple technologies in parallel - specifically if they: 
1.  enable better privacy preserving properties than the status quo or 
2.  improve international interoperability.

Where possible, foundational choices are made that enable usage of multiple formats and cryptographic signatures.
The utilized DID Method for public identifiers shall be agnostic to the utilized cryptographic scheme. 
The underlying communication protocol is independent of the credential format transmitted. 

To ensure easy interoperability within the initial ecosystem an interoperability profile [(Swiss Profile)](https://github.com/e-id-admin/open-source-community/blob/main/tech-roadmap/swiss-profile.md) is published. This currently covers the technology as initially chosen for the e-ID and underlying trust infrastructure. It currently focusses on the Public Beta and will evolve with the provided infrastructure over time.  

## What happened leading up to this decision

In order to answer the question<br> 
  _"What should the __initial__ technical basis for this e-ID, trust ecosystem and underlying trust infrastructure be?"_
<br>the Confederation conducted various activities:
- A technical advisory circle (TAC) met in late 2023 and provided the Confederation with insight regarding this question. 
Slides available [here](https://github.com/e-id-admin/general/tree/d8ec463895cc4c59d7b7dc522856e61c0ce62a2b/meetings)
- A [discussion paper](https://github.com/e-id-admin/open-source-community/blob/main/discussion-paper-tech-proposal/discussion-paper-tech-proposal.md) was published by the Confederation outlining possible scenarios which could be utilized 

- An informal consultation was held. The feedback received is published [here](https://github.com/e-id-admin/open-source-community/tree/main/discussion-paper-tech-proposal) 
- Internal evaluation of feedback and scenarios was conducted
- A [press release](https://www.bj.admin.ch/bj/en/home/aktuell/mm.msg-id-101414.html) regarding further clarifications needed was published



## Changelog

_06.12.2024_ - Initial Publication
