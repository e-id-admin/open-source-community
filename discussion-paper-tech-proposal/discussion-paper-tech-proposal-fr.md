[Original version english](https://github.com/e-id-admin/open-source-community/blob/main/discussion-paper-tech-proposal/discussion-paper-tech-proposal.md) - [Deutsche Version](https://github.com/e-id-admin/open-source-community/blob/main/discussion-paper-tech-proposal/discussion-paper-tech-proposal-de.md)

# Document de travail : Base technologique initiale pour l'infrastructure de confiance suisse

### Définition d'une base commune pour l'identité électronique suisse et d'autres moyens de preuves électroniques

#### Table des matières
- [Objet du document](#1-Objet-du-document)
- [Contexte](#2-Contexte)
- [Principes de conception et critères](#3-Principes-de-conception-et-critères)
- [Décision sur les scénarios technologiques](#4-Décision-sur-les-scénarios-technologiques)
- [Débat public sur le présent document et suite du processus](#5-d%C3%A9bat-public-sur-le-pr%C3%A9sent-document-et-suite-du-processus)
- [Annexe](#Annexe)
- [Références](#Références)
- [Changelog](#Changelog)

# 1 Objet du document
En 2021, le gouvernement fédéral suisse a été mandaté par le Parlement pour commencer à poser les bases d'une identité électronique pour les personnes vivant en Suisse et les Suisses vivant à l'étranger. Parallèlement à la rédaction de la loi au cours des deux dernières années, le gouvernement fédéral suisse a obtenu des preuves techniques de concepts, établi une large communauté et développé une vision commune pour la future identité électronique suisse (e-ID), un écosystème de confiance permettant de mettre en œuvre d'autres moyens de preuves électroniques de tiers et l'infrastructure technique sous-jacente.

> La Confédération est maintenant confrontée à une question importante : <br />
  Quelle devrait être la base technique __initiale__ de cette vision ?

En impliquant la communauté au sens large, il est prévu de mener une discussion concernant deux scénarios potentiels et réalistes décrits dans le présent document. Ce document vise à donner un aperçu synthétique des connaissances actuelles établies au sein du gouvernement fédéral et à mettre en évidence les multiples aspects qui peuvent favoriser l'un ou l'autre type de solution technique pour l'architecture d'une infrastructure de confiance nationale. Les avantages et les inconvénients sont exposés, notamment en ce qui concerne les protocoles de communication et d'échange, les formats de donnée des moyens de preuves électroniques et la cryptographie utilisée pour les moyens de preuves électroniques.

Les scénarios présentés ont pour but de stimuler le débat public et de fournir à l'équipe du projet e-ID des perspectives externes sur les voies possibles à suivre. Bien que le contenu de ce document soit axé sur la technique, les aspects politiques et sociétaux seront des éléments déterminants pour la prise de décision. Sur la base de la réaction du public et des évaluations des scénarios présentés, la Confédération décidera d'un point de départ pour jeter les fondations techniques de l'infrastructure de confiance nationale.

# 2 Contexte
## 2.1 Vision d'une e-ID et d'une infrastructure de confiance pour des moyens de preuves électroniques

Le principal moteur de l'e-ID est la volonté politique exprimée par six groupes parlementaires de doter la Suisse d'une e-ID émise et gérée par l'État. Le rejet aux urnes en 2021 du projet de loi précédent a donné lieu à six [motions politiques](https://www.parlament.ch/de/ratsbetrieb/suche-curia-vista/geschaeft?AffairId=20213124), exigeant que la future e-ID respecte les principes de la protection de la vie privée dès la conception, de la minimisation des données et du stockage décentralisé des données.

En conséquence, un débat public a été lancé pour définir le concept de l'e-ID et de son système. Les décisions suivantes ont été prises à l'issue de ce débat :

- Un nouveau projet législatif a été lancé.
- Le projet de loi doit être technologiquement neutre, mais l'architecture générale et les rôles suivront les principes de l'identité souveraine (SSI).
- Le niveau d'ambition 3 doit être atteint ; ce qui permettra non seulement à l'Etat mais également aux tiers, tels les prestaires de services privés, d'établir des moyens de preuves électroniques grâce l'infrastructure de confiance envisagée.
- L'État est responsable de l'établissement de l'e-ID et de l'exploitation des systèmes nécessaires à l'infrastructure de confiance (registre de base, registre de confiance, portefeuille électronique, l'application de vérification, normalisation des canaux et des formats).
- Les étapes détaillées du projet législatif et des informations complémentaires sont disponibles sur le site de l'OFJ : https://www.bj.admin.ch/bj/de/home/staat/gesetzgebung/staatliche-e-id.html


## 2.2 EU eIDAS 2.0 et le cadre de référence de l'architecture
En juin 2021, l'UE a publié un projet de loi dont les ambitions sont similaires à celles de la Suisse. L'architecture et le cadre de référence (Architecture Reference Framework, ARF) de l'EUDI Wallet ont été publiés en janvier 2023 dans le but de développer une solution de portefeuille d'identité numérique interopérable dans l'UE. L'ARF définit une orientation initiale pour la mise en œuvre technique du règlement eIDAS 2.0. Selon l'équipe du projet e-ID, l'ARF est considéré comme une version initiale qui évoluera au fil du temps.

L'ARF propose une approche basée sur une solution cryptographique et technologique bien connue, qui fait l'objet d'une normalisation rapide. Il s'agit d'un moyen techniquement pragmatique d'offrir des identités numériques. L’équipe du projet e-ID estime que les technologies décrites dans l'ARF offrent une approche réaliste pour fournir des identités numériques, et que la mise en œuvre pour les responsables de l'intérgation est transparente et relativement simple.

Il convient de noter que le projet de loi et la technologie proposée par l'ARF de l'UE présentent des inconvénients. La législation a été critiquée par divers experts issus du milieu universitaire, de la société civile et des organisations de protection de la vie privée ([Open Letter Pre-Trilogue Conclusion](https://epicenter.works/en/content/eidas-open-letter-pre-trilogue-conclusion-300-academics-19-ngos)), qui ont demandé que la législation exige prévoit la dissociabilité (voir [Annexe Dissociabilité](https://github.com/e-id-admin/open-source-community/blob/main/discussion-paper-tech-proposal/discussion-paper-tech-proposal.md#a3unlinkability)). La technologie proposée dans l'ARF offre un niveau élevé de conformité avec les pratiques de sécurité existantes, mais présente des failles très spécifiques concernant certains aspects de la préservation de la vie privée. En particulier, les titres contiennent une empreinte technique unique qui est transmise aux vérificateurs lors de chaque vérification. Cela peut faciliter la traçabilité des actions d'un titulaire et la collusion si les vérificateurs combinent les données.

La Suisse n'étant pas membre de l'UE, l'équipe du projet e-ID s'appuie sur les informations mises à la disposition du public et fournies par la communauté e-ID au sens large.

## 2.3 Expérience pratique et expertise externe recueillies par la Confédération
La mise en place d'une infrastructure de confiance à l'échelle nationale est un effort complexe qui prendra plusieurs années. Il est donc primordial de disposer d'une base technique solide. Cette base doit reposer sur des principes techniques fiables et être largement acceptée sur les plans technique et politique ainsi que par le public. Pour développer les capacités techniques nécessaires, il a été décidé que la Confédération commencerait à créer des systèmes à petite échelle et à acquérir une expérience pratique du déploiement et de l'exploitation de produits basés sur la technologie SSI. C'est ainsi qu'ont été lancés les projets ePerso (carte d'identité électronique des employés), "Public Sandbox Trust Infrastructure" (mise en œuvre d'un registre de base) et eLFA (permis d'élève conducteur électronique). L'expertise de l'équipe du projet e-ID, ainsi que la plupart des connaissances contribuant à ce document de discussion, ont été largement acquises dans le cadre de ces projets.

Afin de bénéficier davantage des vastes connaissances et du réseau de la communauté suisse de l'e-ID, un cercle consultatif technique (Technical Advisory Circle, TAC) a été créé dans le but de conseiller la Confédération. L'équipe du projet e-ID souhaite profiter de cette occasion pour remercier sincèrement les membres du TAC pour leur participation dans les travaux jusqu'à présent.

Dans la perspective de la publication du présent document, le TAC a tenu trois réunions, au cours desquelles il a examiné divers aspects des défis généraux, répondu à des questions spécifiques et envisagé des solutions et des scénarios potentiels pour la fourniture de l'e-ID suisse. Il convient de noter que les membres du TAC ont des expériences diverses et que les discussions ne débouchent pas toujours sur un consensus. Néanmoins, les réunions offrent une excellente mesure de l'état actuel du domaine SSI.

Points intéressants soulevés lors des réunions du TAC:
- Discussions sur la maturité des solutions technologiques
- Manque de preuves scientifiques approfondies concernant certaines technologies
- Défis associés au stockage décentralisé des informations d'identification
- Viabilité future de la cryptographie utilisée (cryptographie post-quantique)
- Questions de protection de la vie privée liées à la traçabilité des moyens de preuves électroniques (impossibilité de les relier)
- Questions de protection de la vie privée concernant la traçabilité de la révocation des moyens de preuves électroniques 
- Stockage sécurisé des informations d'identification et des clés cryptographiques
- Scénarios technologiques potentiels et approche multi-technologique

Les présentations utilisées comme base de discussion et les comptes rendus des réunions du TAC sont publiés au lien suivant:
https://github.com/e-id-admin/general/tree/main/meetings


# 3 Principes de conception et critères
## 3.1 Principes de conception
Les motions parlementaires – demandant une identité électronique (e-ID) émise et gérée par l'État – ont chargé le Conseil fédéral de fournir une e-ID qui respecte les principes de la protection de la vie privée dès la conception, de la minimisation des données et du stockage décentralisé des données.

Une architecture de base a été définie et les rôles et les principes essentiels ont été intégrés dans le [projet de loi](https://www.newsd.admin.ch/newsd/message/attachments/84263.pdf) et expliqués dans le [message](https://www.newsd.admin.ch/newsd/message/attachments/84260.pdf).

![swiss_trust_diamond](https://github.com/e-id-admin/open-source-community/assets/12694135/3fe1b206-7563-403a-ac92-be9e28d582e6)
*Illustration 1 : Infrastructure de confiance suisse*

**La protection de la vie privée dès la conception** est abordée par différents moyens :
- La loi sur l'e-ID suit les principes SSI avec trois rôles différents : les émetteurs, les titulaires et les vérificateurs. Ces rôles communiquent directement entre eux.
- L'émetteur ne sait pas si ou comment les moyens de preuves électroniques qu'il a délivré sont utilisées. La loi sur l'e-ID définit ce principe de préservation de la vie privée.
- Le système doit protéger la vie privée par défaut. La loi sur l'e-ID étant neutre sur le plan technologique, elle ne précise pas ce que cela signifie exactement.

**La minimisation des données** est abordée par différents moyens :
- Un moyen de preuve électronique vérifiable peut être présenté dans son intégralité ou seulement en partie (divulgation sélective) ;
- Si possible, les informations peuvent être présentées sous forme de dérivation (preuve par information dérivée ; par exemple, "a plus de 18 ans") ou sous forme de "preuve à divulgation nulle de connaissance" (Zero Knowledge Proof, ZKP).
- Le registre de base central et nécessaire ne contient que le minimum de données concernant les émetteurs et les vérificateurs enregistrés ou relatives aux moyens de preuves électroniques révoqués. Il n'y a pas de traces de données relatives aux moyens de preuves électroniques délivrés individuellement qui soient stockées de manière centralisée. La loi sur l'e-ID définit les informations contenues dans le registre de base.
- Les flux de données inutiles sont évités grâce au paradigme SSI (par exemple en utilisant la communication directe entre les acteurs concernés).

**Le stockage décentralisé des données** est réalisé en stockant l'e-ID et d'autres moyens de preuves électroniques uniquement dans le portefeuille électronique de l'utilisateur. De là, ils sont présentés à un vérificateur. Par conséquent, chaque transmission de données est sous le contrôle de l'utilisateur, et la présentation des données à un vérificateur nécessite toujours le consentement actif de l'utilisateur.

**Des exigences supplémentaires** ont été vivement demandées lors de la consultation publique :
- Compatibilité internationale, en particulier au sein de l'Union européenne
- L'e-ID suisse devrait être lancée dès que possible.


## 3.2 Critères
Les critères suivants ont été utilisés pour structurer les discussions lors des activités d'acquisition de connaissances de l'équipe du projet e-ID : préservation de la vie privée, sécurité, maturité, facilité d'utilisation, viabilité future, interopérabilité et normalisation, opérabilité et intégration. L'intention était de se concentrer sur les principales différences entre les approches possibles et non de créer une matrice de comparaison complète. Celle-ci existe déjà, par exemple, sous la forme d'une [Comparaison des formats de justificatifs](https://openwallet-foundation.github.io/credential-format-comparison-sig/) réalisée par les [membres de l'IDunion](https://www.linkedin.com/posts/idunion_credential-format-comparison-and-idunion-ugcPost-7008024118574895104-Cu1d/) et a été transférée à l'Open Wallet Foundation. Étant donné que toutes les combinaisons arbitraires de technologies ne sont pas réalisables, il est devenu évident qu'il n'existe pas de "pile technologique" (tech stack) parfaite.

La discussion autour des scénarios suivants au chapitre 4 devrait aider à définir les compromis qui doivent être faits. Nous pouvons détecter différents aspects qui sont en conflit les uns avec les autres, par exemple : assurer les niveaux les plus élevés de sécurité des données contre garder le produit simple et convivial ; avoir une vitesse de mise en œuvre élevée contre assurer une interopérabilité complète ; protéger au maximum la vie privée de l'utilisateur contre maintenir la complexité du système à un niveau bas. 

Il n'y a pas de réponses vraiment tranchées à ces questions, et un certain niveau de compromis dans des domaines spécifiques sera nécessaire. Afin d'offrir une voie à suivre, les approches proposées s'orientent soit vers une direction plus pragmatique et harmonisée au niveau international, soit vers un renforcement de la protection de la vie privée au prix de l'utilisation de technologies moins connues et moins matures dans le domaine des moyens de preuves électroniques.

# 4 Décision sur les scénarios technologiques
## 4.1 Introduction et défis

Dans le domaine des identités souveraines, de nombreuses initiatives développent différentes solutions. Il existe actuellement peu de normes et de systèmes reconnus au niveau mondial ; un modèle dominant doit encore s'imposer. Pour concrétiser l'aspiration à une infrastructure de confiance ouverte, utilisée par le public, et à des scénarios qui recourent à l'e-ID et l'étendent à d'autres services et d'autres moyens de preuves électroniques à valeur ajoutée, il est nécessaire de définir un cadre technologique de base pour la Suisse.

La décision sur les technologies offrira un certain degré de stabilité aux équipes de développement qui fournissent l'infrastructure de base. En outre, ceux qui ont l'intention d'utiliser l'e-ID et son infrastructure de confiance sous-jacente pour vérifier et délivrer des moyens de preuves électroniques de manière indépendante bénéficieront d'un certain degré de stabilité pour le développement de leurs propres logiciels. 
Pour ce faire, au début de l'année 2024 la Confédération souhaite prendre une décision sur les technologies de base utilisées pour la transmission et la communication ainsi que sur le format des moyens de preuves électroniques.

Il est important de préciser que le concept d'identité électronique fondé sur le SSI est encore jeune, et que des changements dynamiques et une évolution ultérieure suivront. La décision qui sera prise doit être considérée comme un **point de départ** susceptible d'**évoluer**. L'e-ID ne sera pas un système développé une seule fois et exploité indéfiniment sans changement. Un développement continu et une adaptation à de nouvelles circonstances seront nécessaires pour assurer la continuité du service, la sécurité et l'interopérabilité à long terme.

Sur la base de ce que les équipes du projet e-ID ont appris, et de ce qui a été implicitement confirmé par le TAC, nous n'estimons qu'aucun des cadres ou des piles technologiques (tech stacks) actuels ne satisfait pleinement à toutes les exigences énoncées dans :
- les motions parlementaires
- les avis soumis lors des consultations publiques
- les exigences de l'équipe du projet e-ID relatives à l'exploitation d'une infrastructure nationale.

Les principaux points relatifs à cette évaluation ont été mentionnés au chapitre 3.2. En résumé, ils concernent les domaines techniques suivants :
- Préservation de la vie privée (en particulier, dans quelle mesure la technologie de base empêche -t-elle le suivi des actions de l'utilisateur ?)
- Interopérabilité (la technologie choisie sera-t-elle compatible avec les systèmes d'autres pays ?)
- Maturité des systèmes (en particulier, quels efforts doivent être déployés afin que le système puisse fonctionner de manière sûre et stable, et puisse être utilisé par des millions d'utilisateurs ?).

Néanmoins, en acceptant certains compromis sur ces points, il sera possible de fournir une solution d'identification électronique au public suisse. Afin de trouver une solution pragmatique, le présent document de discussion présente deux scénarios réalistes ainsi que leurs avantages et leurs inconvénients. Par la suite, le débat public devrait permettre à la Confédération d'obtenir un avis extérieur sur le scénario à privilégier dans un premier temps.

## 4.2 Scénario A : Suivre l'orientation technique de l'UE
Le scénario A consisterait à utiliser un sous-ensemble des technologies proposées dans le cadre de référence de l'architecture [ARF](https://digital-strategy.ec.europa.eu/en/library/european-digital-identity-wallet-architecture-and-reference-framework) de l'Union européenne, dans le but de s'aligner étroitement sur la proposition de mise en œuvre de l'UE. Les technologies suivantes seraient notamment utilisées : SD-JWTs pour les informations d'identification et OID4VCI/VP comme protocole d'émission et de présentation.

L'ARF propose une approche technique pragmatique de la mise en œuvre des identités numériques et des portefeuilles d'identité numérique. Il repose sur des solution technologiques et cryptographiques connues et éprouvées, et même que les protocoles de communication proposés sont nouveaux, ils offrent une intégration facile pour les participants de l'écosystème et une transparence pour les développeurs de logiciels.

Le fait que l'UE ait indiqué qu'elle souhaitait utiliser des technologies spécifiques a accéléré les efforts dans les domaines de la normalisation technique. Étant donné l'indépendance des organismes de normalisation et le statut de l'ARF en tant que travail en cours, un certain niveau d'incertitude subsistera quant au résultat final de l'architecture technique. Comme l'ARF contient déjà plusieurs normes (SD-JWT et ISO-MDL), il est également possible qu'une nouvelle fragmentation technologique se produise.

En ce qui concerne la préservation de la vie privée, les communautés techniques, les responsables de la protection des données, le monde universitaire et la société civile ont exprimé des inquiétudes depuis la publication de l'ARF. Dans le contexte de la Suisse, l'analyse de l'équipe du projet e-ID indique que ces critiques portent principalement sur la question de la "dissociabilité". En l'absence de solutions de contournement, telles que l'émission en masse d'identifiants à usage unique, la dissociabilité des identifiants utilisés ne peut être garantie à l'heure actuelle (voir [Annexe Dissociabilité](https://github.com/e-id-admin/open-source-community/blob/main/discussion-paper-tech-proposal/discussion-paper-tech-proposal.md#a3unlinkability)). Cette question est particulièrement sensible pour les données d'identification utilisées régulièrement ou pour indiquer l'identité d'une personne.

La divulgation sélective est possible et les utilisateurs peuvent consentir à partager des sous-ensembles du contenu d'un moyen de preuve électronique vérifiable. Cependant, les hachages de toutes les données contenues dans le moyen de preuve électronique sont toujours transmis lors de la vérification. Il s'agit d'une sorte d'empreinte digitale.

La solution proposée pour gérer la révocation des moyens de preuves électroniques (Status List) est facile à mettre en œuvre et peut permettre une vérification hors ligne (voir [Annexe Status List](https://github.com/e-id-admin/open-source-community/blob/main/discussion-paper-tech-proposal/discussion-paper-tech-proposal.md#status-list)). On peut faire valoir qu'elle présente certaines faiblesses en ce qui concerne la préservation de la vie privée. Des acteurs spécifiques de l'écosystème peuvent mémoriser une position dans la liste prévue pour la révocation et suivre ensuite tout changement de statut lié au moyen de preuve électronique (par exemple, lorsqu'il est révoqué). Aucune information relative au contenu d'un moyen de preuve électronique ou à raison de la révocation n'est stockée dans la liste de révocation. Ce suivi ne peut avoir lieu que lorsque l'acteur connaît la position du moyen de preuve électronique dans la liste de statut (divulguée lors de la présentation). En théorie, d'autres mécanismes de révocation offrant un niveau plus élevé de préservation de la vie privée pourraient être utilisés dans le _Scénario A_. Toutefois, étant donné qu'il n'est pas possible de dissocier les moyens de preuves électroniques, il semble approprié de procéder à un examen plus approfondi afin de déterminer l'ampleur des gains en matière de protection des données. 

N'étant pas membre de l'UE, la Suisse ne doit pas automatiquement reprende le droit européen. Ainsi, l'adoption du_Scénario A_ ne signifie pas que la compatibilité du droit suisse avec le règlement eIDAS modifié doit être assurée dès son entrée en vigueur. En outre, les travaux relatifs au ARF ne sont pas achevés, et une évolution vers une approche plus respectueuse de la vie privée est possible. L'adoption du _Scénario A_ pourrait faciliter la compatibilité avec l'UE à l'avenir. 

![Scenario_A](https://github.com/e-id-admin/open-source-community/assets/12694135/5dcce8b1-85d6-4fd4-a096-8da9d5d9842a)
*Illustration 2 : Scénario A*

**Principaux avantages pour l'écosystème de confiance suisse**
- Compatibilité technique avec l'UE potentiellement plus facile à mettre en œuvre 
- Solution technologique répandue et solution cryptographique normées 
- Normalisation rapide et amélioration des connaissances du public grâce aux efforts de l'UE
- Facilité de compréhension et d'intégration pour les prestataires de services
- Large soutien à la cryptographie basée sur le matériel

**Principaux inconvénients pour l’écosystème de confiance suisse**
- Possibilité de relier l'utilisation des moyens de preuves électroniques par le titulaire
- Traçabilité potentielle au travers de la révocation
- Pas d'informations dérivées ou de preuves à divulgation nulle de connaissance
- La divulgation sélective ne peut pas être mise en œuvre en respectant la confidentialité des données maximale.

**Technologies**
- Format des moyens de preuves électroniques : SD-JWT
- Signatures : ECDSA ou RSA
- Normes de communication : OID4VC
- Service de révocation : Status List


**Risques et incertitudes**
- L'ARF de l'UE n'est pas encore considéré comme définitif. Pour se conformer pleinement à la technologie finale utilisée par l'UE, la Suisse pourrait devoir prolonger son projet national d'e-ID d'une durée indéterminée.
- Les questions de confidentialité et de traçabilité suscitent des inquiétudes, mais elles n'ont pas encore été abordées. Cette situation pourrait ébranler la confiance dans le système.


## 4.3 Scénario B
Le _Scénario B_ utiliserait une pile technologique (tech stack) visant à offrir aux détenteurs de l'écosystème un niveau de confidentialité plus élevé que celui prévu dans le _Scénario A_. À l'heure actuelle, une voie technologique indépendante devrait être choisie pour mettre en œuvre l'infrastructure de confiance suisse et l'e-ID nationale selon ce scénario.

Les moyens de preuves électroniques seraient émis sur la base de JSON-LD Verifiable Credentials utilisant des signatures BBS+. En ce qui concerne le protocole de communication, le verdict reste incertain dans ce scénario particulier. Les deux protocoles couramment utilisés dans le domaine SSI (DIDcomm et OID4VC) sont compatibles avec le format de moyen de preuve électronique proposé. DIDcomm présente l'avantage d'un cryptage de bout en bout, tandis que OID4VC offre une intégration plus facile et repose sur des normes auxquelles les différents utilisateurs sont habitués.

En utilisant une cryptographie alternative mais moins courante, il est possible de délivrer des moyens de preuve électroniques qui permettent à leurs titulaires de naviguer dans le futur écosystème de confiance tout en laissant moins de traces numériques. Les propriétés relatives à la "dissociabilité" (voir [Annexe Dissociabilité](#a3 dissociabilité)) sont respectées par cette pile technologique (tech stack). Les moyens de preuves électroniques utiliseraient des schémas de signature permettant aux portefeuilles de générer des preuves uniques pour chaque vérification. Ces preuves ne contiennent pas de constantes et ne renvoient donc pas au titre émis à l'origine. Étant donné que la génération des preuves est unique pour chaque vérification, la corrélation entre les vérificateurs est plus difficile. Néanmoins, il convient de noter que le contenu divulgué lui-même peut permettre l'établissement d'un lien et d'une corrélation.

Comme pour la technologie utilisée dans le _Scénario A_, la divulgation sélective est également possible. Il semble possible de réaliser des preuves d'informations dérivées dans BBS+ en utilisant des sous-protocoles supplémentaires. Les informations disponibles à ce sujet sont relativement limitées et nécessiteraient des recherches scientifiques supplémentaires.

Lorsque l'on suit le _Scénario B_ dans le but de parvenir à la non-corrélation et à la non-liaison, d'autres défis se posent, qui vont au-delà de la présentation des moyens de preuves électroniques. Dans le contexte du _Scénario B_, la révocation préservant la vie privée est un autre sujet qui devrait être abordé. Diverses possibilités pourraient être mises en œuvre pour réduire la traçabilité de l'utilisation des titres par le biais de services de révocation. Ces possibilités s'articulent autour du stockage des données de révocation dans le système d'un service gouvernemental, de la délégation de la production de preuves de non-révocation aux titulaires (non-revocation proof), ou de l'utilisation de justificatifs de validité (validity credentials). Sans ces efforts, les acteurs peuvent potentiellement savoir quand un moyen de preuve électronique a été utilisé ou quand le statut d'un titre a changé (par exemple, il a été révoqué). Il est important de mentionner qu'il n'existe pas de solution unique qui réponde aux besoins de tous les moyens de preuves électroniques ou de tous les participants à l'écosystème. Certains cas d'utilisation pourraient bénéficier de la possibilité de suivre les changements de statut des moyens de preuves électroniques. C'est pourquoi, selon le _Scénario B_, plusieurs mécanismes de révocation peuvent entrer en jeu.

La normalisation de [JSON-LD Credentials](https://www.w3.org/TR/vc-data-model-2.0/) et de [BBS+](https://datatracker.ietf.org/doc/draft-irtf-cfrg-bbs-signatures/) est en cours. Il est impossible de prédire quand ces normes seront considérées comme définitives.

En optant dans un premier temps pour le _Scénario B_, il faudrait accepter un décalage entre la mise en œuvre en Suisse et dans l'UE. Néanmoins, à l'avenir, le format des moyens de preuves électroniques et le système de signature proposés au présent chapitre pourraient être acceptés par l'UE, dans le contexte d'eIDAS 2.0 (concernant les moyens de preuves électroniques qui ne représentent pas une personne). La conformité du_Scénario B_ pourrait être assurée ultérieurement.
L'équipe du projet e-ID a constaté que la technologie employée dans le cadre du_Scénario B_ offre une meilleure préservation de la vie privée, mais présente un niveau de maturité inférieur à celui du _Scénario A_, ce qui pourrait nécessiter plus d'efforts et de ressources pour fournir l'e-ID. 

![Scenario_BII](https://github.com/e-id-admin/open-source-community/assets/12694135/7de2c66d-dd7e-4351-acbd-6e68d5a9364e)
*Illustration 3 : Scénario B*

**Principaux avantages pour un écosystème de confiance suisse :**
- La dissociabilité pour les titulaires est assurée
- La corrélation des données des détenteurs est rendue plus difficile pour les vérificateurs qui coopèrent entre eux (connivence)
- La divulgation sélective est réalisée avec les données minimales
- La révocation préservant la vie privée peut être réalisée
- Les preuves à divulgation nulle de connaissance et les preuves d'informations dérivées semblent réalisables.
- Les capacités JSON-LD sont acquises (automatisation et expérience utilisateur)
- Le retour au _Scénario A_ est possible

**Principaux inconvénients pour un écosystème de confiance suisse :**
- Au départ, pas de compatibilité technique ou d'effort supplémentaire pour atteindre l'interopérabilité avec l'identité numérique de l'UE
- Intégration plus complexe et cryptographie moins connue (la compréhension complète de la fonction n'est accessible qu'aux experts)
- La sécurité du scénario proposé a fait l'objet de moins de recherches
- Communauté de développement plus restreinte
- Prise en charge imprécise de la cryptographie matérielle (certains fournisseurs affirment prendre en charge la norme BLS12-381)

**Technologies**
- Format des moyens de preuves électroniques : JSON-LD
- Signatures : BBS+
- Normes de communication : OID4VC ou DIDCOMM
- Service de révocation : Ouvert

**Risques et incertitudes**
- L'utilisation de technologies moins courantes (par exemple, un nombre réduit de bibliothèques disponibles) et l'obligation de mettre en œuvre des mécanismes plus complexes pour renforcer la protection de la vie privée pourraient avoir un impact sur le l'échéancier de développement.
- L'adoption internationale et donc l'interopérabilité potentielle pour les détenteurs d'une d'identité électronique suisse restent incertaines.
- Les capacités de préservation de la vie privée acquises grâce à l'utilisation des signatures BBS+ pourraient être perdues à l'avenir en raison de la cryptographie post-quantique qui pourrait ne pas les prendre en charge.


## 4.4 Résumé

En conclusion, nous constatons qu'il existe deux voies potentielles que la Suisse peut suivre pour offrir une e-ID nationale. Toutes deux présentent des avantages et des inconvénients. De manière simplifiée, elles peuvent être résumées comme suit :

Scénario A : La solution repose sur une cryptographie répandue. L'interopérabilité technique avec l'UE devrait pouvoir être réalisée facilement. Une large communauté, des bibliothèques existantes et une cryptographie approuvée devraient simplifier le développement. En ce qui concerne le respect de la vie privée, les détenteurs ont un contrôle total par rapport au contenu d'un moyen de preuve électronique (divulgation sélective) qu'ils partagent et à l'acteur avec lequel ils décident de le partager. Étant donné que les moyens de preuves électroniques utilisés ont une empreinte constante et que le système de révocation n'offre pas une préservation totale de la vie privée, les détenteurs peuvent être suivis plus facilement après avoir présenté un moyen de preuve électronique à un vérificateur. Le fait de s'appuyer fortement sur l'ARF de l'UE, d'autant plus qu'il ne s'agit pas d'une version finale, pourrait ne pas offrir la stabilité souhaitée.

Scénario B : La cryptographie utilisée est moins courante et a été moins recherchée que celle du _Scénario A_. L'interopérabilité technique dans l'UE pourrait s'avérer plus difficile. Néanmoins, il serait possible d'y parvenir en convertissant le système ou en délivrant ultérieurement un moyen de preuve électronique conforme à eIDAS 2.0. Étant donné les aspirations plus élevées en matière de préservation de la vie privée, les mises en œuvre (par exemple le service de révocation) pourraient devenir plus complexes. Il faudra peut-être consacrer plus d'efforts à la création de bibliothèques et à l'attestation de sécurité. En ce qui concerne la protection de la vie privée, les détenteurs ont un contrôle total par rapport au contenu d'un moyen de preuve électronique (divulgation sélective) qu'ils partagent et à l'acteur avec lequel ils décident de le partager. Étant donné qu'une solution cryptographique plus sophistiquée est utilisée et que les titres ont une empreinte dynamique lors de chaque présentation, il devrait être plus difficile de suivre les actions d'un détenteur après la vérification. En cas d'échec de la mise en œuvre, il est possible de revenir au _Scénario A_.

Afin de fournir un produit qui réponde aux attentes des utilisateurs (respect de la vie privée et interopérabilité), il est fort probable que, dans sa version finale, l'e-ID et l'infrastructure sous-jacente devront couvrir les deux scénarios présentés.

Une approche pratique pourrait consister à mettre en œuvre dans un premier temps l'un des scénarios susmentionnés et, après une introduction réussie, à étendre le système pour utiliser parallèlement d'autres technologies. Il est donc clair que la décision doit être considérée comme le point de départ de l'infrastructure productive.

## 4.5 Aspects non pris en compte

En raison de la complexité du sujet SSI et de la technologie sous-jacente, certaines questions restent en suspens et ne sont pas couvertes par les scénarios proposés. Si certaines d'entre elles peuvent être abordées indépendamment, d'autres nécessiteront la décision d'opter pour l'un des scénarios présentés afin de permettre un examen adéquat.

Aspects non pris en compte actuellement dans les scénarios :
- Sécurité des portefeuilles basée sur le matériel/la puce cryptographique
- Conception et mise en œuvre du registre de base
- Conception et mise en œuvre du registre de confiance
- Identités organisationnelles et processus d'enregistrement dans le registre de base et le registre de confiance
- Décision sur l'utilisation de la DID et les méthodes de DID
- Processus d'émission de l'e-ID
- Sauvegarde des informations d'identification vérifiables

# 5 Débat public sur le présent document et suite du processus
Maintenant que nous avons atteint ce chapitre, nous avons besoin de votre avis !

Pour parvenir à une décision éclairée, l'équipe du projet e-ID sollicite les commentaires et les points de vue d'un large éventail d'acteurs. Veuillez utiliser les scénarios proposés comme base pour vos commentaires et considérer que la décision finale est un point de départ pour le développement technique à moyen terme. L'équipe du projet e-ID est convaincue que les deux variantes répondent aux principales attentes par rapport à un écosystème de confiance de moyens de preuves électroniques. De votre point de vue, quelles sont les caractéristiques et les fonctionnalités les plus précieuses et qui doivent être réalisées ? Quels compromis et quels risques sont acceptables, pourquoi et pour combien de temps ?

**Les pierres angulaires du débat public**
- Le débat public commence par la présentation de ce document lors de la séance "réunion participative" du 1er décembre 2023.
- Les déclarations écrites doivent être saisies dans notre formulaire de retour d'information à l'adresse https://findmind.ch/c/XFvu-a7c6 jusqu’au 15 janvier 2024. Veuillez consulter le formulaire _avant_ de commencer à rédiger vos commentaires.
- Les discussions écrites peuvent avoir lieu sur GitHub : https://github.com/e-id-admin/open-source-community/discussions

**Les principales questions sur lesquelles nous aimerions avoir votre avis**
- Quel scénario préférerez-vous ? Et pour quelle raison ?
- Les deux scénarios répondent-ils à vos attentes ?
- Quels risques majeurs prévoyez-vous ?
- Quelles sont les "lignes rouges" à ne pas franchir ? Dans quels domaines aucun compromis n'est envisageable pour vous ?

Vous pouvez également ajouter des commentaires supplémentaires à vos réponses. N'oubliez pas que la discussion actuelle ne doit pas nécessairement porter sur les mêmes sujets que ceux abordés lors des consultations publiques précédentes (Zielbild E-ID, consultation externe sur l'avant-projet de loi).
Toutes les réponses seront publiées à l'issue de la période de consultation, avec indication de l'expéditeur officiel. 

Pour les questions, les réflexions communes et les demandes d'information en cas de doute, utilisez le forum public sur [GitHub](https://github.com/e-id-admin/open-source-community/discussions). Nous estimons que ce débat public est une fois de plus l'occasion pour chacun (l'équipe du projet e-ID et la communauté) d'élargir ses perspectives.

Les différentes contributions seront prises en compte, ce qui permettra à la Confédération de définir la pile technologique initiale (protocoles, formats et cryptographie) au début de l'année 2024. Cette décision jettera les bases pour la conception de l'infrastructure de confiance suisse pour les moyens de preuves électroniques vérifiables. La définition des spécifications détaillées attendues des parties externes pour le futur écosystème pourra alors être abordée. La réalisation de l'infrastructure de confiance suisse commencera, l'objectif étant de lancer l'e-ID en 2026.


# Annexe
Toutes les annexes ne sont disponibles que dans la version originale anglaise du document :</br>.
https://github.com/e-id-admin/open-source-community/blob/main/discussion-paper-tech-proposal/discussion-paper-tech-proposal.md#annex

# Références
Les références sont disponibles dans la version originale anglaise du document:</br>
https://github.com/e-id-admin/open-source-community/blob/main/discussion-paper-tech-proposal/discussion-paper-tech-proposal.md#references

# Changelog
Les modifications de contenu sont répertoriées dans la version originale anglaise du document :</br>
https://github.com/e-id-admin/open-source-community/blob/main/discussion-paper-tech-proposal/discussion-paper-tech-proposal.md#changelog

Les corrections linguistiques du document français sont documentées ici.

_13.12.2023_ - Publication de la version française
