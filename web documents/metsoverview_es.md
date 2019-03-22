# METS: introducción y tutorial

## Introducción

La gestión de una biblioteca de objetos digitales requiere la gestión de
metadatos sobre esos objetos. Los metadatos necesarios para gestionar y
usar con éxito objetos digitales son más cmplejos que los que se emplean
para gestionar colecciones de documentos impresos y materiales con
soporte físico. Una biblioteca puede registrar metadatos descriptivos
sobre un libro de su colección, pero el libro nunca se disolverá en una
serie de páginas independientes, desconectadas, si la biblioteca no
registra los metadatos estructurales relativos a la organización del
libro; tampoco los usuarios se verán incapacitados para valorar la obra
si la biblioteca no registra que el libro se produjo usando una prensa
offset de un tipo determinado. Sin embargo, esto mismo no podría
afirmarse para la versión digital de ese mismo libro. Sin metadatos
estructurales, las imágenes y los archivos de texto que conforman el
objeto digital tienen poca utilidad, y sin los metadatos técnicos
relativos al proceso de digitalización los usuarios no pueden evaluar en
qué medida la obra digital es un fiel reflejo del original impreso. Para
la gestión interna, la biblioteca debe conocer los metadatos técnicos
para poder refrescar y migrar regularmente los contenidos y asegurar la
preservación de estos valiosos recursos.

El proyecto [Making of America II](http://sunsite.berkeley.edu/MOA2/)
(MOA2) trató de afrontar estos problemas y propuso un formato de
codificación de metadatos descriptivos, administrativos y estructurales
para obras textuales y basadas en imágenes. METS, una iniciativa de la
[Digital Library Federation](http://www.diglib.org/), se desarrolló a
partir del trabajo de MOA2, y ofrece un formato basado en XML para
codificar los metadatos necesarios para la gestión de objetos digitales
y para su intercambio entre repositorios (o entre repositorios y sus
usuarios). Dependiendo de cómo se aplique, un documento METS podría
usarse como un Submission Information Package (SIP), Archival
Information Package (AIP), o Dissemination Information Package (DIP)
dentro del modelo de referencia [Open Archival Information System
(OAIS).](http://ssdoo.gsfc.nasa.gov/nost/isoas/ref_model.html)

Un documento METS consta de siete secciones:

1.  [**Cabecera METS**](#MHead) - contiene metadatos que describen el
    propio documento METS, e incluye datos como su creador, editor, etc.

2.  [**Metadatos Descriptivos**](#descMD) - Esta sección puede: a)
    apuntar a metadatos descriptivos externos al documento METS (por
    ejemplo, un registro MARC en un OPAC o un documento EAD disponible
    en un servidor web); b) contener internamente los metadatos
    descriptivos, o c) combinar ambas aproximaciones. En la sección
    Metadatos Descriptivos se pueden incluir múltiples metadatos
    descriptivos, tanto internos como externos.

3.  [**Metadatos Administrativos**](#admMD) - ofrece información sobre
    cómo se crearon y almacenaron los archivos que conforman el objeto
    digital, derechos de propiedad intelectual, metadatos sobre el
    objeto original a partir del cual se obtuvo la representación
    digital, e información sobre la procedencia de los archivos que
    conforman el objeto digital (es decir, relaciones entre copias
    maestras y derivadas, migraciones y transformaciones). Al igual que
    sucede con los metadatos descriptivos, los metadatos administrativos
    pueden ser externos o codificarse dentro del propio documento METS.

4.  [**Sección Archivo**](#filegrp) - lista todos los archivos con
    contenidos que forman parte del objeto digital. Los archivos pueden
    agruparse en elementos \<fileGrp\>, uno para cada una de las
    distintas versiones del objeto.

5.  [**Mapa Estructural**](#structmap) - es la parte principal de un
    documento METS. Recoge la estructura jerárquica del objeto digital,
    y enlaza sus secciones con los archivos de contenido y los metadatos
    correspondientes a cada una de ellas.

6.  [**Enlaces Estructurales**](#structlink) - permite registrar la
    existencia de hiperenlaces entre las secciones del mapa estructural.
    Tiene gran valor cuando se usa METS para archivar sitios web.

7.  [**Comportamientos**](#behavior) - se puede usar para vincular
    comportamientos ejecutables con los contenidos del documento METS.
    Cada comportamiento tiene una definición de interfaz y un
    "mecanismo" que identifica un módulo de código ejecutable que
    implementa y ejecuta el comportamiento definido de forma abstracta
    por la interfaz.

Los siguientes apartados recogen una explicación más detallada de cada
una de estas secciones y sus interrelaciones.

## <span id="MHead">Cabecera METS</span>

El elemento Cabecera METS (METS Header) permite registrar - dentro del
propio documento METS - unos mínimos metadatos descriptivos sobre el
propio documento METS. Estos metadatos incluyen la fecha de creación del
documento METS, fecha de última modificación y estado. También se puede
registrar el nombre de uno o más agentes que han desempeñado alguna
función en el ciclo de vida del documento METS, especificar dicha
función y añadir una breve nota sobre estas actividades. Finalmente, se
puede registrar una variedad de identificadores alternativos para el
documento METS adicionales al identificador principal que se registrará
en el atributo OBJID del elemento raíz METS.  El siguiente fragmento
recoge un ejemplo de Cabecera METS:  

``` 
      <metsHdr CREATEDATE="2003-07-04T15:00:00" RECORDSTATUS="Complete">
    <agent ROLE="CREATOR" TYPE="INDIVIDUAL">
      <name>Jerome McDonough</name>
    </agent>
    <agent ROLE="ARCHIVIST" TYPE="INDIVIDUAL">
      <name>Ann Butler</name>
    </agent>
      </metsHdr>

            
```

En este ejemplo el elemento \<metsHdr\> contiene dos atributos:
CREATEDATE y RECORDSTATUS. Indican respectivamente la fecha y hora en
que se creó el documento METS y su estado. Se listan dos agentes que han
trabajado en este documento: la persona responsable de su creación y un
archivero responsable del material original. Los atributos ROLE y TYPE
del elemento \<agent\> toman sus valores de vocabularios controlados. 
Los valores permitidos para el atributo ROLE son: "ARCHIVIST,"
"CREATOR," "CUSTODIAN," "DISSEMINATOR," "EDITOR," "IPOWNER" y "OTHER." 
Los valores permitidos para el atributo TYPE son: "INDIVIDUAL,"
"ORGANIZATION" y "OTHER."

## <span id="descMD">Metadatos Descriptivos</span>

La sección Metadatos Descriptivos consiste en uno o más elementos
\<dmdSec\> (Descriptive Metadata Section). Cada elemento \<dmdSec\>
puede: a) contener un puntero a metadatos externos (elemento \<mdRef\>);
b) contener metadatos internamente (dentro de un elemento \<mdWrap\>), o
c) combinar estas dos opciones.

**Metadatos descriptivos externos (mdRef):** un elemento mdRef recoge
una URI en la que se pueden recuperar metadatos externos. Por ejemplo,
la siguiente referencia apunta a un instrumento de descripción externo
para un objeto digital:

``` 
      <dmdSec ID="dmd001">
          <mdRef LOCTYPE="URN" MIMETYPE="application/xml" MDTYPE="EAD" 
          LABEL="Berol Collection Finding Aid">urn:x-nyu:fales1735</mdRef>
      </dmdSec>
            
```

El elemento \<mdRef\> de este \<dmdSec\> contiene cuatro atributos. El
atributo LOCTYPE especifica el tipo de localizador que se usa; los
valores aceptados para LOCTYPE son: 'URN,' 'URL,' 'PURL,' 'HANDLE,'
'DOI,' y 'OTHER.' El atributo MIMETYPE especifica el tipo MIME de los
metadatos descriptivos externos, y MDTYPE a qué tipo de metadatos se
hace referencia. Los valores aceptados para MDTYPE incluyen MARC, MODS,
EAD, VRA (VRA Core), DC (Dublin Core), NISOIMG (NISO Technical Metadata
for Digital Still Images), LC-AV (Library of Congress Audiovisual
Metadata) , TEIHDR (TEI Header), DDI (Data Documentation Initiative),
FGDC (Federal Geographic Data Committee Metadata Standard
\[FGDC-STD-001-1998\] ), y OTHER. LABEL ofrece un mecanismo para
describir estos metadatos para aquellas personas que vean el documento
METS.

**Metadatos descriptivos internos (mdWrap):** el elemento mdWrap
contiene los metadatos dentro del propio documento METS. Estos metadatos
podrán ser: 1. metadatos codificados en XML, en cuyo caso se indicará
que pertenecen a un espacio de nombres distinto de METS, o 2. metadatos
en cualquier otro formato binario o textual (no XML), siempre que los
metadatos se codifiquen en Base64 y se escriban dentro de un elemento
\<binData\> contenido dentro del elemento mdWrap. Los siguientes
ejemplos muestran el uso del elemento mdWrap:

``` 
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

``` 
      <dmdSec ID="dmd003">
    <mdWrap MIMETYPE="application/marc" MDTYPE="MARC" LABEL="OPAC Record">
      <binData>MDI0ODdjam0gIDIyMDA1ODkgYSA0NU0wMDAxMDA...(etc.)
      </binData>
    </mdWrap>
      </dmdSec>
            
```

Debemos señalar que todos los elementos \<dmdSec\> deben contar con un
atributo ID. Este atributo asigna un identificador interno, único en el
documento, a cada elemento \<dmdSec\>. Este identificador podrá usarse
en el mapa estructural para enlazar una división particular de la
jerarquía del documento con un elemento \<dmdSec\> específico. Esto
permite enlazar metadatos descriptivos con secciones específicas del
objeto digital.

## <span id="admMD">Metadatos Administrativos</span>

Los elementos \<amdSec\> contienen los metadatos administrativos
correspondientes a los archivos que conforman el objeto digital, y
también los del material original a partir del cual se creó la
representación digital. En los documentos METS hay cuatro tipos de
metadatos administrativos: 1. Metadatos técnicos (información relativa a
la creación del archivo, su formato y características de uso), 2.
Metadatos sobre derechos de propiedad intelectual (copyright e
información sobre licencias), 3. Metadatos sobre el origen (metadatos
descriptivos y administrativos sobre el documento origen a partir del
cual se ha generado el objeto digital), y 4. Metadatos sobre la
procedencia digital (información sobre la relación entre el documento
original y su representación digital, incluyendo la relación entre
copias maestras y derivadas, migraciones y transformaciones realizadas
sobre los archivos desde su digitalización inicial).  Cada uno de estos
cuatro tipos de metadatos administrativos tienen un elemento propio
dentro de la sección \<amdSec\>: \<techMD\>, \<rightsMD\>, \<sourceMD\>,
y \<digiprovMD\>. Todos pueden repetirse.

Los elementos \<techMD\>, \<rightsMD\>, \<sourceMD\> y \<digiprovMD\>
tienen el mismo modelo de contenido que \<dmdSec\>: pueden contener un
elemento \<mdRef\> para apuntar a metadatos administrativos externos, un
elemento \<mdWrap\> para incorporar metadatos administrativos dentro del
propio documento METS, o combinar ambas opciones. Un documento METS
puede incorporar múltiples instancias de estos elementos. y todos ellos
deben contar con un atributo ID de forma que otros elementos del
documento METS (como las divisiones del mapa estructural o los elementos
\<file\>) puedan hacerles referencia. Por ejemplo, podríamos tener un
elemento \<techMD\> con los metadatos técnicos para un archivo:

``` 
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

Y un elemento \<file\> dentro de un elemento \<fileGrp\> en el que se
apuntase a esos metadatos administrativos mediante un atributo ADMID:

``` 
      <file ID="FILE001" ADMID="AMD001">
    <FLocat LOCTYPE="URL">http://dlib.nyu.edu/press/testimg.tif</FLocat>
      </file>
            
```

## <span id="filegrp">Sección Archivo</span>

La sección archivo (\<fileSec\>) contiene uno o más elementos
\<fileGrp\>. Estos agrupan archivos relacionados entre sí. Un
\<fileGrp\> reúne todos los archivos que conforman una misma versión
electrónica del objeto digital. Por ejemplo, puede haber elementos
\<fileGrp\> para las miniaturas, las copias maestras (alta resolución)
de las imágenes, la versión en pdf , la versión codificadas en TEI, etc.

El siguiente ejemplo muestra una sección Archivo para un registro sonoro
del que hay tres versiones: una transcripción codificada en TEI, una
copia maestra audio en formato WAV y una versión audio derivada de la
anterior en formato MP3:

``` 
      <fileSec>
    <fileGrp ID="VERS1">
      <file ID="FILE001" MIMETYPE="application/xml" SIZE="257537" CREATED="2001-06-10">
        <FLocat LOCTYPE="URL">http://dlib.nyu.edu/tamwag/beame.xml</FLocat>
      </file>
    </fileGrp>
    <fileGrp ID="VERS2">
      <file ID="FILE002" MIMETYPE="audio/wav" SIZE="64232836"
        CREATED="2001-05-17" GROUPID="AUDIO1">
        <FLocat LOCTYPE="URL">http://dlib.nyu.edu/tamwag/beame.wav</FLocat>
      </file>
    </fileGrp>
    <fileGrp ID="VERS3" VERSDATE="2001-05-18">
      <file ID="FILE003" MIMETYPE="audio/mpeg" SIZE="8238866"
        CREATED="2001-05-18" GROUPID="AUDIO1">
        <FLocat LOCTYPE="URL">http://dlib.nyu.edu/tamwag/beame.mp3</FLocat>
      </file>
    </fileGrp>
      </fileSec>
            
```

En este caso, \<fileSec\> contiene tres elementos \<fileGrp\>, uno para
cada versión del objeto. El primero es una transcripción codificada en
XML, el segundo es una versión audio en formato WAV, y el tercero una
versión audio en formato MP3. Aunque en este ejemplo puede no parecer
necesario utilizar elementos \<fileGrp\> para las distintas versiones
del objeto, \<fileGrp\> sería mucho más útil si el objeto consistiese en
un gran número de imágenes escaneadas, o si cada versión del objeto
constase de un mayor número de archivos. En estos casos, ser capaz de
agrupar los elementos \<file\> en distintos \<fileGrp\> facilita la
identificación de los archivos que pertenecen a cada versión.

En el ejemplo anterior se puede apreciar la presencia de atributos
GROUPID con idénticos valores en los elementos \<file\> correspondientes
a los archivos de audio; esto indica que esos dos archivos recogen la
misma información, aunque forman parte de distintas versiones (también
se puede usar GROUPID para indicar equivalencias entre archivos de
imagen en objetos digitales que contengan varias páginas escaneadas).

Podemos ver igualmente que todos los elementos \<file\> tienen un único
atributo ID. Este atributo da un identificador único a cada archivo,
para poder hacerles referencia desde otras secciones del documento.
Estas referencias se describen en la sección correspondiente al Mapa
Estructural.

Debemos mencionar que los elementos \<file\> pueden incluir un elemento
\<FContent\> en lugar de un elemento \<FLocat\>. Los elementos
\<FContent\> se usan para incorporar los contenidos dentro del propio
documento METS; de hacerse así, los contenidos del archivo pueden ser
datos en formato XML o datos codificados en Base64. La inclusión de
archivos empotrados no es algo que se haga frecuentemente en documentos
METS creados para mostrar objetos digitales a los usuarios, pero puede
ser interesante para intercambiar objetos digitales entre repositorios o
para archivar objetos digitales (almacenamiento off-site).

## <span id="structmap">Mapa Estructural</span>

La sección Mapa Estructural de un documento METS define una estructura
jerárquica que puede presentarse a los usuarios para navegar a través
del objeto digital. El elemento \<structMap\> establece esta jerarquía
como una serie de elementos \<div\> anidados. Cada \<div\> cuenta con
atributos que especifican de qué tipo de división se trata; también
puede contener múltiples punteros METS (\<mptr\>) y punteros a archivos
(\<fptr\>) para identificar los contenidos correspondientes a esa
sección. Los punteros METS apuntan a documentos METS aparte que
contienen la información sobre los archivos relevantes para la sección
\<div\>. Son útiles cuando se codifican grandes colecciones de
materiales (por ejemplo, una revista completa) y se quiere mantener el
tamaño de cada documento METS relativamente pequeño. Los punteros a
archivos indican qué archivos (o en ciertos casos, qué grupos de
archivos o partes de un archivo) previamente declarados en la sección
\<fileSec\> del documento METS se corresponden con la sección
representada por el elemento \<div\>.

A continuación se muestra un ejemplo de mapa estructural:

``` 
      <structMap TYPE="logical">
    <div ID="div1" LABEL="Oral History: Mayor Abraham Beame"
      TYPE="oral history">
      <div ID="div1.1" LABEL="Interviewer Introduction"
        ORDER="1">
    <fptr FILEID="FILE001">
      <area FILEID="FILE001" BEGIN="INTVWBG" END="INTVWND"
        BETYPE="IDREF" />
    </fptr>
    <fptr FILEID="FILE002">
      <area FILEID="FILE002" BEGIN="00:00:00" END="00:01:47"
        BETYPE="TIME" />
    </fptr>
    <fptr FILEID="FILE003">
      <area FILEID="FILE003" BEGIN="00:00:00" END="00:01:47"
        BETYPE="TIME" />
    </fptr>
      </div>
    <div ID="div1.2" LABEL="Family History" ORDER="2">
    <fptr FILEID="FILE001">
      <area FILEID="FILE001" BEGIN="FHBG" END="FHND"
        BETYPE="IDREF" />
    </fptr>
    <fptr FILEID="FILE002">
      <area FILEID="FILE002" BEGIN="00:01:48"END="00:06:17"
        BETYPE="TIME" />
    </fptr>
    <fptr FILEID="FILE003">
      <area FILEID="FILE003" BEGIN="00:01:48" END="00:06:17"
        BETYPE="TIME" />
    </fptr>
      </div>
    <div ID="div1.3" LABEL="Introduction to Teachers' Union"
      ORDER="3">
    <fptr FILEID="FILE001">
      <area FILEID="FILE001" BEGIN="TUBG" END="TUND"
        BETYPE="IDREF" />
    </fptr>
    <fptr FILEID="FILE002">
      <area FILEID="FILE002" BEGIN="00:06:18" END="00:10:03"
        BETYPE="TIME" />
    </fptr>
    <fptr FILEID="FILE003">
      <area FILEID="FILE003" BEGIN="00:06:18" END="00:10:03"
        BETYPE="TIME" />
    </fptr>
      </div>
      </div>
      </structMap> 
            
```

Este mapa estructural se corresponde con un registro sonoro (una
entrevista al Alcalde Abraham Beame de la ciudad de Nueva York) e
incluye tres subsecciones: una introducción por parte del entrevistador,
la historia familiar por parte del Alcalde Beame, y una discusión de
cómo llegó a participar en el sindicato de maestros de Nueva York. Cada
una de estas subsecciones o divisiones está enlazada con tres archivos
(los mismos que usamos en el ejemplo anterior): una transcripción XML,
un archivo audio maestro y uno correspondiente a una versión derivada.
El elemento hijo \<area\> se usa en cada \<fptr\> para indicar que la
división se corresponde únicamente con una parte del archivo al que se
hace referencia, y con él se identifica la parte exacta del archivo. Por
ejemplo, la primera división (la introducción por parte del
entrevistador) está enlazada a un fragmento del archivo con la
transcripción XML (FILE001) que se encuentra entre las dos etiquetas del
archivo cuyos atributos ID recogen los valores "INTVWBG" y "INTVWND".
También está enlazado a los dos archivos de audio; en esos casos, en
lugar de especificar valores del atributo ID para acotar el fragmento,
su inicio y fin se indica en forma de tiempo HH:MM:SS. Así, la
introducción del entrevistador se puede encontrar en los archivos de
audio en los fragmentos que comienzan en el instante 00:00:00 y que
tienen una duración de 00:01:47.

## <span id="structlink">Enlaces Estructurales</span>

La sección Enlaces Estructurales es la más sencilla de todas las
secciones METS, y contiene un único elemento \<smLink\> (que puede
repetirse).  La sección tiene como finalidad registrar la presencia de
hiperenlaces entre las distintas partes del mapa estructural, codficadas
mediante elementos \<div\>.  Es útil si se quiere usar METS para
archivar sitios web y mantener un registro de su estructura hipertextual
a parte de la que se establecen mediante los hiperenlaces de las propias
páginas HTML.

Como ejemplo, veamos un documento METS para una página web que contiene
una imagen que sirve de origen de un enlace a otra página. El elemento
\<structMap\> contendría probablemente \<divs\> como los siguientes para
las dos páginas:

``` 
    <div ID="P1" TYPE="page" LABEL="Page 1">
      <fptr FILEID="HTMLF1"/>
    <div ID="IMG1" TYPE="image" LABEL="Image Hyperlink to
      Page 2">
    <fptr FILEID="JPGF1"/>
    </div>

    <div ID="P2" TYPE="page" LABEL="Page 2">
      <fptr FILEID="HTMLF2"/>
    </div>
            
```

Si se quisiese indicar que el archivo de imagen está enlazado al archivo
HTML de la segunda página \<div\>, tendríamos un elemento \<smLink\>
dentro de la sección \<structLink\> como la siguiente:

``` 
      <smLink from="IMG1" to="P2" xlink:title="Hyperlink from 
      JPEG Image on Page 1 to Page 2" xlink:show="new"
      xlink:actuate="onRequest" />
            
```

El elemento \<smLink\> anterior usa la sintaxis XLink ligeramente
modificada; todos los atributos XLink se utilizan, pero los atributos
"to" y "from" se declaran de tipo IDREF en lugar de NMTOKEN, como se
hace en la especificación original XLink.  Esto permite indicar la
presencia de enlaces entre cualquier par de divisiones del mapa
estructural, y también usar herramientas de procesamiento XML para
confirmar que las dos divisiones existen realmente.

## <span id="behavior">Sección Comportamiento</span>

Una sección Comportamiento (behavior) puede usarse para asociar
comportamientos ejecutables al contenido de un documento METS. Una
sección Comportamiento contiene uno o más elementos \<behavior\>, y
cada uno de ellos tiene un elemento de definición de interfaz. Un
\<behavior\> también tiene un elemento \<mechanism\> que apunta a un
módulo de código ejecutable que implementa el comportamiento definido
de forma abstracta por la definición de la interfaz.

Los comportamientos de objetos digitales pueden implementarse como
enlaces a servicios web distribuidos, como en el siguiente ejemplo del
proyecto Mellon
[Fedora](http://www.fedora.info/).

``` 
         <METS:behavior ID="DISS1.1" STRUCTID="S1.1" BTYPE="uva-bdef:stdImage"
      CREATED="2002-05-25T08:32:00" LABEL="UVA Std Image Disseminator"
      GROUPID="DISS1" ADMID="AUDREC1">
           <METS:interfaceDef LABEL="UVA Standard Image Behavior Definition"
        LOCTYPE="URN" xlink:href="uva-bdef:stdImage"/>
           <METS:mechanism LABEL="A NEW AND IMPROVED Image Mechanism"
        LOCTYPE="URN" xlink:href="uva-bmech:BETTER-imageMech"/>
         </METS:behavior>
            
```

Véase además:

  - [Fedora: especificación
    técnica](http://www.fedora.info/documents/master-spec-12.20.02.pdf)
    (pdf)

  - [Objeto digital
    (ejemplo)](http://www.fedora.info/documents/obj-image-userinput-archdraw.xml)
    (codificado en METS)

  - [Objeto de definición de comportamiento
    (ejemplo)](http://www.fedora.info/documents/bdef-image-userinput.xml)
    (codificado en METS)

  - [Objeto de mecanismo de comportamiento
    (ejemplo)](http://www.fedora.info/documents/bmech-image-userinput-mrsid.xml)
    (codificado en METS)

## Conclusión

El esquema METS ofrece un medio flexible para codificar metadatos
descriptivos, administrativos y estructurales para un objeto digital, y
expresar las complejas relaciones entre estos tipos de metadatos. Ofrece
un estándar útil para el intercambio de objetos digitales entre
repositorios. Además, METS permite asociar objetos digitales con
comportamientos o servicios. Los párrafos anteriores destacan las
principales características del esquema, pero se recomienda un examen
más detallado del esquema y de su documentación para comprender todas
sus posibilidades.

  

Traducido por el profesor [Ricardo Eito
Brun](/cdn-cgi/l/email-protection#9be9fef2eff4dbf9f2f9b5eef8a8f6b5fee8),
Ciencias de Bibliotecologia e Information de la Universidad Carlos III
de Madrid, España.
