# METS : vue d'ensemble & tutoriel

## Introduction

METS est un standard d'encodage de métadonnées descriptives, administratives et de structure pour décrire des objets numériques complexes, qu’ils aient pour contenu du texte, des images fixes ou des images animées. Conçu initialement par la [Digital Library Federation](http://www.diglib.org/), METS est un format basé sur XML pour encoder les métadonnées nécessaires à la gestion d’objets numériques dans un entrepôt et à l’échange de ces objets entre entrepôts, ou entre un entrepôt et ses utilisateurs. Un fichier METS peut aussi être utilisé pour accompagner un Paquet d’informations à verser (SIP), un Paquet d’informations archivé (AIP) ou un Paquet d’informations diffusé (DIP) selon la [norme OAIS](https://public.ccsds.org/Pubs/650x0m2%28F%29.pdf).
METS propose une structure cohérente pour intégrer l’ensemble complexe de métadonnées nécessaires à la gestion d’une bibliothèque numérique. Ces métadonnées sont à la fois plus étendues et différentes de celles utilisées pour gérer des collections d’imprimés ou d’autres objets physiques. Un livre, par exemple, ne se disloquera pas en une série de pages isolées si son propriétaire néglige de conserver les métadonnées de structure décrivant son organisation, pas plus que les universitaires n’échoueront à évaluer son intérêt si son créateur néglige de décrire comment il a été créé. On ne peut en dire autant de la version numérique de ce même livre. Sans métadonnées de structure, les fichiers images ou textuels des pages composant le livre sont de peu d’utilité, et sans métadonnées techniques sur le processus de numérisation, les universitaires peuvent être incapables d’évaluer la fidélité de la copie numérique à son original. Une organisation doit également avoir accès à des métadonnées techniques appropriées à ses besoins de gestion interne, par exemple pour migrer et convertir les données, garantissant ainsi la pérennité de ressources précieuses.

Un document METS comprend sept sections principales :
1.  [**En-tête METS**](#MHead) : l'en-tête METS contient des métadonnées  décrivant le document METS lui-même, ce qui inclut des informations sur la création et la modification du document, etc.
2.  [**Métadonnées descriptives**](#descMD) : la section des métadonnées descriptives peut pointer vers des métadonnées descriptives externes au document METS (par exemple une notice MARC dans un OPAC ou un instrument de recherche en EAD hébergé sur un serveur Web), ou contenir des métadonnées descriptives encapsulées dans le fichier, ou les deux. Il est possible d'inclure plusieurs sections de métadonnées externalisées et/ou encapsulées dans cette section des métadonnées descriptives.
3.  [**Métadonnées administratives**](#admMD) : la section des métadonnées administratives fournit des informations sur la manière dont les fichiers ont été créés et stockés, sur les droits de propriété intellectuelle, sur l'original dont l'objet numérique est dérivé et sur la provenance des fichiers constituant l'objet  numérique, c'est-à-dire les relations entre les fichiers master et les fichiers dérivés ainsi que les informations de migration ou de transformation. Comme les métadonnées descriptives, les métadonnées administratives peuvent être externes au document METS ou y être encapsulées.
4.  [**Section des fichiers**](#filegrp) : la section des fichiers liste tous les fichiers composant la version numérique de l'objet. Les éléments `<file>` peuvent être regroupés dans des éléments `<fileGrp>` qui permettent de grouper les fichiers correspondant à une même version de l'objet.
5.  [**Carte de structure**](#structmap) : la carte de structure est le cœur d'un document METS ; elle en est le seul élément obligatoire. Elle dessine une structure hiérarchique de l'objet numérique et relie chaque élément de cette structure aux fichiers et aux métadonnées qui s'y rapportent.
6.  [**Liens structurels**](#structlink) : la section des liens structurels de METS permet aux utilisateurs de documenter l'existence d'hyperliens entre différents nœuds de la hiérarchie dessinée par la carte de structure. C'est particulièrement utile si l'on utilise METS pour archiver des sites Web.
7.  [**Comportement**](#behavior) : une section de comportement peut être utilisée pour associer des exécutables au contenu d'un objet METS. Chaque comportement compris dans une section de comportement possède un élément « définition de l'interface », qui est une définition abstraite de l'ensemble des comportements représentés par une section de comportement distincte. Chaque comportement possède aussi un élément « mécanisme », qui décrit le module de code exécutable implémentant et exécutant les comportements définis de manière abstraite dans la définition de l'interface.

On trouvera ci-dessous une explication plus détaillée de chaque section et de leurs relations.

## <span id="MHead">En-tête METS</span>
L'élément en-tête METS permet d'enregistrer dans le document METS des métadonnées descriptives minimales sur le document METS lui-même. Ces métadonnées comprennent la date de création, la date de dernière modification et le statut du document METS. On peut également enregistrer le nom d'un ou plusieurs agents ayant joué un rôle dans l’histoire du document METS, préciser le rôle qu'ils ont joué et ajouter une note sur leur activité. Enfin, on peut y enregistrer une série d'identifiants alternatifs afin de compléter l'identifiant principal du document METS, qui figure dans l'attribut OBJID de l'élément racine METS. Un exemple simple d'en-tête METS aura l'aspect suivant :  
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

Cet exemple comprend deux attributs pour l'élément `<metsHdr>`, CREATEDATE et RECORDSTATUS, qui servent à indiquer respectivement la date et l'heure auxquelles le fichier METS a été créé et le statut du document METS. On y liste deux agents individuels qui ont travaillé sur ce fichier METS : la personne responsable de la création du fichier et un archiviste responsable du document original. Les deux attributs ROLE et TYPE de l'élément `<agent>` utilisent des vocabulaires contrôlés. Les valeurs autorisées de ROLE incluent "ARCHIVIST" (archiviste), "CREATOR" (créateur), "CUSTODIAN" (conservateur), "DISSEMINATOR" (diffuseur), "EDITOR" (éditeur), "IPOWNER" (propriétaire de l'adresse IP) et "OTHER" (autre). Les valeurs autorisées de l'attribut TYPE sont "INDIVIDUAL" (individu), "ORGANIZATION" (organisation) ou "OTHER" (autre).
## <span id="descMD">Métadonnées descriptives</span>
La section des métadonnées descriptives d'un document METS consiste en un ou plusieurs éléments `<dmdSec>` (section des métadonnées descriptives). Chaque élément `<dmdSec>` peut contenir un pointeur vers des métadonnées externes (élément `<mdRef>`), des métadonnées encapsulées (à l'intérieur d'un élément `<mdWrap>`), ou les deux.

**Métadonnées descriptives externes (mdRef) :** l'élément `<mdRef>` fournit une URI qui peut être utilisée pour récupérer les métadonnées externes. Par exemple, les métadonnées ci-dessous font référence à un instrument de recherche pour un objet numérique donné :
```xml 
<dmdSec ID="dmd001">
  <mdRef LOCTYPE="URN" MIMETYPE="application/xml" MDTYPE="EAD"
    LABEL="Berol Collection Finding Aid" xlink:href="urn:x-nyu:fales1735" />
</dmdSec>       
```

L'élément `<mdRef>` de cette `<dmdSec>` comporte quatre attributs. L'attribut LOCTYPE précise le système de localisation contenu dans le corps de l'élément ; comme valeurs autorisées de LOCTYPE, on trouve 'URN', 'URL', 'PURL', 'HANDLE', 'DOI' et 'OTHER'. L'attribut MIMETYPE permet de préciser le type MIME de métadonnées descriptives externes, et MDTYPE permet d'indiquer le format de métadonnées auquel il est fait référence. Comme valeurs autorisées de MDTYPE, on trouve MARC, MODS, EAD, VRA (VRA Core), DC (Dublin Core), NISOIMG (NISO Technical Metadata for Digital Still Images), LC-AV (Library of Congress Audiovisual Metadata), TEIHDR (TEI Header), DDI (Data Documentation Initiative), FGDC (Federal Geographic Data Committee Metadata Standard \[FGDC-STD-001-1998\]), et OTHER. L'attribut LABEL fournit un mécanisme pour décrire ces métadonnées à quelqu'un qui visualise un fichier METS, par exemple en diffusant le fichier METS sous la forme d'une table des matières.

**Métadonnées descriptives internes (mdWrap) :** l'élément `<mdWrap>` fournit un élément conteneur encapsulant des métadonnées à l'intérieur d'un fichier METS. Ces métadonnées peuvent se présenter sous deux formes : 1. des métadonnées encodées en XML, l'encodage XML concerné déclarant un autre espace de noms que celui du document METS, ou 2. toute forme de données binaire ou textuelle, À CONDITION QUE les métadonnées soient encodées en Base64 et encapsulées dans un élément `<binData>` fils de l'élément `<mdWrap>`. Les exemples ci-dessous illustrent l'utilisation de l'élément `<mdWrap>`.

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

Remarque : tout élément `<dmdSec>` doit obligatoirement avoir un attribut ID. Cet attribut fournit pour chaque élément `<dmdSec>` une dénomination interne unique qui peut être utilisée dans la carte de structure pour relier une division dans la hiérarchie du document à cet élément `<dmdSec>`. Cela permet de relier des sections de métadonnées descriptives à des parties précises de l'objet numérique.

## <span id="admMD">Métadonnées administratives</span>

Les éléments `<amdSec>` comprennent les métadonnées administratives concernant les fichiers qui constituent l'objet numérique, ainsi que des métadonnées concernant l’original à partir duquel a été créé cet objet. Il y a quatre types principaux de métadonnées administratives prévus dans un fichier METS : 1. les métadonnées techniques (informations concernant la création, le format et les caractéristiques d'usage des fichiers), 2. les droits de propriété intellectuelle (informations de droits d’auteur et de licence d'utilisation), 3. les métadonnées de source (métadonnées descriptives et administratives concernant la source analogique dont l’objet numérique est dérivé) et 4. les métadonnées de provenance numérique (informations concernant les relations de source à résultat entre les fichiers, y compris les relations de master à dérivé, et les migrations ou transformations réalisées sur des fichiers entre la numérisation originale d'un artefact et sa manifestation actuelle sous forme d'un objet numérique). Chacun de ces quatre types différents de métadonnées administratives possède un sous-élément de la section `<amdSec>` du document METS dans lequel les métadonnées du type concerné peuvent être encapsulées : `<techMD>`, `<rightsMD>`, `<sourceMD>` et `<digiprovMD>`. Chacun de ces quatre éléments peut apparaître plus d'une fois dans un fichier METS.

Les éléments `<techMD>`, `<rightsMD>`, `<sourceMD>` et `<digiprovMD>` utilisent le même modèle que `<dmdSec>` : ils peuvent contenir un élément `<mdRef>` pour faire référence à des métadonnées administratives externes, un élément `<mdWrap>` pour encapsuler des métadonnées administratives à l'intérieur d'un document METS, ou les deux. Ces éléments peuvent être répétés dans un fichier METS, et chacun doit comporter un attribut ID afin que d'autres éléments dans le fichier METS (tels que des divisions de la carte de structure ou des éléments `<file>`) puissent être reliés aux éléments fils d'`<amdSec>` qui les décrivent. Par exemple, on pourrait avoir un élément `<techMD>` comprenant des métadonnées techniques sur la création d'un fichier :

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

L'élément `<file>` dans un `<fileGrp>` pourrait alors identifier les métadonnées administratives ci-dessus comme celles du fichier correspondant en utilisant un attribut ADMID qui fasse référence à l'élément `<techMD>` ci-dessus :

```xml
<file ID="FILE001" ADMID="AMD001">
  <FLocat LOCTYPE="URL" xlink:href="http://dlib.nyu.edu/press/testimg.tif" />
</file>
```

## <span id="filegrp">Section des fichiers</span>

La section des fichiers (`<fileSec>`) comprend un ou plusieurs éléments `<fileGrp>` qui servent à regrouper des fichiers de même nature. Un `<fileGrp>` liste tous les fichiers constituant une version distincte de l'objet numérique. Par exemple, on pourrait avoir des éléments `<fileGrp>` distincts pour les vignettes, les masters des images, les versions PDF, les versions textuelles encodées en TEI, etc.

Voici un exemple d'une section de fichiers décrivant un objet numérique ayant trois versions différentes : une transcription encodée en TEI, un fichier master audio au format WAV et un fichier audio dérivé au format MP3 :

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

Dans ce cas, la `<fileSec>` contient trois sous-éléments `<fileGrp>`, un pour chacune des trois versions différentes de l'objet. Le premier est un fichier de transcription du texte encodé en XML, le second est un fichier master audio au format WAV, et le troisième est un fichier audio dérivé au format MP3. Même si dans un exemple aussi simple il ne semble pas vraiment nécessaire d'utiliser des éléments `<fileGrp>` pour distinguer les différentes versions d'un objet, cet élément `<fileGrp>` devient beaucoup plus utile dans le cas d'objets composés d'un grand nombre d'images de pages numérisées, et en fait dans tout cas où une seule version de l'objet consiste en un grand nombre de fichiers. Dans ces cas-là, la possibilité de répartir les éléments `<file>` dans des `<fileGrp>` facilite l'identification des fichiers appartenant à une version particulière du document.

Vous avez peut-être remarqué la présence des attributs GROUPID avec des valeurs identiques pour les deux éléments `<file>` correspondant à un fichier audio ; ils indiquent que les deux fichiers, bien qu'appartenant à deux versions différentes de l'objet, contiennent pour l'essentiel la même information (on peut utiliser le GROUPID de la même manière pour indiquer que des fichiers images correspondent à la même page dans des objets numériques comprenant plusieurs images de chaque page numérisée).

Vous remarquerez également que tous les éléments `<file>` ont un attribut ID unique. Cet attribut fournit une dénomination unique à l’intérieur du fichier afin que d'autres portions du document puissent y faire référence. On verra ce type de référencement par l'exemple dans la section de la carte de structure.

Il convient de signaler que les éléments `<file>` peuvent contenir un élément `<FContent>` au lieu d'un élément `<FLocat>`. L'élément `<FContent>` est utilisé pour encapsuler le contenu même du fichier à l'intérieur du document METS ; si l'on fait cela, le contenu du fichier doit être soit dans un format XML soit encodé en Base64. Bien que l'inclusion de fichiers ne soit pas une pratique habituelle quand on crée un document METS pour diffuser des objets numériques à des utilisateurs, elle peut s'avérer une fonctionnalité intéressante pour les échanger entre différents entrepôts ou en archiver des copies dans un stockage hors site.

## <span id="structmap">Carte de structure</span>

La carte de structure d'un document METS définit une structure hiérarchique qui peut être diffusée aux utilisateurs d'un objet numérique afin de leur permettre de naviguer dans celui-ci. L'élément `<structMap>` définit cette hiérarchie comme une arborescence d'éléments `<div>` imbriqués. Chaque division `<div>` comprend un attribut spécifiant son type. Une division peut aussi comprendre des pointeurs vers un document METS (`<mptr>`) ou vers un fichier de données (`<fptr>`), qui permettent d'identifier le contenu correspondant à cette `<div>`. Les pointeurs METS référencent des documents METS distincts comprenant la description du contenu de la `<div>` courante. Cette disposition peut être utile lors de la description d'un grand ensemble d'objets (par exemple, la collection complète d'un périodique) pour conserver une taille relativement faible à chaque fichier METS de l'ensemble. Les pointeurs de fichiers référencent des fichiers (ou dans certains cas des groupes de fichiers, ou bien encore des emplacements précis dans un fichier) cités dans la section `<fileSec>`du document METS courant et correspondant à la partie de la hiérarchie représentée par la `<div>` courante.

Voici un exemple d'une carte de structure extrêmement simple :

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


La carte de structure ci-dessus indique que le document décrit est un entretien avec Abraham Beame, maire de la ville de New York, document qui comprend trois sous-sections : une introduction par l'intervieweur, une histoire de sa famille par le maire Beame et une discussion sur la façon dont il a été amené à être impliqué dans le syndicat des enseignants de New York. Chacune de ces sous-sections ou divisions est liée à trois fichiers (tirés de notre exemple précédent de groupes de fichiers) : une transcription XML, un master et un fichier audio dérivé. Un élément complémentaire `<area>` est utilisé dans chaque `<fptr>` pour indiquer que cette division correspond à une partie seulement du fichier lié et identifier exactement la partie concernée dans chacun d’eux. Par exemple, la première division (l'introduction par l'intervieweur) est liée à une partie du fichier de transcription en XML (FILE001) qui se trouve dans le fichier de transcription entre les deux balises d'attributs ID de valeurs "INTVWBG" et "INTVWND". Cette première division est également liée à deux fichiers audio différents. Dans ce cas, plutôt que de spécifier des valeurs d'attribut ID dans les fichiers liés, on indique les positions de début et de fin du contenu dans les fichiers par un simple code de temps sous forme HH:MM:SS. Ainsi, l'introduction de l'intervieweur se trouve dans les deux fichiers audio dans le segment commençant au temps 00:00:00 et finissant au temps 00:01:47.

## <span id="structlink">Liens structurels</span>

La section des liens structurels du format METS a la forme la plus simple parmi les principales sections de METS. En effet, cette section ne contient qu'un seul élément : `<smLink>` (bien que cet élément puisse être répété). La section des liens structurels de METS est destinée à permettre de décrire l'existence d'hyperliens entre éléments au sein de la carte de structure, le plus souvent des éléments `<div>`. Cette disposition est très utile, par exemple, si l'on souhaite utiliser METS pour archiver des sites Web, et conserver trace de la structure hypertextuelle des sites séparément des fichiers HTML des sites eux-mêmes.

A titre d'exemple, considérons le cas d'un document METS décrivant une page web qui contient une image liée à une autre page. L'élément `<structMap>` comprendra probablement des `<div>` pour chacune des deux pages comme dans l'exemple suivant :

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

Pour indiquer que le fichier image, décrit dans une `<div>` contenue dans la `<div>` de la première page, a un lien hypertexte vers un fichier HTML décrit dans la `<div>` de la deuxième page, on utilise un élément `<smLink>` au sein de la section `<structLink>` du document METS comme suit :

```xml 
<smLink xlink:from="IMG1" xlink:to="P2" xlink:title="Hyperlink from 
  JPEG Image on Page 1 to Page 2" xlink:show="new"
  xlink:actuate="onRequest" />
```

L'élément de lien `<smLink>` ci-dessus utilise une forme légèrement modifiée de la syntaxe XLink. Tous les attributs XLink sont utilisés, mais les attributs « to » et « from » sont déclarés de type IDREF, au lieu de NMTOKEN comme dans la spécification XLink d'origine. Cela permet d'indiquer l'existence de liens entre deux nœuds quelconques dans la carte de structure, et également d'utiliser des outils de traitement XML pour vérifier que les nœuds liés existent réellement.

## <span id="behavior">Comportement</span>

Une section de comportement peut être utilisée pour associer des comportements exécutables au contenu d'un objet METS. Une section de comportement contient un ou plusieurs éléments `<behavior>` dont chacun comprend un élément de définition d'interface `<interfaceDef>` qui donne une définition abstraite de l'ensemble de comportements représentés par une section de comportement. Un `<behavior>` comprend également un élément `<mechanism>` utilisé pour pointer vers un module de code exécutable qui implémente et exécute le comportement défini de façon abstraite par la définition d'interface.

Les comportements des objets numériques peuvent être implémentés sous la forme de liens vers des services web distribués, ainsi qu'on peut le voir dans l'exemple ci-dessous tiré du projet Mellon [Fedora](http://www.fedora.info/).

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

Voir aussi :

  - [The Fedora Technical Specification](http://www.autopenhosting.org/fedora/master-spec-12.20.02.pdf) \[Spécifications techniques de Fedora (version en pdf)\]

  - [Sample Digital Object](http://www.autopenhosting.org/fedora/obj-image-userinput-archdraw.xml)  \[Exemple d'objet numérique (codé en METS)\]

  - [Sample Behavior Definition Object](http://www.autopenhosting.org/fedora/bdef-image-userinput.xml) \[Exemple de définition de comportement (codé en METS)\]

  - [Sample Behavior Mechanism Object](http://www.autopenhosting.org/fedora/bmech-image-userinput-mrsid.xml) \[Exemple de mécanisme de comportement (codé en METS)\]

## Conclusion

Le format METS fournit un mécanisme souple pour l'encodage des métadonnées descriptives, administratives et de structure d'un objet numérique, ainsi que pour exprimer les liens complexes entre ses composantes et leurs métadonnées sous différentes formes. Il fournit donc un standard utile pour l'échange d'objets numériques entre différents entrepôts. Le format METS fournit également la capacité d'associer un objet numérique à des comportements ou des services. Le présent document d'introduction a présenté les principales caractéristiques du format, mais une lecture approfondie du schéma et de la documentation qui l'accompagne est nécessaire pour comprendre toute l'étendue de ses possibilités.

Ce document est la traduction de "METS: An Overview & Tutorial", publié le ?? ???? 2019 à l'adresse :
[http://www.loc.gov/standards/mets/METSOverview.v2.html](//www.loc.gov/standards/mets/METSOverview.v2.html).
La traduction a été réalisée par Sébastien Peyrard et Jean-Philippe Tramoni et révisée par Bertrand Caron ([Bibliothèque nationale de France](http://www.bnf.fr)).
