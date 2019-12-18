# METS: Überblick und Anleitung

## Einleitung

METS ist ein Kodierungsstandard für beschreibende, administrative und strukturelle Metadaten von komplexen digitalen Objekten unterschiedlichster Art, bspw. Text-, Bild- oder Multimedia-Objekte. Ursprünglich eine Initiative der [Digital Library Federation](http://www.diglib.org/) ist METS ein XML-Dokumentformat für die Kodierung von Metadaten sowohl für die Verwaltung von digitalen Objekten innerhalb eines digitalen Archivsystems als auch für den Austausch solcher Objekte zwischen digitalen Archivsystemen und mit den Benutzerinnen und Benutzern. Abhängig vom jeweiligen Anwendungsfall können METS-Dokumente im Kontext des [Referenzmodells für ein Offenes Archiv-Informations-System (OAIS)](http://ssdoo.gsfc.nasa.gov/nost/isoas/ref_model.html) entweder als Übergabeinformationspaket (Submission Information Package - SIP), Archivinformationspaket (Archival Information Package - AIP) oder Auslieferungsinformationspaket (Dissemination Information Package - DIP) verwendet werden.

METS bietet ein kohärentes, integriertes Framework für die komplexen Metadaten, die für die Pflege von einer Sammlung digitaler Objekte benötigt werden. Diese Metadaten sind sowohl ausführlicher als auch unterschiedlich im Vergleich zu denen über Sammlungen von gedruckten Werken oder anderen physischen Objekten. Ein Buch bspw. wird weder in einzelne Blätter zerfallen, wenn keine Strukturmetadaten über die innere Ordnung des Buches vom Besitzer erfasst werden, noch wird es Benutzerinnen und Benutzer hindern, den Nutzungswert beurteilen zu können, wenn der Hersteller keine Angaben zum Entstehungsprozess gemacht hat. Gleiches gilt jedoch nicht für eine digitale Version desselben Buches. Ohne Strukturmetadaten sind die Seitenabbildungen oder die Textdateien, aus denen es besteht, so gut wie wertlos. Und ohne technische Metadaten über den Digitalisierungsprozess können Benutzerinnen und Benutzer nicht sicher sein, wie genau die digitale Version die ursprüngliche Vorlage wiedergibt. Eine Organisation muss auch über notwendige technische Metadaten für interne Verwaltungszwecke verfügen, um bspw. die Daten regelmäßig überprüfen und migrieren zu können und damit die dauerhafte Nutzbarkeit der wertvollen Ressourcen sicher zu stellen.

Ein METS-Dokument besteht aus sieben Hauptabschnitten:

1.  [**Der Kopfteil (METS Header)**](#MHead) - Der Kopfteil enthält
    Metadaten, die das jeweilige METS-Dokument selbst beschreiben,
    einschließlich der Angaben zum Bearbeiter, zum Herausgeber, des 
    letzten Änderungsdatums des METS-Dokuments, etc.

2.  [**Erschließungsangaben (Descriptive Metadata)**](#descMD) - Der
    Abschnitt für die Erschließungsangaben kann sowohl Verweise auf ein
    externes Dokument, (etwa einen MARC Datensatz in einem OPAC oder ein
    EAD-Findbuch auf einem WWW-Server), wie auch in das METS-Dokument
    eingebettete Angaben oder beides enthalten. Es können auch mehrere
    externe und interne Erschließungspakete in dem
    Erschließungsabschnitt verwendet werden.

3.  [**Verwaltungsangaben (Administrative Metadata)**](#admMD) - Der
    Abschnitt für die Verwaltungsangaben liefert Informationen über die
    Herstellung und Speicherung von Dateien, über Urheberrechte und über
    die digitalisierte Vorlage. Außerdem werden hier Angaben zur
    Herkunft der Digitalisate erfasst (z.B. über das Verhältnis von
    Master und Derivaten sowie über Migrationen.) Ähnlich wie die
    Erschließungsangaben können diese Metadaten extern oder in das
    METS-Dokument integriert vorliegen.

4.  [**Dateienabschnitt (File Section)**](#filegrp) - Im
    Dateienabschnitt werden alle Dateien mit Inhalten, aus denen das
    digitale Objekt besteht, aufgelistet. Einzelne zusammengehörige
    Dateien können dabei mit dem Element zusammengefasst werden, um etwa
    verschiedene Versionen auseinander halten zu können.

5.  [**Strukturbeschreibung (Structural Map)**](#structmap) - Die
    Strukturbeschreibung ist zentraler Bestandteil eines jeden
    METS-Dokuments und das einzige obligatorische Element. Sie bildet
    den inneren Aufbau des digitalen Objektes ab und verknüpft die
    Elemente der Struktur mit den Dateien, aus denen der Inhalt des
    digitalen Objektes besteht, sowie mit deren Metadaten. Mehrere
    Strukturbeschreibungen sind möglich

6.  [**Strukturverknüpfungen (Structural Links)**](#structlink) - Der
    Abschnitt mit den Strukturverknüpfungen erlaubt es den Erstellern
    von METS-Dokumenten das Vorhandensein von Hyperlinks zwischen
    einzelnen Knoten des im Strukturabschnitt dargestellten
    hierarchischen Aufbaus des digitalen Objekt zu beschreiben. Diese
    Funktion ist besonders für die Archivierung von Webseiten gedacht.

7.  [**Verhalten (Behavior)**](#behavior) - Ein Abschnitt über das
    Verhalten des digitalen Objekts kann verwendet werden, um
    ausführbare Anweisungen für das Verhalten mit den Inhalten in
    METS-Objekten zu verknüpfen. Jede Verhaltensform hat ein
    Schnittstellendefinitionselement, das eine abstrakte Definition
    eines Satzes von Verhaltensformen in dem jeweiligen Abschnitt
    enthält. Außerdem besitzt jede Verhaltensform ein
    Mechanismuselement, das ein Modul ausführbaren Codes enthält, mit
    dem die in der Schnittstellendefinition abstrakt formulierten
    Verhaltensformen ausgeführt werden können.

Es folgte eine detailliertere Erläuterung jedes Abschnitts und deren
Bezüge zu einander.

## <span id="MHead">Kopfteil (METS Header)</span>

Der Kopfteil erlaubt die Erfassung eines Minimums an beschreibenden
Metadaten über das METS-Dokument selbst. Dazu gehört das Datum der
Erstellung und der letzten Veränderung sowie eine Angabe zum Status des
METS-Dokuments. Dort können auch die Namen der Personen, die an dem
METS-Dokument mitgearbeitet haben, aufgenommen werden und ihre Funktion
sowie die Art ihres Beitrags beschrieben werden. Schließlich können
verschiedene alternativer Identifikatoren für das METS-Dokument als
Ergänzung des Hauptidentifikators im `<objid>`-Element im
METS-Wurzelelement erfasst werden. Ein kleines Beispiel eines
METS-Kopfteils könnte folgendermaßen aussehen:  

```xml
<metsHdr CREATEDATE="2003-07-04T15:00:00" RECORDSTATUS="Complete">
  <agent ROLE="CREATOR" TYPE="INDIVIDUAL">
    <name>Jerome McDonough</name>
  </agent>
  <agent ROLE="ARCHIVIST" TYPE="INDIVIDUAL">
    <name>Ann Butler</name>
  </agent>
</metsHdr>         
```

Dieses Beispiel hat zwei Attribute im Element `<metshdr>`, CREATEDATE
und RECORDSTATUS, die für die Angabe des Erstellungsdatums und des
Bearbeitungsdatum genutzt werden. Es werden zwei Personen erwähnt, die
das METS-Dokument bearbeitet haben. Die erste Person hat den
METS-Datensatz erstellt, die zweite Person ist für die archivarische
Bearbeitung des Ausgangsmaterials verantwortlich. Sowohl das ROLE- als
auch das TYPE- Attribut zum `<agent>`-Element benutzen kontrollierte
Terminologien. Erlaubte Werte für das Attribut ROLE sind "ARCHIVIST,"
"CREATOR," "CUSTODIAN," "DISSEMINATOR," "EDITOR," "IPOWNER" und "OTHER."
Erlaubte Werte für das Attribut TYPE sind "INDIVIDUAL," "ORGANIZATION"
oder "OTHER."

## <span id="descMD">Erschließungsangaben (Deskriptive Metadata)</span>

Der Abschnitt für die Erschließungsangaben besteht aus einem oder
mehreren `<dmdSec>` Elementen (Erschließungsabgaben - Descriptive
Metadata Section). Jedes `<dmdSec>`-Element kann einen Verweis auf
externe Erschließungsangaben (ein `<mdRef>`-Element), auf interne
eingebettete Erschließungsangaben (in einem `<mdWrap>`-Element) oder
beides enthalten.

**Externe Erschließungsangaben (External Descriptive Metadata -
mdRef):** ein `<mdRef>`-Element liefert einen URI (Uniform Resource
Identifier), der für den Zugriff auf die externen Angaben verwendet
werden kann. So verweisen etwa die folgenden Metadatenreferenzen auf ein
Findbuch für ein spezielles digitales Objekt:

```xml 
<dmdSec ID="dmd001">
  <mdRef LOCTYPE="URN" MIMETYPE="application/xml" MDTYPE="EAD"
    LABEL="Berol Collection Finding Aid" xlink:href="urn:x-nyu:fales1735" />
</dmdSec>       
```

Das `<mdRef>`-Element dieser `<dmdSec>` besitzt vier Attribute. Das
LOCTYPE-Attribut legt den Typ der Lokalisierungsangabe, die in dem
Element angegeben wird, fest. Mögliche Werte für das LOCTYPE-Attribut
umfassen 'URN,' 'URL,' 'PURL,' 'HANDLE,' 'DOI,' und 'OTHER.' Das
MIMETYPE-Attribute erlaubt es, den MIME-Typ der externen
Erschließungsangaben oder externen Metadaten anzugeben. Das
MDTYPE-Attribut erlaubt es, anzugeben, nach welchem Standard die Angaben
erfasst wurden. Gültige Werte des MDTYPE-Attributs sind MARC, MODS, EAD,
VRA (VRA Core), DC (Dublin Core), NISOIMG (NISO Technical Metadata for
Digital Still Images), LC-AV (Library of Congress Audiovisual Metadata)
, TEIHDR (TEI Header), DDI (Data Documentation Initiative), FGDC
(Federal Geographic Data Committee Metadata Standard
\[FGDC-STD-001-1998\] ) und OTHER. Das LABEL-Attribut liefert einen
Mechanismus für die Bezeichnung dieser Metadaten bei der Präsentation
des METS-Dokuments auf dem Bildschirm z.B. in einem
"Inhaltsverzeichnis".

**Interne Erschließungsangaben (Internal Descriptive Metadata -
mdWrap):** Das `<mdWrap>`-Element liefert eine Hülle für Metadaten, die
in ein METS-Dokument eingebettet sind. Solche Metadaten können eine der
beiden folgenden Formen besitzen: 1. XML-kodierte Metadaten, wobei die
XML-Kodierung sich selbst als zugehörig zu einem anderen Namensraum als
dem des METS-Dokumentes identifiziert, oder 2. beliebige Angaben in
binärer oder textlicher Form, soweit sie Base64 kodiert und in ein
`<binData>`-Element eingekapselt sind. Die folgenden Beispiele zeigen
die Verwendung des `<mdWrap>`-Elements:

```xml
<dmdSec ID="dmd002">
  <mdWrap MIMETYPE="text/xml" MDTYPE="DC" LABEL="Dublin Core Metadata">
    <xmlData>
      <dc:title>Alice's Adventures in Wonderland</dc:title>
      <dc:creator>Lewis Carroll</dc:creator>
      <dc:date>between 1872 and 1890</dc:date>
      <dc:publisher>McCloughlin Brothers</dc:publisher>
      <dc:type>text</dc:type>
    </xmlData>
  </mdWrap>
</dmdSec>           
```

```xml 
<dmdSec ID="dmd003">
  <mdWrap MIMETYPE="application/marc" MDTYPE="MARC" LABEL="OPAC Record">
    <binData>MDI0ODdjam0gIDIyMDA1ODkgYSA0NU0wMDAxMDA...(etc.)</binData>
  </mdWrap>
</dmdSec>           
```

Es ist zu beachten, dass alle `<dmdSec>`-Elemente ein ID-Attribut
besitzen müssen. Dieses Attribut liefert eine eindeutige, interne
Bezeichnung für jedes `<dmdSec>`-Element, das in dem Strukturabschnitt
genutzt werden kann, um einen besonderen Bereich innerhalb der internen
Hierarchie des Dokumentes mit einem speziellen `<dmdSec>`-Element zu
verknüpfen. Damit können bestimmte Bereiche der Erschließungsangaben mit
bestimmten Teilen des digitalen Objektes verbunden werden.

## <span id="admMD">Verwaltungsangaben (Administrative Metadata)</span>

Die `<amdSec>`-Elemente enthalten die Metadaten für die Administration
der Dateien des digitalen Objektes sowie für die Verwaltung des
ursprünglichen Archivobjektes. Es gibt vier Arten von
Verwaltungsmetadaten für METS-Dokumente: 1. Technische Metadaten
(Angaben über die Herstellung der Dateien, das Format und
Nutzungsbesonderheiten), 2. Urheberrechte und Lizenzinformationen, 3.
Angaben zur Quelle (Beschreibungs- und Verwaltungsangaben zur analogen
Quelle einer Digitalisierung), 4. Digitale Herkunftsangaben (zum Quelle-
und Abbild-Verhältnis zwischen Dateien, einschließlich der Bestimmung
von Master und Derivaten und Informationen über Migrationen und
Transformationen der Daten zwischen der ursprünglichen Digitalisierung
eines Gegenstandes und seiner gegenwärtigen Erscheinung als digitales
Objekt). Diese vier verschiedenen Typen von Verwaltungsangaben werden
mit eigenen Unterelementen innerhalb des `<amdSec>`-Abschnitts des
METS-Dokumentes bezeichnet, die diese Art der Metadaten aufnehmen
können: `<techMD>`, `<rightsMD>`, `<sourceMD>` und `<digiprovMD>`.
Jedes dieser Elemente kann mehrfach verwendet werden.

Die Elemente `<techMD>`, `<rightsMD>`, `<sourceMD>` und `<digiprovMD>`
verwenden das gleiche Modell wie das `<dmdSec>`-Element: sie können ein
`<mdRef>`-Element mit Verweis auf externe Verwaltungsdaten, ein
`<mdWrap>`-Element als Hülle für eingebettete Verwaltungsangaben im
METS-Dokument oder beides neben einander enthalten. Mehrere
Wiederholungen dieser Elemente können in einem METS-Dokument enthalten
sein und jedes von ihnen muss ein ID-Attribut besitzen, damit andere
Elemente innerhalb des METS-Dokuments, etwa Teile der Struktur oder das
`<file>`-Element auf die `<amdSec>`-Unterelemente, die sie beschreiben,
verweisen können. So könnte man etwa ein `<techMD>`-Element benutzen, um
technische Metadaten über die Erstellung des Datei aufzunehmen:

```xml
<techMD ID="AMD001">
  <mdWrap MIMETYPE="text/xml" MDTYPE="NISOIMG" LABEL="NISO Img. Data">
    <xmlData>
      <niso:MIMEtype>image/tiff</niso:MIMEtype>
      <niso:Compression>LZW</niso:Compression>
      <niso:PhotometricInterpretation>8</niso:PhotometricInterpretation>
      <niso:Orientation>1</niso:Orientation>
      <niso:ScanningAgency>NYU Press</niso:ScanningAgency>
    </xmlData>
  </mdWrap>
</techMD>
```

Ein `<file>`-Element innerhalb einer `<fileGrp>` kann dann diese
Verwaltungsangaben oder Metadaten als zu der Datei gehörig kennzeichnen,
indem mit dem ADMIN-Attribut auf dieses `<techMD>`-Elemente verwiesen
wird:

```xml
<file ID="FILE001" ADMID="AMD001">
  <FLocat LOCTYPE="URL" xlink:href="http://dlib.nyu.edu/press/testimg.tif" />
</file>
```

## <span id="filegrp">Dateienabschnitt (File Section)</span>

Der Dateienabschnitt (`<fileSec>`) enthält eine oder mehrere
`<fileGrp>`-Elemente, mit denen zusammengehörige Dateien
zusammengehalten werden können. Ein `<fileGrp>`-Element listet alle
Dateien auf, die eine einzelne elektronische Version eines digitalen
Objekts ausmachen. So können getrennte `<fileGrp>`-Elemente für die
Thumbnail, die Masterimages, die PDF-Versionen und den TEI-kodierten
Text genutzt werden.

Das folgende Beispiel enthält den Dateienabschnitt eines digitalen
Objektes aus dem Bereich der Oral History mit drei verschiedenen
Versionen: die TEI-kodierte Transkription, die Master-Audiodatei im
WAV-Format und eine umgewandelte Datei im MP3-Format.

```xml 
<fileSec>
  <fileGrp ID="VERS1">
    <file ID="FILE001" MIMETYPE="application/xml" SIZE="257537" CREATED="2001-06-10T00:00:00Z">
      <FLocat LOCTYPE="URL" xlink:href="http://dlib.nyu.edu/tamwag/beame.xml" />
    </file>
  </fileGrp>
  <fileGrp ID="VERS2">
    <file ID="FILE002" MIMETYPE="audio/wav" SIZE="64232836"
      CREATED="2001-05-17T00:00:00Z" GROUPID="AUDIO1">
      <FLocat LOCTYPE="URL" xlink:href="http://dlib.nyu.edu/tamwag/beame.wav" />
    </file>
  </fileGrp>
  <fileGrp ID="VERS3" VERSDATE="2001-05-18T00:00:00Z">
    <file ID="FILE003" MIMETYPE="audio/mpeg" SIZE="8238866"
      CREATED="2001-05-18T00:00:00Z" GROUPID="AUDIO1">
      <FLocat LOCTYPE="URL" xlink:href="http://dlib.nyu.edu/tamwag/beame.mp3" />
    </file>
  </fileGrp>
</fileSec>
```

In diesem Fall umfasst das `<fileSec>`-Element drei untergeordnete
`<file-Grp>`-Elemente, je eines für jede einzelne Version des Objektes.
Das erste ist eine XML-kodierte Transkriptionsdatei, das zweite die
Master-Audiodatei im WAV-Format und das dritte eine Umwandlung davon im
MP3-Format. Wenn auch ein so schlichtes Beispiel die
`<fileGrp>`-Elemente nicht wirklich zu benötigen scheint, um die
verschiedenen Versionen des Objektes zu unterscheiden, so wird die
`<fileGrp>` bei Dokumenten mit einer grossen Anzahl gescannter
Seitenimages oder in Fällen, in denen eine einzelne Version aus einer
grossen Anzahl von Dateien besteht, umso nützlicher sein. In diesen
Fällen erleichtert die Möglichkeit zur Aufteilung der Dateien in
`<fileGrp>`-Elemente die Identifikation der Dateien, die zu einer
bestimmten Version gehören, deutlich.

Im Beispiel sind die Werte der GROUPID im `<file>`-Element bei den
beiden Audiodateien identisch; damit wird angezeigt, dass beide Dateien,
obwohl sie zu verschiedenen Versionen des digitalen Objektes gehören,
dieselben Basisinformationen enthalten. (Man kann das GROUPID-Attribut
genauso für gleichartige Seitenimages in digitalen Objekten mit vielen
gescannten Seiten verwenden.)

Es ist ebenfalls zu bemerken, dass alle `<file>`-Elemente eigene
ID-Attribute besitzen. Dieses Attribut liefert eine einmalige, interne
Bezeichnung für die Datei, auf die sich andere Teile des Dokuments
beziehen können. Diese Funktion wird im Zusammenhang des
Strukturabschnittes genauer erläutert.

Es sollte erwähnt werden, dass `<file>`-Elemente statt des
`<FContent>`-Elements auch ein `<FLocat>`-Element enthalten können.
`<FContent>`-Elemente werden benutzt, um den Inhalt der Datei in das
METS-Dokument einzubetten. Falls das beabsichtigt wird, muss der
Dateiinhalt entweder im XML-Format oder Base64-kodiert sein. Auch wenn
üblicherweise Dateien nicht eingebettet werden, wenn ein METS-Dokument
für eine Präsentation digitaler Objekte verwendet werden soll, kann es
für einen Austausch digitaler Objekte zwischen Sammlungen oder für
off-line zu speichernde Archivierungsversionen der digitalen Objekte
nützlich sein.

## <span id="structmap">Strukturbeschreibung (Structural Map)</span>

Die Strukturbeschreibung eines METS-Dokumentes beschreibt die
hierarchische Struktur, die den Nutzern eines digitalen Objektes zur
Navigation innerhalb des Objektes angeboten werden kann. Das
`<structMap>`-Element kodiert die Hierarchie als ineinander
verschachtelte Serie von `<div>`-Elementen. Jedes `<div>`-Element nutzt
Attribute, um die Art des Strukturelements festzulegen. Es kann außerdem
mehrere METSpointer- (`<mptr>`) und Dateiverweiselemente
(`<fptr>`)-enthalten und damit die dem `<div>`-Element entsprechenden
Inhalte kennzeichnen. METS-Verweise bezeichnen separate METS-Dokumente,
in denen sich die zugehörigen Informationen über die Dateien befinden.
Das kann nützlich sein, wenn große Materialsammlungen, (z.B. eine
vollständige Zeitschrift) erfasst werden, um damit die Größe jeder
einzelnen METS-Datei relativ klein zu halten. Dateiverweise bestimmen
zugehörige Dateien (oder in einigen Fällen auch Gruppen von Dateien bzw.
Teile einer Datei), welche in der `<fileSec>` desselben METS-Dokuments
definiert sind und zu der Stelle in der Hierarchie gehören, die das
akuelle `<div>` repräsentiert.

Im Folgenden wird das Beispiel einer sehr einfachen Struktur gezeigt:


```xml 
<structMap TYPE="logical">
  <div ID="div1" LABEL="Oral History: Mayor Abraham Beame" TYPE="oral history">
    <div ID="div1.1" LABEL="Interviewer Introduction" ORDER="1">
      <fptr FILEID="FILE001">
        <area FILEID="FILE001" BEGIN="INTVWBG" END="INTVWND" BETYPE="IDREF"/>
      </fptr>
      <fptr FILEID="FILE002">
        <area FILEID="FILE002" BEGIN="00:00:00" END="00:01:47" BETYPE="TIME"/>
      </fptr>
      <fptr FILEID="FILE003">
        <area FILEID="FILE003" BEGIN="00:00:00" END="00:01:47" BETYPE="TIME"/>
      </fptr>
    </div>
    <div ID="div1.2" LABEL="Family History" ORDER="2">
      <fptr FILEID="FILE001">
        <area FILEID="FILE001" BEGIN="FHBG" END="FHND" BETYPE="IDREF"/>
      </fptr>
      <fptr FILEID="FILE002">
        <area FILEID="FILE002" BEGIN="00:01:48" END="00:06:17" BETYPE="TIME"/>
      </fptr>
      <fptr FILEID="FILE003">
        <area FILEID="FILE003" BEGIN="00:01:48" END="00:06:17" BETYPE="TIME"/>
      </fptr>
    </div>
    <div ID="div1.3" LABEL="Introduction to Teachers' Union" ORDER="3">
      <fptr FILEID="FILE001">
        <area FILEID="FILE001" BEGIN="TUBG" END="TUND" BETYPE="IDREF"/>
      </fptr>
      <fptr FILEID="FILE002">
        <area FILEID="FILE002" BEGIN="00:06:18" END="00:10:03" BETYPE="TIME"/>
      </fptr>
      <fptr FILEID="FILE003">
        <area FILEID="FILE003" BEGIN="00:06:18" END="00:10:03" BETYPE="TIME"/>
      </fptr>
    </div>
  </div>
</structMap>
```

Diese Struktur zeigt, dass es um ein Oral-History-Interview handelt (mit
dem Bürgermeister von New York City Abraham Beame), das drei inhaltliche
Abschnitte umfasst: eine Einführung durch den Interviewer, etwas zur
Familiengeschichte von Bürgermeister Beame und eine Diskussion darüber,
wie es zu seinem Engagement in der Lehrergewerkschaft in New York kam.
Jede dieser Unterteilungen ist mit drei Dateien verknüpft (entsprechend
dem obigen Beispiel für Dateigruppen): eine XML-Transkription, eine
Master- und eine Derivat-Audio-Datei. Dazu wird ein ergänzendes
`<area>`-Element genutzt, um in jedem `<fptr>`-Element den
entsprechenden Bereich innerhalb der Datei zu identifizieren. Zum
Beispiel ist die erste Unterteilung (die Einleitung des Interviewers)
mit einem Teil der der XML-Transkriptionsdatei (Datei FILE001)
verknüpft, deren Begrenzung die ID-Attribute INTVWBG und INTVWND
festlegen. Außerdem ist der erste Abschnitt mit zwei verschiedenen
Audio-Dateien verknüpft; in diesem Fall sind anstatt der
ID-Attributwerte Anfangs- und Endpunkt des verknüpften Materials
innerhalb der Dateien mit einem einfachen Zeitcodewert in der Form
HH:MM:SS angegeben. So befindet sich die Einleitung des Interviewers in
beiden Audio-Dateien in dem Abschnitt, der mit 00:00:00 beginnt und bis
zum Zeitpunkt 00:01:47 andauert.

## <span id="structlink">Strukturverknüpfungen (Structural Links)</span>

Der Abschnitt für die Strukturverknüpfungen hat die einfachste Form
aller Hauptabschnitte des METS-Formats, weil er nur ein Element
enthält:`<smLink>` (auch wenn das Element wiederholt werden kann).
Dieser Abschnitt hat den Zweck, das Vorhandensein von Hyperlinks
zwischen Einheiten in der Struktur, also normalerweise den
`<div>`-Elementen, festzuhalten. Das ist speziell für den Fall der
Webseitenarchivierung eine nützliche Funktion, da die Hypertextstruktur
der Seite unabhängig von den HTML-Dateien erhalten bleiben kann.

Als Beispiel kann ein METS-Dokument für eine Webseite mit einem Bild,
das mit einer anderen Seite verlinkt ist, dienen. Die Struktur würde
`<div>`-Elemente wie im folgenden Beispiel für zwei Seiten enthalten:

```xml 
<div ID="P1" TYPE="page" LABEL="Page 1">
  <fptr FILEID="HTMLF1"/>
  <div ID="IMG1" TYPE="image" LABEL="Image Hyperlink to Page 2">
    <fptr FILEID="JPGF1"/>
  </div>
</div>

<div ID="P2" TYPE="page" LABEL="Page 2">
  <fptr FILEID="HTMLF2"/>
</div>
```

Falls angegeben werden soll, dass die Bilddatei im `<div>`-Element, das
dem `<div>`-Element für die erste Seite untergeordnet ist, mit der
HTML-Datei in dem `<div>`-Element für die zweite Seite verknüpft ist,
würde ein `<smLink>`-Element innerhalb einer Strukturverknüpfung, also
in dem `<structLink>`-Element, wie folgt eingesetzt werden:

```xml 
<smLink xlink:from="IMG1" xlink:to="P2" xlink:title="Hyperlink from 
  JPEG Image on Page 1 to Page 2" xlink:show="new"
  xlink:actuate="onRequest" />
```

Das `<smLink>`-Element benutzt eine leicht modifizierte Form der
XLink-Syntax; alle XLink-Attribute werden verwendet. Lediglich die
beiden Richtungsattribute "to" und "from" werden anders als in der
originalen XLink-Spezifikation als IDREF-Typ statt NMTOKEN benutzt.
Dadurch kann das Vorhandensein von Links zwischen beliebigen Knoten in
der Struktur angegeben werden. Außerdem können Werkzeuge für
XML-Verarbeitung genutzt werden, um die Existenz der verknüpften Knoten
zu überprüfen.

## <span id="behavior">Verhaltensabschnitt (Behavior)</span>

Ein Abschnitt zum Verhalten kann eingefügt werden, um ausführbare
Aktionen mit dem Inhalt eines METS-Dokumentes zu verbinden. Ein
Verhaltensabschnitt enthält ein oder mehrere Verhaltens-Elemente
`<behavior>`, von denen jedes ein Element für eine
Schnittstellendefinition enthält, das die abstrakte Definition eines
Verhaltenskomplexes darstellt, die in einem eigenen Verhaltensabschnitt
dargestellt wird. Ein Verhaltenselement `<behavior>` enthält auch ein
`<mechanism>`-Element, das dafür verwendet wird, um auf ein Modul von
ausführbarem Programmcode zu verweisen, mit dem das in der
Schnittstellendefinition abstrakt definierte Verhalten umgesetzt wird.

Das Verhalten von digitalen Objekten kann als Verknüpfung zu verteilten
Webdiensten eingesetzt werden, wie es in dem folgenden Beispiel aus dem
[Fedora-Projekt](http://www.fedora.info/) der Mellonstiftung verwendet
wird:

```xml 
<behavior ID="DISS1.1" STRUCTID="S1.1" BTYPE="uva-bdef:stdImage"
  CREATED="2002-05-25T08:32:00" LABEL="UVA Std Image Disseminator"
  GROUPID="DISS1" ADMID="AUDREC1">

  <interfaceDef LABEL="UVA Standard Image Behavior Definition"
    LOCTYPE="URN" xlink:href="uva-bdef:stdImage"/>

  <mechanism LABEL="A NEW AND IMPROVED Image Mechanism"
    LOCTYPE="URN" xlink:href="uva-bmech:BETTER-imageMech"/>
</behavior>
```

Vergleiche dazu:

  - [The Fedora Technical
    Specification](http://www.fedora.info/documents/master-spec-12.20.02.pdf)
    (pdf)

  - [Sample Digital
    Object](http://www.fedora.info/documents/obj-image-userinput-archdraw.xml)
    (encoded using METS)

  - [Sample Behavior Definition
    Object](http://www.fedora.info/documents/bdef-image-userinput.xml)
    (encoded using METS)

  - [Sample Behavior Mechanism
    Object](http://www.fedora.info/documents/bmech-image-userinput-mrsid.xml)
    (encoded using METS)

## Zusammenfassung

Das METS-Schema liefert einen flexiblen Mechanismus für die Kodierung
von Erschließungs-, Verwaltungs- und Strukturangaben eines digitalen
Objekts und für die Beschreibung der komplexen Bezüge zwischen diesen
verschiedenen Metadatenkategorien und der einzelnen Bestandteile des 
digitalen Objekts. Es kann deshalb einen nützlichen
Standard für den Austausch von digitalen Objekte zwischen
unterschiedlichen Sammlungen liefern. Außerdem kann METS digitale
Objekte mit Verhalten und Diensten verknüpfen. Diese Beschreibung
benennt die hauptsächlichen Funktionen des Schemas. Doch ist ein
gründliches Studium des Schemas und der darin enthaltenen Dokumentation
nötig, um das ganze Spektrum seiner Möglichkeiten zu verstehen.

Übersetzung: Angelika Menne-Haritz, Juli 2005  
Dank an Markus Enders für Hinweise zur Übersetzung.
