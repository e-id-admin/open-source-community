# Diskussionspapier: Erste technologische Umsetzung für die Schweizer Vertrauensinfrastruktur

### Festlegen einer gemeinsamen Grundlage für die elektronische Identität der Schweiz und andere elektronischen Nachweise

#### Inhaltsverzeichnis
- [Zweck des Dokuments](#1-Zweck-des-Dokuments)
- [Hintergrund](#2-Hintergrund)
- [Gestaltungsprinzipien und Kriterien](#3-Gestaltungsprinzipien-und-Kriterien)
- [Auswahl der technologischen Szenarien](#4-auswahl-der-technologischen-szenarien)
- [Öffentliche Diskussion dieses Papiers und weitere Schritte](#5-öffentliche-diskussion-dieses-diskussionspapiers-und-weiteres-vorgehen)
- [Anhang](#Anhang)
- [Referenzen](#Referenzen)
- [Changelog](#Changelog)

# 1 Zweck des Dokuments
In 2021 wurde der Bund vom Parlament beauftragt, die Grundlagen für eine elektronische Identität für in der Schweiz lebende Personen und für Auslandschweizerinnen und Auslandschweizer zu schaffen. Parallel zur Ausarbeitung des Gesetzes hat der Bund in den letzten zwei Jahren technische Konzepte validiert, eine breite Community aufgebaut und eine Vision entwickelt für die künftige elektronische Identität der Schweiz (E-ID), für ein Ökosystem digitaler Nachweise und für die zugrunde liegende technische Infrastruktur.

> Nun steht der Bund vor einer wichtigen Frage:<br> _Wie soll diese Vision in einer ersten Phase technisch umgesetzt werden?_

Unter Einbezug der Öffentlichkeit soll eine Diskussion über zwei mögliche und realistische Szenarien geführt werden, die in diesem Dokument beschrieben werden. Ziel dieses Dokuments ist es, einen Überblick über den aktuellen Wissensstand des Bundes zu bieten und Aspekte hervorzuheben, welche für die Bewertung der Szenarien relevant sind. Die Vor- und Nachteile werden dargelegt, insbesondere in Bezug auf Kommunikations- und Austauschprotokolle, Nachweis- und Datenformate sowie die für digitale Nachweise verwendete Kryptographie.

Die Szenarien sollen die öffentliche Diskussion anregen und diese soll wiederum dem E-ID Team eine externe Sichtweise auf mögliche Lösungsansätze liefern. Auch wenn der Inhalt dieses Dokuments einen technischen Schwerpunkt hat, werden politische und gesellschaftliche Aspekte für eine Entscheidung ausschlaggebend sein. Basierend auf den Reaktionen der Öffentlichkeit und den Bewertungen der Szenarien wird der Bund entscheiden, auf welchen technischen Grundlagen die nationale Vertrauensinfrastruktur initial geschaffen wird.

# 2 Hintergrund
## 2.1 Vision einer E-ID und einer Vertrauensinfrastruktur für elektronische Nachweise

Die Ablehnung des früheren Vorschlags für ein nationales E-ID-Gesetz im Jahr 2021 führte zu sechs [Motionen](https://www.parlament.ch/de/ratsbetrieb/suche-curia-vista/geschaeft?AffairId=20213124), die verlangen, dass der Staat eine E-ID ausgeben soll, die den Grundsätzen des «Privacy by Design», der Datenminimierung und der dezentralen Datenspeicherung folgt.

In der Folge wurde eine öffentliche Diskussion über den Umfang der E-ID und eines möglichen Systems geführt, die zu folgenden Entscheidungen führte:

- Es wurde ein neues Gesetzesprojekt lanciert.
- Das Gesetz soll technologisch neutral sein, aber die allgemeine Architektur und die Rollen sollen den Grundsätzen der Selbst-Souveränen Identität (SSI, Self-sovereign Identity) folgen.
- Es soll die Ambitionsstufe 3 erreicht werden, d.h. es werden nicht nur staatlich ausgestellte Nachweise wie die E-ID unterstützt, sondern jede Art von digitalem Nachweis kann von Dritten mit der bereitgestellten Infrastruktur ausgestellt werden.
- Der Staat ist für die Ausgabe der E-ID und den Betrieb der für die Vertrauensinfrastruktur notwendigen Systeme zuständig (Basisregister, Vertrauensregister, Wallet, Check-App, Standardisierung der Kanäle und Formate).
- Die detaillierten Schritte des Gesetzgebungsprojekts und weitere Informationen sind auf der Website des Bundesamts für Justiz veröffentlicht: https://www.bj.admin.ch/bj/de/home/staat/gesetzgebung/staatliche-e-id.html


## 2.2 EU eIDAS 2.0 und das Architecture Reference Framework
Im Juni 2021 hat die EU einen Gesetzesentwurf veröffentlicht, der ähnliche Ambitionen wie die Schweiz verfolgt. Das Architecture and Reference Framework (ARF) zur EUDI Wallet wurde im Januar 2023 veröffentlicht mit dem Ziel, eine interoperable digitale Identitäts-Wallet-Lösung in der EU zu etablieren. Das ARF gibt eine erste Richtung für die technische Umsetzung der eIDAS 2.0 Verordnung vor. Nach dem derzeitigen Verständnis des E-ID-Projektteams handelt es sich beim ARF um eine erste Version, die sich im Laufe der Zeit weiterentwickeln wird.

Das ARF beinhaltet einen Ansatz, der auf bekannter Kryptographie und Technologie basiert, deren Standardisierung stark vorangetrieben wird. Dies ermöglicht einen technisch pragmatischen Weg, um digitale Identitäten anbieten zu können. Gemäss der Einschätzung des E-ID-Teams stellen die im ARF beschriebenen Technologien einen realistischen Weg zur Bereitstellung digitaler Identitäten dar, und für Integratoren ist die Umsetzung transparent und relativ einfach.

Es ist anzumerken, dass der Gesetzesentwurf und die im EU-ARF vorgeschlagene Technologie auch Nachteile mit sich bringen. Die Gesetzgebung wurde von verschiedenen Experten aus der Wissenschaft, der Zivilgesellschaft und von Datenschutzorganisationen kritisiert ([Offener Brief vor dem Trilog](https://epicenter.works/en/content/eidas-open-letter-pre-trilogue-conclusion-300-academics-19-ngos)), die gefordert haben, dass die Gesetzgebung die Nicht-Verknüpfbarkeit (Unlinkability) vorschreibt. Die im ARF vorgeschlagene Technologie bietet ein hohes Mass an Konformität mit bestehenden Sicherheitspraktiken, weist jedoch in Bezug auf bestimmte Aspekte des Schutzes der Privatsphäre Schwachstellen auf. Insbesondere enthalten die Berechtigungsnachweise einen eindeutigen technischen Fingerabdruck, der bei jedem Vorweisen an die Verifikatorin übermittelt wird. Dies kann die Nachverfolgung der Handlungen einer Inhaberin erleichtern, vor allem wenn Verifikatorinnen untereinander Daten austauschen.

Da die Schweiz nicht Mitglied der EU ist, stützt sich das E-ID-Team auf Informationen, welche der Öffentlichkeit zugänglich sind beziehungsweise von der E-ID-Community bereitgestellt werden.

## 2.3 Vom Bund gesammelte praktische Erfahrungen und externes Fachwissen
Der Aufbau einer landesweiten Vertrauensinfrastruktur ist ein komplexes Unterfangen, das mehrere Jahre in Anspruch nehmen wird. Eine solide technische Grundlage ist deshalb von zentraler Bedeutung. Diese muss auf bewährten technischen Grundlagen beruhen und sowohl fachlich, politisch und auch in der Öffentlichkeit breit akzeptiert sein. Um die notwendigen technischen Fähigkeiten zu entwickeln, wurde beschlossen, dass der Bund mit dem Bau kleinerer Systeme beginnt und praktische Erfahrungen mit dem Einsatz und Betrieb von Produkten auf Basis der SSI-Technologie sammelt. So wurden die Projekte ePerso (elektronischer Mitarbeiterausweis), die «Public Sandbox Trust Infrastructure» (Aufbau eines Basisregisters) und eLFA (elektronischer Lernfahrausweis) gestartet. Das Fachwissen des E-ID-Teams vom Bund sowie ein grosser Teil des Wissens, welches in dieses Dokument eingeflossen ist, stammen weitgehend aus diesen Projekten.

Um vom grossen Wissen und Netzwerk der Schweizer E-ID-Community weiter zu profitieren, wurde ein Experten-Kreis, ein Technical Advisory Circle (TAC) gebildet, welches den Bund berät. Das Projektteam möchte sich an dieser Stelle bei den Mitgliedern des TAC für ihre bisherige Mitarbeit herzlich bedanken.

Im Vorfeld der Veröffentlichung dieses Dokuments hat der TAC drei Sitzungen abgehalten, an denen verschiedene Aspekte der allgemeinen Herausforderungen diskutiert, spezifische Fragen beantwortet und mögliche Lösungen und Szenarien für die Umsetzung der Schweizer E-ID erwogen wurden. Es ist erwähnenswert, dass die Mitglieder des TAC unterschiedliche Hintergründe haben und die Diskussionen nicht immer zu einem Konsens führen. Nichtsdestotrotz bieten die Treffen einen ausgezeichneten Überblick über den aktuellen Stand im Bereich der SSI.

Folgende Aspekte wurden während den TAC-Sitzungen erwähnt:
- Diskussionen über den Reifegrad technologischer Lösungen
- Mangel an fundierten wissenschaftlichen Nachweisen für einige Technologien
- Herausforderungen im Zusammenhang mit der dezentralen Speicherung von elektronischen Nachweisen
- Zukunftsfähigkeit der verwendeten Kryptografie (Post-Quantum-Kryptografie)
- Überlegungen zum Datenschutz in Bezug auf die Rückverfolgbarkeit von elektronischen Nachweisen (Unlinkability)
- Überlegungen zum Schutz der Privatsphäre in Bezug auf die Rückverfolgbarkeit von elektronischen Nachweisen im Zusammenhang mit deren Revokation
- Sichere Speicherung von Berechtigungsnachweisen und kryptografischen Schlüsseln
- Mögliche technologische Szenarien und ein technologieübergreifender Ansatz

Die als Diskussionsgrundlage dienenden Folien und die Sitzungsprotokolle der TAC-Sitzungen wurden hier veröffentlicht:
https://github.com/e-id-admin/general/tree/main/meetings


# 3 Gestaltungsprinzipien und Kriterien
## 3.1 Gestaltungsprinzipien
Die Motionen, welche eine vom Staat ausgestellte und betriebene elektronische Identität (E-ID) forderten, beauftragten den Bundesrat, eine E-ID bereitzustellen, die den Grundsätzen von «Privacy by Design», der Datenminimierung und der dezentralen Datenspeicherung folgt.

Eine grobe Architektur wurde definiert und die wesentlichen Rollen und Grundsätze wurden in den [Gesetzesentwurf](https://www.newsd.admin.ch/newsd/message/attachments/84263.pdf) eingebracht und in der [Botschaft](https://www.newsd.admin.ch/newsd/message/attachments/84260.pdf) erläutert.

![swiss_trust_diamond](https://github.com/e-id-admin/open-source-community/assets/12694135/3fe1b206-7563-403a-ac92-be9e28d582e6)
*Abbildung 1: Vertrauensinfrastruktur der Schweiz*

**Privacy by design** wird berücksichtigt durch:
- Das E-ID-Gesetz folgt den SSI-Prinzipien und definiert drei verschiedene Rollen: Ausstellerin, Inhaberin und Verifikatorin. Diese Rollen kommunizieren direkt miteinander.
- Die Ausstellerin hat keine Kenntnis darüber, ob und wie die von ihr ausgestellten elektronischen Nachweise verwendet werden. Das E-ID-Gesetz regelt dieses Prinzip zur Wahrung der Privatsphäre.
- Das System soll die Privatsphäre standardmässig schützen (by default). Da das E-ID-Gesetz technologieunabhängig ist, wird nicht näher ausgeführt, was dies genau bedeutet.

**Datenminimierung** wird berücksichtigt durch:
- Ein elektronischer Nachweis kann vollständig präsentiert werden oder nur Teile davon (Selective Disclosure);
- Wenn machbar, können abgeleitete Information (Predicate Proof; z. B. "ist älter als 18") oder "Zero Knowlege Proofs" präsentiert werden.
- Das erforderliche zentrale Basisregister enthält nur ein Minimum an Daten über registrierte Aussteller und Prüfer sowie über revozierte elektronische Nachweise. Daten über individuell ausgestellte Nachweise werden nicht zentral gespeichert. Das E-ID-Gesetz regelt, welche Informationen im Basisregister enthalten sind.
- Unnötige Datenflüsse werden durch das SSI-Paradigma vermieden (z.B. durch direkte Kommunikation zwischen den betroffenen Akteuren).

**Dezentrale Datenspeicherung** wird dadurch erreicht, dass die E-ID und andere elektronische Nachweise ausschliesslich in der elektronischen Brieftasche (Wallet) des Benutzers gespeichert werden. Von dort aus werden sie der Verifikatorin vorgelegt. Folglich unterliegt jede Datenübermittlung der Kontrolle des Nutzers, und das Vorweisen der Daten bei einer Prüfstelle erfordert immer die aktive Zustimmung des Nutzers.

**Als weitere Anforderungen** wurden während der öffentlichen Konsultation nachdrücklich gefordert:
- Internationale Kompatibilität, insbesondere mit der Europäischen Union
- Die Schweizer E-ID soll so schnell wie möglich eingeführt werden


## 3.2 Kriterien
Während dem Wissensaufbau des E-ID-Teams wurden zur Strukturierung der Diskussionen folgende Kriterien definiert: Schutz der Privatsphäre, Sicherheit, Reifegrad, Benutzerfreundlichkeit, Zukunftsfähigkeit, Interoperabilität & Standardisierung, Bedienbarkeit & Integration. Die Absicht war, sich auf die Hauptunterschiede zwischen den möglichen Ansätzen zu konzentrieren und nicht eine vollständige Vergleichsmatrix zu erstellen. Diese existiert zum Beispiel bereits als [Credential Format Comparison](https://openwallet-foundation.github.io/credential-format-comparison-sig/) von [IDunion-Mitgliedern](https://www.linkedin.com/posts/idunion_credential-format-comparison-and-idunion-ugcPost-7008024118574895104-Cu1d/) und wurde an die Open Wallet Foundation übergeben. Angesichts der Tatsache, dass nicht jede beliebige Kombination von Technologien realisierbar ist, wurde deutlich, dass es keinen perfekten «Tech-Stack» gibt.

Die Diskussion um die Szenarien in Kapitel 4 soll helfen, die Kompromisse zu verstehen, die wir eingehen müssen. Es lassen sich verschiedene Aspekte ausmachen, die miteinander in Konflikt stehen, z.B. die Gewährleistung eines Höchstmasses an Datensicherheit im Vergleich zu einem einfachen und benutzerfreundlichen Produkt, eine hohe Implementierungsgeschwindigkeit im Vergleich zur Gewährleistung vollständiger Interoperabilität, ein maximaler Schutz der Privatsphäre des Benutzers im Vergleich zu einer möglichst geringen Systemkomplexität. 

Es gibt keine Schwarz-Weiss-Antworten auf diese Fragen, und ein gewisses Mass an Kompromissen wird notwendig sein. Das erste Szenario zeichnet verfolgt einen pragmatischeren, international harmonisierten Ansatz. Das zweite Szenario gibt dem Schutz der Privatsphäre mehr Gewicht, in dem Technologien eingesetzt werden, die im Bereich der digitalen Nachweise weniger bekannt und ausgereift sind.

# 4 Auswahl der technologischen Szenarien
## 4.1 Einführung und Herausforderungen

Im Bereich der Self-sovereign Identity gibt es viele Initiativen, die unterschiedliche Lösungen entwickeln. Derzeit sind nur wenige weltweit anerkannte Standards und Systeme im Einsatz; es gibt noch keine dominanten Umsetzungen. Aus diesem Grund ist es nötig, einen ersten technologischen Rahmen für die Schweiz zu definieren, damit die Vertrauensinfrastruktur aufgebaut und sich ein Ökosystem digitaler Nachweise bilden kann.

Der Technologie-Entscheid wird den Entwicklungsteams der Vertrauensinfrastruktur ein gewisses Mass an Stabilität bieten. Zudem erhalten diejenigen, welche die E-ID und die ihr zugrundeliegende Vertrauensinfrastruktur zur selbständigen Prüfung und Ausstellung von Nachweisen nutzen wollen, ebenfalls eine gewisse Stabilität für ihre eigene Softwareentwicklung. Daher will der Bundes Anfangs 2024 einen Entscheid hinsichtlich der Technologie für die Übermittlung und Kommunikation von Nachweisen sowie hinsichtlich des Formats der Nachweise fällen.

Wichtig zu erwähnen ist, dass das Konzept der digitalen Identitäten auf der Basis von SSI noch jung ist und dynamische Veränderungen und Weiterentwicklungen stattfinden werden. Die Entscheidung, die getroffen wird, sollte als **Ausgangspunkt** verstanden werden, der sich wahrscheinlich **entwickeln** wird. Die E-ID wird kein System sein, welches einmal entwickelt und dann auf unbestimmte Zeit ohne Änderungen betrieben werden kann. Eine kontinuierliche Entwicklung und Anpassung an neue Gegebenheiten wird erforderlich sein, um die Kontinuität der Dienste, die Sicherheit und die Interoperabilität langfristig zu gewährleisten.

Basierend auf den Erkenntnissen des E-ID-Teams und den Bestätigungen des TAC kann die Einschätzung gemacht werden, dass keines der aktuellen technologischen Frameworks die Anforderungen aus folgenden Kreisen vollständig erfüllt:
- Den parlamentarischen Anträgen
- Die Rückmeldungen zu den öffentlichen Konsultationen
- Die Anforderungen des E-ID-Teams an den Betrieb einer nationalen Infrastruktur

Die wichtigsten Punkte für die Beurteilung wurden in Kapitel 3.2 erwähnt. Zusammengefasst beziehen sie sich auf die technischen Bereiche:
- Wahrung der Privatsphäre (insbesondere inwieweit die Technologie ein Nachverfolgen der Handlungen eines Benutzers verhindert)
- Interoperabilität (ist die ausgewählte Technologie mit Systemen in anderen Ländern kompatibel)
- Reifegrad der Systeme (insbesondere wie viel Aufwand betrieben werden muss, um das System sicher, stabil und für Millionen von Nutzern nutzbar zu machen)

Wenn man in diesen Punkten gewisse Kompromisse eingeht, wird es dennoch möglich sein, der Schweizer Bevölkerung eine E-ID-Lösung anzubieten. Um einen pragmatischen Weg zu finden, werden in diesem Diskussionspapier zwei realistische Szenarien mit ihren Vor- und Nachteilen aufgezeigt. Die öffentliche Diskussion soll dem Bund eine Aussensicht liefern, welches Szenario für den Start bevorzugt wird.

## 4.2 Szenario A: Der technischen Richtung der EU folgen
_Szenario A_ würde bedeuten, dass ein Teil der im Architecture Reference Framework [ARF](https://digital-strategy.ec.europa.eu/en/library/european-digital-identity-wallet-architecture-and-reference-framework) der Europäischen Union vorgeschlagenen Technologien verwendet wird, mit dem Ziel, sich eng an den Umsetzungsvorschlag der EU anzulehnen. Konkret sollen die folgenden Technologien verwendet werden: SD-JWTs für Berechtigungsnachweise und OID4VC/VP als Ausstellungs- und Präsentationsprotokoll. 

Die ARF bietet einen pragmatischen technischen Ansatz für die Implementierung digitaler Identitäten und elektronischer Brieftaschen (Wallets). Er basiert auf bekannten und bewährten Technologien und Kryptographie, und obwohl die vorgeschlagenen Kommunikationsprotokolle neu sind, bieten sie eine einfache Integration für Teilnehmer des Ökosystems und Transparenz für Softwareentwickler.

Die Hinweise der EU, welche Technologien eingesetzt werden sollen, hat die Anstrengungen um die technische Standardisierung intensiviert. Dennoch bleibt in Anbetracht der Unabhängigkeit der Normungsgremien und des Status des ARF als noch nicht abgeschlossenes Projekt ein gewisses Mass an Ungewissheit bestehen, wie die finale technische Architektur der EU aussehen wird. Da das ARF bereits mehrere Normen enthält (SD-JWT und ISO-MDL), ist es auch möglich, dass es zu einer weiteren technologischen Fragmentierung kommen könnte.

Was die Wahrung der Privatsphäre betrifft, so haben technische Communities, Datenschutzbeauftragte, die Wissenschaft und die Zivilgesellschaft seit der Veröffentlichung des ARF Bedenken geäussert. Mit Blick auf die Schweiz zeigt die Analyse des E-ID-Teams, dass sich diese Kritik hauptsächlich auf das Thema «Unverknüpfbarkeit» (Unlinkability) bezieht. Ohne Umgehungslösungen, wie z.B. die massenhafte Ausstellung von Nachweisen zur einmaligen Verwendung, kann die Unlinkability der verwendeten Nachweise derzeit nicht gewährleistet werden (siehe - [Anhang Unlinkability](https://github.com/e-id-admin/open-source-community/blob/main/discussion-paper-tech-proposal/discussion-paper-tech-proposal.md#a3unlinkability)). Besonders heikel ist dies bei elektronischen Nachweisen, die regelmässig verwendet werden oder auf die Identität einer Person schliessen lassen.

Das Vorweisen von Teilen eines Nachweises ist möglich, und die Benutzer können der Weitergabe der gewünschten Teile eines elektronischen Nachweises zustimmen. Beim Vorweisen werden jedoch immer Hashes aller im Nachweis enthaltenen Attribute übermittelt. Dies stellt eine Art Fingerabdruck dar.

Die vorgeschlagene Lösung (Statusliste) für das Führen von Revokationsinformationen ist einfach zu implementieren und kann eine Offline-Überprüfung ermöglichen (siehe -[Anhang Statusliste](https://github.com/e-id-admin/open-source-community/blob/main/discussion-paper-tech-proposal/discussion-paper-tech-proposal.md#status-list)). Es kann argumentiert werden, dass sie gewisse Schwächen hinsichtlich der Wahrung der Privatsphäre aufweist. Bestimmte Akteure innerhalb des Ökosystems können sich eine Position in der für den Widerruf vorgesehenen Liste merken und dann die Statusänderung im Zusammenhang mit der Berechtigung verfolgen (wenn sie widerrufen wird). In der Revokationsliste werden weder Informationen über den Inhalt eines Nachweises noch der Grund für den Widerruf gespeichert. Diese Nachverfolgung kann nur gemacht werden, wenn der Akteur die Position des Nachweises in der Statusliste kennt (wird während der Präsentation bekannt gegeben). Theoretisch könnten in _Szenario A_ alternative Revokationsmechanismen verwendet werden, die ein höheres Mass an Datenschutz gewährleisten. Da jedoch die Unlinkability der Nachweise nicht gegeben ist, wäre zu untersuchen, wie gross der Gewinn an Datenschutz effektiv sein könnte. 

Es ist anzumerken, dass die Schweiz eigenständig Recht setzt und _Szenario A_ keine direkte Übernahme der EIDAS 2.0-Gesetzgebung oder Kompatibilität mit der EU bedeutet. Das ARF wird als nicht-statisch betrachtet und auch eine Entwicklung hin zu einem Ansatz, welcher die Privatsphäre stärker schützt, ist daher in Zukunft möglich. Sollte sich die Schweiz für  _Szenario A_ entscheiden, sollte dies die technische Kompatibilität mit der EU erleichtern.

![Scenario_A](https://github.com/e-id-admin/open-source-community/assets/12694135/5dcce8b1-85d6-4fd4-a096-8da9d5d9842a)
*Abbildung 2: Szenario A*

**Hauptvorteile für ein Schweizer Vertrauens-Ökosystem:**
- Potenziell erleichterte technische Kompatibilität mit der EU
- Erprobte Technologie und standardisierte Kryptographie
- Rasche Standardisierung und breiter Wissensaufbau in der Öffentlichkeit dank den Bemühungen der EU
- Erleichtertes Verständnis und Integration für Dienstanbieter
- Breite Unterstützung für hardwarebasierte Kryptographie

**Hauptnachteile für ein Schweizer Vertrauens-Ökosystem:**
- Potenzielle Rückverfolgbarkeit der Nutzung der Nachweise
- Potenzielle Rückverfolgbarkeit durch Revokationsmechanismus
- Keine abgeleiteten Informationen (predicate proofs) oder Zero-Knowledge-Proofs
- Selective Disclosure kann nicht datensparsam implementiert werden

**Technologien**
- Format der Nachweise: SD-JWT
- Signaturen: ECDSA oder RSA
- Kommunikationsstandards: OID4VC/VP
- Revokationsmechanismus: Statusliste


**Risiken und Ungewissheiten**
- Das ARF der EU gilt noch nicht als endgültig. Um die von der EU verwendete endgültige Technologie vollständig übernehmen zu können, müsste die Schweiz ihr nationales E-ID-Projekt möglicherweise auf einen unbestimmten Zeitpunkt verschieben.
- Es gibt Bedenken hinsichtlich des Datenschutzes und der Rückverfolgbarkeit, die noch nicht ausgeräumt wurden. Dieser Umstand könnte das Vertrauen in das System untergraben.


## 4.3 Szenario B
In _Szenario B_ werden Technologien verwendet, welche den Inhaberinnen im Ökosystem ein höheres Mass an Privatsphäre bieten sollen als in _Szenario A_. Zum aktuellen Zeitpunkt würde dies bedeuten, dass ein eigenständiger technologischer Pfad für die Schweizer Vertrauensinfrastruktur und die nationale E-ID gewählt würde.

Gemäss aktuellem Stand würden elektronische Nachweise auf der Grundlage von JSON-LD Verifiable Credentials unter Verwendung von BBS+-Signaturen ausgestellt werden. Was das Kommunikationsprotokoll betrifft, so ist die Wahl für dieses Szenario noch offen. Beide gängigen Protokolle im SSI-Bereich (DIDcomm und OID4VP/VC) sind mit dem vorgeschlagenen Nachweisformat kompatibel. DIDcomm bietet den Vorteil einer End-zu-End-Verschlüsselung, während OID4VC/VP eine einfachere Integration bietet und auf Standards basiert, die vielen Integratoren vertraut sind.

Durch die Verwendung einer alternativen, aber weniger verbreiteten Kryptographie können Nachweise ausgestellt werden, die es den Inhaberinnen ermöglichen, sich im künftigen Vertrauens-Ökosystem zu bewegen und dabei weniger digitale Spuren zu hinterlassen. Die Anforderungen in Bezug auf «Unlinkability» (siehe - [Anhang Unlinkability](https://github.com/e-id-admin/open-source-community/blob/main/discussion-paper-tech-proposal/discussion-paper-tech-proposal.md#a3unlinkability)) werden von diesen Technologien erfüllt. Die digitalen Nachweise verwenden Signaturverfahren, die es den Wallets ermöglichen, für jede Überprüfung eine einzigartige Nachweis-Präsentation zu erzeugen. Diese Nachweis-Präsentationen enthalten keine Konstanten und sind daher nicht mit dem ursprünglich ausgestellten Berechtigungsnachweis verknüpft. Da die Generierung der Nachweise für jede Überprüfung einzigartig ist, wird die Korrelation auch zwischen Verifikatorinnen erschwert. Nicht berücksichtigt ist dabei, dass der offengelegte Inhalt selbst eine Verknüpfung und Korrelation ermöglichen könnte.

Wie bei der in _Szenario A_ verwendeten Technologie wird auch Selective Disclosure, also die teilweise Offenlegung eines Nachweises unterstützt. Es scheint auch möglich zu sein, überprüfbare abgeleitete Informationen (predicate proofs) in BBS+ durch die Verwendung zusätzlicher Unterprotokolle zu erreichen. Die verfügbaren Informationen zu diesem Thema sind jedoch begrenzt und bedürfen weiterer wissenschaftlicher Forschung.

Mit _Szenario B_ und dem Ziel von Nicht-Korrelation und Unlinkability ergeben sich weitere Herausforderungen, die über das Vorweisen von Nachweisen hinausgehen. Im Zusammenhang mit _Szenario B_ ist der datenschutzkonforme Widerruf (Revokation) ein Thema, das angegangen werden müsste. Es bestehen verschiedene Möglichkeiten, um beim Revokationsmechanismus die Rückverfolgbarkeit der Nutzung von Nachweisen zu reduzieren. Die Ansätze drehen dabei um die Speicherung von Revokationsinformationen in einem staatlichen Dienst (Revokation als Funktion des Basisregisters), das Abholen von Nicht-Widerrufs-Nachweisen (non-revocation proof) durch die Inhaberin oder die Verwendung von zusätzlichen, kurzlebigen Gültigkeitsnachweisen (validity credentials). Ohne diese Möglichkeiten können Akteure potenziell in Erfahrung bringen, wann ein Nachweis verwendet wurde oder wann sich der Status eines Nachweises geändert hat (z. B. widerrufen wurde). Es gibt jedoch nicht die eine Lösung, welche den Bedürfnissen aller Nachweise und aller Teilnehmer des Ökosystems entspricht. So könnten bestimmte Anwendungsfälle von der Möglichkeit profitieren, Änderungen des Widerrufsstatus immer wieder abzurufen. Daher könnten in _Szenario B_ auch mehrere Widerrufsmechanismen ins Spiel kommen.

Die Standardisierung von [JSON-LD Credentials](https://www.w3.org/TR/vc-data-model-2.0/) und [BBS+](https://datatracker.ietf.org/doc/draft-irtf-cfrg-bbs-signatures/) ist im Gange. Es kann nicht vorhergesagt werden, wann diese Standards finalisiert sein werden.

Sich für einen Start mit _Szenario B_ zu entscheiden, würde bedeuten, eine Diskrepanz zwischen der schweizerischen und der EU-Implementierung in Kauf zu nehmen. Das in diesem Kapitel vorgeschlagene Nachweis-Format und Signaturschema könnte jedoch trotzdem in der Zukunft von der EU im Rahmen von eIDAS 2.0 akzeptiert werden (besonders in Bezug auf Nachweise, welche keine Personeninformationen enthalten). Es ist auch nicht komplett ausgeschlossen, dass das vorgeschlagene _Szenario B_ zu einem späteren Zeitpunkt mit den EU-Anforderungen konform wäre.

Das E-ID-Team hat festgestellt, dass die in _Szenario B_ eingesetzte Technologie zwar einen besseren Schutz der Privatsphäre verspricht, aber im Vergleich zu _Szenario A_ einen geringeren Reifegrad aufweist, was dazu führen kann, dass mehr Aufwand und Ressourcen für die Entwicklung der Vertrauensinfrastruktur erforderlich wären.

![Scenario_BII](https://github.com/e-id-admin/open-source-community/assets/12694135/7de2c66d-dd7e-4351-acbd-6e68d5a9364e)
*Abbildung 3: Szenario B*

**Hauptvorteile für ein Schweizer Vertrauens-Ökosystem:**
- Unlinkability für Inhaberinnen ist gegeben
- Die Korrelation von Inhaberinnen wird für zusammenarbeitende Verifikatorinnen erschwert
- Selective Disclosure wird auf eine datenminimierende Weise erreicht
- Datenschutzfreundliche Revokationsmechanismen könnten implementiert werden
- Zero-Knowledge-Proofs und Predicate Proofs scheinen implementierbar zu sein
- JSON-LD-Eigenschaften werden gewonnen (Automatisierung und Benutzerfreundlichkeit)
- Fallback zu _Szenario A_ ist möglich

**Hauptnachteile für ein Schweizer Vertrauens-Ökosystem:**
- Anfänglich keine technische Kompatibilität oder zusätzlicher Aufwand zur Erreichung der Interoperabilität mit den digitalen Identitäten der EU
- Komplexere Integration und weniger bekannte Kryptographie (volles Verständnis der Funktionsweise nur für Experten möglich)
- Sicherheit des vorgeschlagenen Szenarios weniger erforscht
- Kleinere Entwickler-Community
- Nutzung von Hardware-basierte Kryptografie nicht abschliessend geklärt (einige Anbieter geben an, BLS12-381 zu unterstützen)

**Technologien**
- Nachweis-Format: JSON-LD
- Signaturen: BBS+
- Kommunikationsstandards: OID4VC/VP oder DIDCOMM
- Revokationsmechanismus: Offen

**Risiken und Unsicherheiten**
- Die Verwendung weniger verbreiteter Technologien (z.B. eine geringere Anzahl verfügbarer Software-Bibliotheken) und die Notwendigkeit, komplexere Mechanismen zur Verbesserung des Datenschutzes zu implementieren, könnten sich auf den Entwicklungszeitplan auswirken.
- Die internationale Akzeptanz und damit die potenzielle Interoperabilität für Schweizer E-ID-Inhaberinnen bleibt unklar.
- Die durch die Verwendung von BBS+-Signaturen gewonnenen Fähigkeiten zur Wahrung der Privatsphäre könnten in Zukunft durch die Post-Quantum-Kryptographie, welche diese Fähigkeiten möglicherweise nicht unterstützt, verloren gehen.


## 4.4 Zusammenfassung

Es wurden zwei mögliche Wege aufgezeigt, welche die Schweiz beschreiten kann, um eine nationale E-ID umzusetzen. Beide weisen Vor- und Nachteile auf. Vereinfacht lassen sich die Szenarien folgendermassen zusammenfassen:

_Szenario A_: Es wird allgemein bekannte Kryptographie verwendet. Die technische Interoperabilität mit der EU sollte auf einfache Weise erreicht werden können. Eine breite Entwickler-Community, bestehende Software-Bibliotheken und anerkannte Kryptographie sollten die Entwicklung vereinfachen. Was den Schutz der Privatsphäre betrifft, so haben die Inhaberinnen die volle Kontrolle darüber, welchen Inhalt eines Nachweises sie mit welchem Akteur teilen (Selective Disclosure). Da die vorgewiesenen Nachweise einen konstanten Fingerabdruck aufweisen und das System zur Revokation keine vollständige Wahrung der Privatsphäre bietet, können die Inhaberinnen nach dem Vorweisen eines Nachweises durch Verifikatorinnen leichter verfolgt werden. Die Stabilität der Umsetzungsvorgaben sind bei einer strikten Verfolgung des EU-Wegs noch nicht gegeben, da die EU noch keine endgültige technische Vorgabe publiziert hat.

Szenario B_: Die verwendete Kryptographie ist weniger verbreitet und weniger gut erforscht. Die technische Interoperabilität in der EU könnte sich als grössere Herausforderung erweisen. Dennoch könnte sie durch eine Umstellung des Systems oder die zusätzliche Ausstellung eines eIDAS 2.0-konformen Nachweises zu einem späteren Zeitpunkt erreicht werden. Angesichts des höheren Anspruchs an die Wahrung der Privatsphäre könnten die Implementierungen (z.B. der Revokationsmechanismus) komplexer werden. Möglicherweise muss mehr Aufwand in die Entwicklung von Software-Bibliotheken und Sicherheitsprüfungen investiert werden. Was den Schutz der Privatsphäre angeht, so haben die Inhaberinnen die volle Kontrolle darüber, welche Inhalte eines Nachweises sie mit welchen Akteuren teilen (Selective Disclosure). Da eine aufwändigere Kryptographie verwendet wird und die Nachweise bei jeder Präsentation einen einzigartigen Fingerabdruck aufweisen, wird es schwieriger sein, die Handlungen einer Inhaberin nach der Überprüfung zu korrelieren. Sollte die Umsetzung scheitern, ist ein Fallback auf _Szenario A_ möglich.

Um ein Produkt anbieten zu können, das alle Erwartungen der Nutzer (Datenschutz und Interoperabilität) erfüllt, müssten die E-ID und die ihr zugrunde liegende Infrastruktur im Endzustand vielleicht beide vorgestellten Szenarien abdecken.

Ein praktischer Ansatz könnte darin bestehen, zunächst eines der genannten Szenarien zu implementieren und nach erfolgreicher Einführung das System parallel dazu auf weitere Technologien zu erweitern. Dies unterstreicht, dass die Entscheidung als Ausgangspunkt für die produktive Infrastruktur zu verstehen ist.

## 4.5 Unberücksichtigte Aspekte

Aufgrund der Komplexität des Themas SSI und der zugrundeliegenden Technologie bleiben einige Fragen offen, die in den vorgeschlagenen Szenarien nicht beantwortet werden. Während einige von ihnen unabhängig voneinander angegangen werden können, muss bei anderen die Entscheidung für eines der vorgestellten Szenarien getroffen werden, um eine angemessene Prüfung zu ermöglichen.

Aspekte, die derzeit in den Szenarien nicht berücksichtigt werden:
- Hardware-/Krypto-Chip-basierte Sicherheit der Wallet
- Konzept und Implementierung des Basisregisters
- Konzept und Implementierung eines Vertrauensregisters
- Identitäten von Organisationen/juristischen Personen und der Prozess, der zur Registrierung im Basis- und Vertrauensregister führt
- Entscheidung über die Verwendung von DIDs und DID-Methoden
- Der Prozess der E-ID-Ausstellung
- Backup von digitalen Nachweisen

# 5 Öffentliche Diskussion dieses Diskussionspapiers und weiteres Vorgehen
Nun, da wir dieses Kapitel erreicht haben, ist Ihre Meinung gefragt!

Um eine fundierte Entscheidung treffen zu können, bittet das E-ID-Team um Beiträge und Perspektiven von einer Vielzahl öffentlicher Interessengruppen. Bitte nehmen Sie die vorgeschlagenen Szenarien als Grundlage für Ihr Feedback und bedenken Sie, dass die Entscheidung ein Ausgangspunkt für die mittelfristige technische Umsetzung ist. Das E-ID Team ist überzeugt, dass beide Varianten die wichtigsten Erwartungen an eine Vertrauens-Ökosystem für digitale Nachweise abdecken. Welche Eigenschaften und Merkmale sind aus Ihrer Sicht am wertvollsten und müssen erreicht werden? Welche Kompromisse und Risiken sind akzeptabel, warum und für wie lange?

**Eckpfeiler der öffentlichen Diskussion**
- Die öffentliche Diskussion beginnt mit der Präsentation dieses Diskussionspapiers am Partizipationsmeeting vom 1. Dezember 2023.
- Die schriftlichen Stellungnahmen müssen bis zum 15. Januar 2024 via Feedback-Formular unter https://findmind.ch/c/XFvu-a7c6 eingesendet werden. Bitte werfen Sie einen Blick auf das Formular an, _bevor_ Sie mit dem Schreiben Ihres Feedbacks beginnen.
- Schriftliche Diskussionen können auf GitHub geführt werden: https://github.com/e-id-admin/open-source-community/discussions

**Die wichtigsten Fragen für Ihr Feedback**
- Welches Szenario bevorzugen Sie? Und aus welchem Grund?
- Erfüllen beide Szenarien Ihre Erwartungen?
- Welche grossen Risiken sehen Sie?
- Welche «roten Linien» sollten nicht überschritten werden? Wo ist für Sie kein Kompromiss denkbar?

In den schriftlichen Stellungnahmen können Sie auch weitere Punkte einbringen. Bedenken Sie, dass die aktuelle Diskussion nicht dieselben Themen abdecken muss, die in früheren öffentlichen Konsultationen (Zielbild E-ID, öffentliche Konsultation zum Vorentwurf des Gesetzes) angesprochen wurden.
Alle Antworten werden nach Ablauf der Konsultationsfrist mit Angabe des offiziellen Absenders veröffentlicht. 

Für Fragen, Gedankenaustausch und Nachfrage bei Unklarheiten steht Ihnen das öffentliche Forum auf [GitHub](https://github.com/e-id-admin/open-source-community/discussions) zur Verfügung. Das E-ID Team sieht diese öffentliche Debatte erneut als eine Gelegenheit für alle, ihre Perspektiven zu erweitern.

Die verschiedenen Rückmeldungen werden berücksichtigt, so dass der Bund Anfang 2024 die erste Technologieentscheidung treffen kann (Protokolle, Formate und Kryptographie). Mit dem Entscheid werden die Grundlagen für den Aufbau der Schweizer Vertrauensinfrastruktur für elektronische Nachweise gelegt. Die Definition der detaillierten Spezifikationen, die von externen Parteien für das zukünftige Ökosystem erwartet werden, kann dann in Angriff genommen werden. Mit der Realisierung der Schweizer Vertrauensinfrastruktur wird begonnen, mit dem Ziel, die E-ID im Jahr 2026 zu lancieren.

# Anhang
_Sämtliche Anhänge steht nur in der englischen Originalversion des Dokuments zur Verfügung:_</br>
https://github.com/e-id-admin/open-source-community/blob/main/discussion-paper-tech-proposal/discussion-paper-tech-proposal.md#annex

# Referenzen
_Die Referenzen stehen in der englischen Originalversion des Dokuments zur Verfügung:_</br>
https://github.com/e-id-admin/open-source-community/blob/main/discussion-paper-tech-proposal/discussion-paper-tech-proposal.md#references

# Changelog
_Inhaltliche Änderungen werden in der englischen Originalversion des Dokuments aufgeführt:_</br>
https://github.com/e-id-admin/open-source-community/blob/main/discussion-paper-tech-proposal/discussion-paper-tech-proposal.md#references

Sprachliche Korrekturen des deutschen Dokuments werden hier dokumentiert.

_12.12.2023_ - Publikation der Deutschen Fassung
