# Tutorial METS: quadro generale

## Introduzione

METS fornisce fornisce un'infrastruttura, per l'integrazione di insiemi complessi di metadati, che sono necessari al mantenimento di una biblioteca di oggetti digitali. Questi metadati possono sia estendere, quelli usati per gestire le collezioni di opera a stampa e di altri materiali fisici, che differenziarsi da essi. Ad esempio, un libro non si dissolve in una serie di pagine sconnesse, nel caso in cui il suo possessore dovesse incorrere in qualche errore nel memorizzare i metadati strutturali su come e' organizzato il libro, ne' gli studiosi sarebbero impossibilitati nel valutare il libro, se non fossero presenti le annotazioni riguardanti la sua produzione. Lo stesso non puo' dirsi per la versione digitale dello stesso libro. Senza i metadati strutturali, le immagini delle pagine o i file di testo di cui e' costituito un lavoro digitale sono di scarsa utilita', e senza i metadati tecnici riguardanti il processo di digitalizzazione, gli studiosi potrebbero non essere sicuri riguardo all'accuratezza della versione digitale, rispetto a quella dell'originale. Inoltre, per necessita' di gestione interna, un'organizzazione deve avere accesso a metadati tecnici appropriati per rinnovare e migrare periodicamente i dati, assicurando che la risorsa abbia un valore durevole.

Un documento METS e' costituito da sette sezioni principali:

1.  [**Intestazione METS**](#MHead) - Questa sezione contiene i metadati che descrivono il documento METS stesso, includendo alcune informazioni quali autore, editore, la data dell'ultima modifica, ecc.

2.  [**Metadati Descrittivi**](#descMD) - La sezione dei metadati descrittivi potrebbe sia puntare ad un documento METS esterno (p.e.,
    un record MARC in un OPAC oppure un record EAD di uno strumento di ricerca gestito da un server web), che includere direttamente i metadati descrittivi, oppure prevedere entrambe le soluzioni (sia puntare ad una risorsa esterna, che includerli internamente). Inoltre, è possibile includere ripetizioni multiple di entrambi i metadati descrittivi (sia interni che esterni).

3.  [**Metadati Amministrativi**](#admMD) - La sezione dei metadati amministrativi contiene informazioni su come i files sono stati creati e archiviati, sui diritti di proprieta' intellettuale che sono ancora attivi, sull'oggetto di origine da cui deriva l'oggetto digitale, e sulla provenienza dei file che rappresentano gli oggetti digitali (ad esempio, le relazioni tra file master e file di derivazione, o le informazioni riguardanti la migrazione e la trasformazione). Analogamente ai metadati descrittivi, i metadati amministrativi possono essere sia puntati esternamente al documento METS, che codificati internamente.

4.  [**Sezione File**](#filegrp) - La sezione file e' costituita da una lista di tutti i file di contenuto e può comprendere le diverse versioni dell'oggetto digitale. Gli elementi `<file>` possono essere raggruppati all'interno di elementi `<fileGrp>`, per suddividerli in base alle diverse versioni.

5.  [**Mappa Strutturale**](#structmap) - La mappa strutturale e' il cuore del documento METS, ed è l'unica sezione obbligatoria dello stesso. Essa mette in evidenza la struttura gerarchica a cui appartiene l'oggetto digitale, e collega gli elementi di quella struttura ai file di contenuto ed ai metadati appartenenti ad ogni elemento. Lo schema METS, permette di includere nel documento più mappe strutturali.

6.  [**Link Strutturali**](#structlink) - La sezione dei link strutturali METS permette, a chi crea il METS, di memorizzare l'esistenza di hyperlink tra i nodi della gerarchia, descritta dalla sezione della mappa strutturale. Cio' e' di particolare importanza, quando METS viene usato per archiviare siti web.

7.  [**Comportamento**](#behavior) - Una sezione di comportamento puo' essere usata per associare i comportamenti al contenuto
    dell'oggetto METS. Ogni comportamento, descritto in questa sezione, ha un elemento di definizione di interfaccia che rappresenta una
    definizione astratta dell'insieme di comportamenti rappresentati da una particolare sezione comportamento. Ogni comportamento ha anche un elemento di meccanismo che identifica un modulo di codice eseguibile, che implementa ed esegue i comportamenti definiti, in modo  astratto dalla definizione dell'interfaccia.

Segue un'ulteriore spiegazione di dettaglio per ogni sezione e per le relazioni che intercorrono tra di esse.

## <span id="MHead">Intestazione METS</span>

<span id="#MHead">L'elemento di intestazione del METS permette di memorizzare i metadati descrittivi minimi sull'oggetto METS stesso
all'interno di un documento METS. Tali metadati includono la data di creazione del documento METS, la data della sua ultima modifica, e lo status del documento METS. In questa sezione vengono inoltre memorizzati uno o piu' entita', che hanno avuto un ruolo particolare,
rispetto al documento METS, specificandone il ruolo rivestito, ed aggiungendo una piccola nota riguardo la loro responsabilita'.  Inoltre, si possono memorizzare una varieta' di identificatori alternativi del documento METS, come supplemento all'identificativo primario per il documento METS memorizzato nell'attributo OBJID dell'elemento root. Un esempio dell'intestazione METS potrebbe essere il seguente:  
</span>

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

<span id="#MHead">Tale esempio contiene due attributi sull'elemento `<metsHdr>` , CREATEDATE e RECORDSTATUS, che sono utilizzati per
indicare la data e l'ora in cui il record METS e' stato creato, e lo status di elaborazione del record.  Vengono elencate, inoltre, due entita' individuali del record METS, la persona responsabile di aver creato il record e quella responsabile per il materiale originale. Entrambi gli attributi "ROLE" e "TYPE" nell'elemento `<agent>` usano un vocabolario controllato. I valori possibili per l'attributo ROLE includono "ARCHIVIST", "CREATOR", "CUSTODIAN", "DISSEMINATOR", "EDITOR", "IPOWNER" ed "OTHER". I valori per l'attributo TYPE possono essere "INDIVIDUAL" , "ORGANIZATION" o "OTHER."</span>

## <span id="descMD">Metadati Descrittivi</span>

La sezione dei metadati descrittivi di un documento METS e' costituita da uno o piu' elementi `<dmdSec>` (Descriptive Metadata Section). Ogni elemento `<dmdSec>` potrebbe sia contenere un puntatore a metadati esterni (elemento `<mdRef>`), che includere i metadati internamente (nell'elemento `<mdWrap>`), oppure puo' contenerli entrambi.

**I metadati descrittivi esterni (mdRef):** un elemento mdRef fornisce un URI che identifica la locazione dei metadati esterni. Per esempio, il seguente riferimento di metadati punta ad un particolare oggetto digitale:

```xml 
<dmdSec ID="dmd001">
  <mdRef LOCTYPE="URN" MIMETYPE="application/xml" MDTYPE="EAD"
    LABEL="Berol Collection Finding Aid" xlink:href="urn:x-nyu:fales1735" />
</dmdSec>       
```

L'elemento `<mdRef>` di questa `<dmdSec>` contiene quattro attributi. L'attributo LOCTYPE specifica il tipo di locatore contenuto nel corpo dell'elemento; valori validi per LOCTYPE includono 'URN', 'URL', 'PURL', 'HANDLE', 'DOI', e 'OTHER'. L'attributo MIMETYPE specifica il tipo MIME per i metadati descrittivi esterni, e MDTYPE indica il formato dei metadati. Valori validi per l'elemento MDTYPE sono MARC, MODS, EAD, VRA (VRA Core), DC (Dublin Core), NISOIMG (NISO Technical Metadata for Digital Still Images), LC-AV (Library of Congress Audiovisual Metadata) , TEIHDR (TEI Header), DDI (Data Documentation Initiative), FGDC (Federal Geographic Data Committee Metadata Standard \[FGDC-STD-001-1998\] ), e OTHER. 
LABEL fornisce il meccanismo per descrivere questi metadati a coloro che visualizzano il documento METS, per esempio la visualizzazione del documento in una 'Tabella di contenuti'.

**I metadati descrittivi interni (mdWrap):** Un elemento mdWrap e' un contenitore dei dati inseriti direttamente nel documento METS. Tali metadati possono assumere due forme: 1) codificati in XML, con il codice identificativo dell'XML stesso appartenente ad un namespace diverso da quello del METS, oppure 2) qualsiasi arbitraria forma binaria o di testo, considerato che i metadati sono codificati come Base64 e contenuti in un elemento `<binData>` all'interno dell'elemento mdWrap. 
I seguenti esempi mostrano l'uso dell'elemento mdWrap:

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

E' da notare che tutti gli elementi `<dmdSec>` devono possedere un attributo ID. Questo attributo e' un identificativo univoco interno per ogni elemento `<dmdSec>`, e che puo' essere usato nella mappa strutturale per collegare una particolare divisione della gerarchia del documento, ad un particolare elemento `<dmdSec>`. Cio' permette a sezioni specifiche di metadati descrittivi di essere collegati a parti specifiche dell'oggetto digitale.

## <span id="admMD">Metadati Amministrativi</span>

Gli elementi `<amdSec>` contengono sia i metadati amministrativi, relativi ai file che costituiscono l'oggetto digitale, che quelli
relativi al materiale originale di provenienza. Ci sono quattro tipologie principali di metadati amministrativi per un documento METS: 1) metadati tecnici (informazioni riguardanti la creazione, il formato e le caratteristiche di utilizzo), 2) metadati sulla proprieta' intellettuale (copyright e informazioni sulle licenze d'uso), 3) metadati riguardanti l'origine (descrittivi ed amministrativi riguardanti l'origine analogica di derivazione dell'oggetto digitale), e 4) metadati sulla provenienza digitale (informazioni sulle relazioni dei file sorgente e di quelli di destinazione, sulle relazioni tra file master e di derivazione e sui file impiegati nella migrazione/trasformazione tra la digitalizzazione originale di un artefatto e la sua "incarnazione" come oggetto digitale). Ognuna di queste quattro diverse tipologie di metadati amministrativi, ha un subelemento di `<amdSec>`, dedicato all'inserimento dei metadati specifici: `<techMD>`, `<rightsMD>`, `<sourceMD>` e `<digiprovMD>`.  Ognuno di questi elementi potrebbe ripetersi piu' di una volta in un documento METS.

Gli elementi `<techMD>`, `<rightsMD>`, `<sourceMD>` e `<digiprovMD>` seguono lo stesso modello di contenuto dell'elemento `<dmdSec>`: essi potrebbero contenere un elemento `<mdRef>` per puntare ai metadati amministrativi esterni, oppure usare un elemento `<mdWrap>` per i metadati amministrativi interni, oppure essere utilizzati entrambi. Istanze multiple di queste sottosezioni potrebbero ripetersi, ed
ognuna di loro deve essere corredata di un attributo ID, in modo che altri elementi del documento METS (cosi' come le divisioni della mappa strutturale o come gli elementi `<file>`) possano essere collegati ai sottoelementi `<amdSec>` a cui si riferiscono. Si potrebbe, per esempio, avere un elemento `<techMD>` che include i metadati tecnici riguardanti le modalita' di produzione di un file:

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

Un elemento `<file>` all'interno di `<fileGrp>` potrebbe successivamente identificare questi metadati amministrativi come appartenenti al file attraverso l'attributo ADMID, che punta all'elemento `<techMD>`:

```xml
<file ID="FILE001" ADMID="AMD001">
  <FLocat LOCTYPE="URL" xlink:href="http://dlib.nyu.edu/press/testimg.tif" />
</file>
```

## <span id="filegrp">Sezione File</span>

La sezione file (`<fileSec>`) contiene uno o piu' elementi `<fileGrp>` usati per raggruppare i file correlati. Un elemento `<fileGrp>` fornisce l'elenco di tutti i file che costituiscono una singola versione elettronica dell'oggetto digitale. Per esempio,
potrebbero essere separate da elementi `<fileGrp>` le immagini per le gallerie di immagini, quelle master di archivio e le versioni pdf, le versioni TEI, etc.

Consideriamo l'esempio che segue, come una sezione file, da un oggetto digitale di un racconto orale di cui si hanno tre
diverse rappresentazioni: una trascrizione a codifica TEI, un file master audio in formato WAV, ed un file di derivazione in formato MP3:

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

In questo caso la `<fileSec>` contiene tre elementi di raggruppamento `<fileGrp>`, uno per ogni rappresentazione dell'oggetto. Il primo e' il file trascritto in codice XML, il secondo e' un master audio in formato Wav ed il terzo e' il derivato in formato MP3. Sebbene in tale esempio, gli elementi `<fileGrp>` per distinguere le diverse rappresentazioni non sembrino essere utili, `<fileGrp>` diventa molto piu' utile per oggetti costituiti da un numero cospicuo di immagini di pagine digitalizzate o invece in altri casi dove una singola rappresentazione e' costituita da un numero ingente di file. In quei casi, e' necessario suddividere gli elementi `<file>` nei `<fileGrp>` per identificare i file appartenenti ad una rappresentazione specifica del documento.

Si puo' notare la presenza dell'attributo GROUPID con un valore identico per i due elementi per i file audio `<file>`; cio' sta ad indicare che i due file, dal momento che appartengono a diverse rappresentazioni dell'oggetto, contengono le stesse informazioni di base (e' possibile usare GROUPID allo stesso modo, per indicare i file immagine di pagine equivalenti per quegli oggetti digitali che sono costituiti da molte immagini digitalizzate).

E' da notare, inoltre, che gli elementi `<file>` hanno un attributo ID univoco. Tale attributo costituisce un nome univoco, a cui possono fare riferimento le altre parti del documento. Un esempio del meccanismo di riferimento e' visibile nella sezione della Mappa Strutturale.

E' da notare che gli elementi `<file>` potrebbero possedere un elemento `<FContent>` invece di un elemento `<FLocat>`. Gli elementi `<FContent>` vengono usati per inserire i contenuti veri e propri del file nel documento METS, e devono essere codificati in formato XML o Base64. Sebbene incorporare i file non sia una pratica molto comune per preparare un documento METS, con lo scopo di mostrare un oggetto della biblioteca digitale all'utente, cio' puo' essere una caratteristica di una certa importanza per lo scambio di oggetti digitali tra depositi, o per archiviare le diverse versioni degli stessi a scopo conservativo.

## <span id="structmap">Mappa Strutturale</span>

La sezione della mappa strutturale di un documento METS definisce la struttura gerarchica degli oggetti digitali da presentare all'utente, in modo da permettergli di consultarli. L'elemento `<structMap>` codifica tale gerarchia con una serie nidificata di elementi `<div>`. Ogni elemento `<div>` e' corredato di un attributo informativo che specifica di che tipo di divisione si tratta, ed inoltre, puo' contenere puntatori METS multipli (`<mptr>`) e puntatori ad elementi file (`<fptr>`) per identificare il contenuto corrispondente a quella specifica divisione `<div>`. I puntatori METS specificano documenti METS separati, come contenitori di informazioni del file, specifiche per la `<div>` che li contiene. Cio' puo' essere utile per codificare ampie collezioni di materiali (ad esempio, un intero giornale) conservando traccia delle dimensioni di ogni file METS, in un insieme relativamente piccolo. I puntatori ai file specificano i file (o in alcuni casi, sia gruppi di file, che locazioni specifiche all'interno di un file) all'interno del documento METS corrente. La sezione `<fileSec>`corrisponde alla parte di gerarchia rappresentata dalla `<div>` che ne collega gli identificativi.

Di seguito, l'esempio di una semplice mappa strutturale:

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

Questa mappa strutturale definisce la gerarchia di un racconto orale (del sindaco di New York City Abraham Beame) e che comprende tre
sottosezioni: la presentazione introduttiva dell'intervistatore, alcuni racconti familiari del Sindaco Beame, ed un dibattito su come e' stato coinvolto dall'unione degli insegnanti di New York.
Ognuna delle sottosezioni/divisioni e' collegata a tre file (presi dall'esempio precedente "file groups" della sezione file): una trascrizione in codice XML, un master ed un file audio derivato. L'elemento sussidiario `<area>`, viene usato in ogni `<fptr>` per
indicare che questa divisione corrisponde solo ad una parte del file collegato e per identificare la parte specifica di ogni file collegato. Per esempio, la prima divisione (l'introduzione dell'intervistatore ) e' collegata alla parte del file di trascrizione XML (FILE001) che si trova tra due tag identificate rispettivamente da "INTVWBG" e "INTVWND". La prima divisione è inoltre collegata, a due differenti file audio; in questi casi piuttosto che specificare gli identificativi (ID) dell'attributo all'interno dei file
collegati, i punti d'inizio e di fine del materiale collegato all'interno dei file sono indicati da un semplice codice di temporizzazione nel formato HH:MM:SS. Cosi', l'introduzione dell'intervistatore, puo' essere trovata in entrambi i file audio e nel
segmento individuato dal punto iniziale con cronometro uguale a 00:00:00, fino ad arrivare al tempo cronometrico di 00:01:47.

## <span id="structlink">Link Strutturali</span>

La sezione dei link strutturali del formato METS e' la piu' semplice tra le altre sezioni principali del documento METS, dal momento che contiene un singolo elemento `<smLink>` (sebbene tale elemento possa essere ripetuto). La sezione dei link strutturali del METS e' stata creata per permettere di memorizzare la presenza di hyperlink tra gli elementi costitutivi di una mappa strutturale `<div>`. Questo meccanismo facilita l'utilizzo dello standard METS per archiviare siti web e per conservarne la struttura ipertestuale, separatamente dai file HTML del sito stesso.

Facendo un esempio, consideriamo il caso di un documento METS per una pagina web contenente un'immagine che e' collegata con un hyperlink ad un'altra pagina.  L'elemento `<structMap>` probabilmente conterrebbe divisioni `<div>`, come nell'esempio seguente:

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

Se si volesse indicare che il file immagine nella `<div>` contenuta nella prima pagina, `<div>` viene collegata al file HTML della seconda pagina `<div>`, si avrebbe un elemento `<smLink>` all'interno della sezione `<structLink>` del documento METS:

```xml 
<smLink xlink:from="IMG1" xlink:to="P2" xlink:title="Hyperlink from 
  JPEG Image on Page 1 to Page 2" xlink:show="new"
  xlink:actuate="onRequest" />
```

L'elemento `<smLink>` usa una forma leggermente modificata della sintassi XLink; vengono usati tutti gli attributi XLink ma gli attributi "to" e "from" sono dichiarati per essere di tipo IDREF piuttosto che NMTOKEN come la specifica originale XLink. Cio' ci permette di indicare l'esistenza di link tra due nodi qualsiasi, nella mappa strutturale, ed inoltre di usare gli strumenti XML di processamento per dare conferma che il nodo collegato esista realmente.

## <span id="behavior">Sezione Comportamento</span>

Una sezione comportamento puo' essere usata per associare comportamenti eseguibili, al contenuto dell'oggetto METS. Una sezione comportamento contiene uno o piu' elementi `<behavior>`, ognuno dei quali ha una definizione di interfaccia che rappresenta una definizione astratta dell'insieme di comportamenti rappresentati in una particolare sezione.
Una sezione `<behavior>` ha, inoltre, un elemento `<mechanism>` che viene usato per puntare ad un modulo di codice eseguibile che implementa ed esegue il comportamento definito in modo astratto dalla definizione di interfaccia.

I comportamenti degli oggetti digitali possono essere implementati come collegamenti a servizi web, distribuiti come nell'esempio seguente fornito dal progetto della Mellon [Fedora](http://www.fedora.info/).

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

Altri riferimenti utili per questa sezione :

  - [Specifiche tecniche di
    Fedora](http://www.fedora.info/documents/master-spec-12.20.02.pdf)
    (pdf)

  - [Esempio di oggetto
    digitale](http://www.fedora.info/documents/obj-image-userinput-archdraw.xml)
    (codificato METS)

  - [Esempio di definizione del comportamento
    dell'oggetto](http://www.fedora.info/documents/bdef-image-userinput.xml)
    (codificato METS)

  - [Esempio di meccanismo del comportamento
    dell'oggetto](http://www.fedora.info/documents/bmech-image-userinput-mrsid.xml)
    (codificato METS)

## Conclusione

Lo schema METS definisce un meccanismo flessibile per la codifica di metadati descrittivi, amministrativi e strutturali per gli oggetti digitali, e per documentare le relazioni complesse tra i vari formati di metadati. Esso puo', inoltre, fornire un utile standard
per lo scambio tra depositi, di oggetti digitali. Infine, lo schema METS offre la capacita' di associare un oggetto digitale a comportamenti o servizi. Questo tutorial, mette in risalto le principali caratteristiche dello schema METS, ma un'approfondita disamina dello schema e della sua documentazione inclusa, si rende necessaria per capirne l'ampio spettro di potenzialita'.

-----

Questo documento è la traduzione del documento "[**METS: An Overview & Tutorial**](//www.loc.gov/standards/mets/METSOverview.v2.html)",
pubblicato il 2 Giugno 2004.  
La traduzione è stata curata da [Angela Di Iorio](/cdn-cgi/l/email-protection#92f3fcf5f7fef3bcf6fbfbfde0fbfdd2e7fcfbe0fdfff3a3bcfbe6)
nel corso del progetto per la biblioteca digitale ([S.I.M.B.A.D.](http://web-serv.provincia.campobasso.it/biblioteca/digitale/)),
realizzato per la Biblioteca Provinciale "P. Albino" di Campobasso (Italia).
La traduzione, è stata aggiornata nel Maggio 2019.
