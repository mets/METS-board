# METS: Introdução & Tutorial

## Introdução

A manutenção de uma biblioteca de objectos digitais exige a manutenção
dos metadados sobre esses objectos. Os metadados necessários para
utilizar e gerir com sucesso objectos digitais são diferentes e mais
vastos que os metadados utilizados para gerir colecções de obras
impressas e outros materiais físicos. Embora uma biblioteca possa manter
metadados descritivos sobre um livro da sua colecção, o livro não se
dissolverá numa série de páginas soltas caso a biblioteca não registar
metadados estruturais sobre a organização do livro, nem os
investigadores serão incapazes de avaliar o valor do livro se a
biblioteca não anotar que o livro foi produzido numa prensa "Ryoby". O
mesmo não pode ser dito para uma versão digital do mesmo livro. Sem
metadados estruturais, os ficheiros com imagens ou texto que compõem a
obra digital serão de pouca utilidade, e sem metadados técnicos sobre o
processo de digitalização, os investigadores poderão ter dúvidas sobre a
exactidão da reflexão do original que a versão digital oferece. Por
questões de gestão interna, a biblioteca deve ter acesso a metadados
técnicos apropriados para lhe permitir refrescar e migrar os dados,
garantindo a durabilidade de recursos valiosos.

O projecto [Making of America II
(MOA2)](http://sunsite.berkeley.edu/MOA2/) tentou abordar parcialmente
estas questões providenciando um formato de codificação para metadados
administrativos e estruturais para trabalhos textuais e baseados em
imagens. METS, uma iniciativa da Digital Liberará Federativos, tenta
construir sobre o trabalho do MOA2 e providencia um formato em XML para
codificar metadados necessários tanto para a gestão de objectos de
bibliotecas digitais num repositório como para a troca desses objectos
entre repositórios (ou entre repositórios e os seus utilizadores).
Dependendo da sua utilização, um documento METS pode ser utilizado no
papel de um Pacote de Informação de Submissão (Submission Information
Package - SIP), um Pacote de Informação de Arquivo (Archival Information
Package - AIP) ou um Pacote de Informação de Disseminação (Dissemination
Information Package - DIP) no contexto do [Open Archival Information
System (OAIS) Reference
Model](http://ssdoo.gsfc.nasa.gov/nost/isoas/ref_model.html).

Um documento METS consiste em sete secções principais:

1\. [**Cabeçalho METS**](#MHead) - O cabeçalho METS contém metadados
descrevendo o documento METS em si, incluindo informação como o criador,
editor, etc...

2\. [**Metadados Descritivos**](#descMD) - A secção de metadados
descritivos pode apontar para metadados descritivos externos ao
documento METS (e.g., um registo MARC num OPAC ou um registo EAD mantido
num servidor Web), ou conter metadados descritivos embebidos, ou ambos.
Múltiplas instancias de metadados descritivos, tanto internas como
externas, podem ser incluídos na secção de metadados descritivos.

3\. [**Metadados Administrativos**](#admMD) - A secção de metadados
administrativos oferece informação sobre como os ficheiros foram criados
e armazenados, direitos de propriedade intelectual, metadados sobre o
objecto original a partir do qual o objecto digital foi derivado, e
informação sobre a proveniência dos ficheiros que compõem o objecto
digital (i.e., relações de ficheiros originais/derivados, e informação
de migração/transformação). Tal como os metadados descritivos, os
metadados administrativos podem ser tanto externos ao documento METS, ou
codificados internamente.

4\. [**Secção de Ficheiros**](#filegrp) - A secção de ficheiros lista
todos os ficheiros que contêm as versões electrónicas do objecto
digital. Elementos `<file>` podem ser agrupados em elementos
`<fileGrp>`, para permitir a subdivisão de ficheiros por versão do
objecto.

5\. [**Mapa Estrutural**](#structmap) - O Mapa Estrutural é o coração do
documento METS. Ele esboça uma estrutura hierárquica para o objecto da
biblioteca digital, e liga os elementos dessa estrutura a ficheiros com
conteúdos e metadados referentes a cada elemento.

6\. [**Ligações Estruturais**](#structlink) - A secção de Ligações
Estruturais do METS permite aos criadores METS registar a existência de
hiperligações entre nós na hierarquia esboçada no Mapa Estrutural. Esta
secção tem um valor particular na utilização do METS para arquivar
sites.

7\. [**Comportamento**](#behavior) - Uma secção de comportamento pode
ser usada para associar comportamentos executáveis com o conteúdo no
objecto METS. Cada comportamento numa secção de comportamento tem um
elemento de definição de interface que representa uma definição
abstracta do conjunto de comportamentos representado por uma secção de
comportamento particular. Cada comportamento também tem um elemento de
mecanismo que identifica um módulo de código executável que implementa e
executa os comportamentos definidos de forma abstracta perla definição
de interface.

Segue-se uma explicação mais detalhada de cada secção e das suas
inter-relações.

## <span id="MHead"></span>Cabeçalho METS

O elemento Cabeçalho METS permite-lhe registar metadados descritivos
minimalistas sobre o objecto METS em si dentro do documento METS. Estes
metadados incluem a data de criação para o documento METS, a data da sua
última alteração e um estado para o documento METS. Pode também registar
nomes de um ou mais agentes que tenham desempenhado algum papel para o
documento METS, especificar o papel que eles desempenharam, e
acrescentar uma pequena nota sobre a sua actividade. Finalmente, pode
registar uma variedade de identificadores alternativos para o documento
METS por acréscimo ao identificador principal para o documento METS
registado no atributo OBJID no elemento raiz METS. Um pequeno exemplo de
um Cabeçalho METS pode ter o seguinte aspecto:

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

Este exemplo contém dois atributos no elemento `<metsHdr>`, CREATEDATE e
RECORDSTATUS, os quais são usados para indicar a data e a hora em que o
registo METS foi criado, e indicar o estado do processamento do registo.
Dois agentes que trabalharam neste registo estão listados, a pessoa
responsável pela criação do registo e um arquivista responsável pelo
material original. Ambos os atributos ROLE e TYPE no elemento `<agent>`
utilizam vocabulários controlados. Valores permitidos para ROLE incluem
"ARCHIVIST" (arquivista), "CREATOR" (criador), "CUSTODIAN" (responsável,
guarda, conservador), "DISSEMINATOR" (divulgador), "EDITOR" (editor),
"IPOWNER" (detentor de endereço IP), e "OTHER" (outro). Valores
permitidos para o atributo TYPE são "INDIVIDUAL" (individúo)
"ORGANIZATION" (organização) ou "OTHER" (outro)

## <span id="descMD"></span>Metadados Descritivos

A secção de Metadados Descritivos de um documento METS consistem em um
ou mais elementos `<dmdSec>` (Secção de Metadados Descritivos). Cada
elemento `<dmdSec>` pode conter um ponteiro para metadados externos (um
elemento `<mdRef>`), metadados embebidos (dentro de um elemento
`<mdWrap>`), ou ambos.

Metadados Descritivos Externos (mdRef): um elemento mdRef oferece um URI
que pode ser utilizado para obter os metadados externos. Por exemplo, a
seguinte referência a metadados aponta para o registo de um objecto
digital em particular:

```xml 
<dmdSec ID="dmd001">
  <mdRef LOCTYPE="URN" MIMETYPE="application/xml" MDTYPE="EAD"
    LABEL="Berol Collection Finding Aid" xlink:href="urn:x-nyu:fales1735" />
</dmdSec>       
```

O elemento `<mdRef>` desta `<dmdSec>` contém quatro atributos. O
atributo LOCTYPE especifica o tipo de identificador de localização
contido no corpo do elemento; valores válidos para LOCTYPE incluem
'URN,' 'URL,' 'PURL,' 'HANDLE,' 'DOI,' e 'OTHER' (outro). O atributo
MIMETYPE permite-lhe especificar o tipo MIME para os metadados
descritivos externos, e o MDTYPE permite-lhe indicar qual a forma de
metadados está a ser referenciada. Valores válidos para o elemento
MDTYPE incluem MARC, MODS, EAD, VRA (VRA Core), DC (Dublin Core),
NISOIMG (NISO Technical Metadata for Digital Still Images), LC-AV
(Library of Congress Audiovisual Metadata), TEIHDR (TEI Header), DDI
(Data Documentation Initiative), FGDC (Federal Geographic Data Committee
Metadata Standard \[FGDC-STD-001-1998\] ), e OTHER (outro). LABEL
oferece um mecanismo para descrever estes metadados para aqueles que
visualizam um documento METS, numa visualização do tipo 'Índice de
Conteúdos' do documento METS, por exemplo.

Metadados Descritivos Internos (mdWrap): Um elemento mdWrap oferece um
contentor para metadados embebidos dentro de um documento METS. Estes
metadados podem tomar uma de duas formas: 1. metadados codificados em
XML, em que a codificação XML se identifica como pertencendo a um outro
espaço de nomes que não o espaço de nomes dos documentos METS, ou 2.
qualquer forma binária ou textual, DESDE QUE esses metadados sejam
codificados em Base64 e contidos num elemento `<binData>` dentro do
elemento mdWrap. Os exemplos seguintes demonstram a utilização do
elemento mdWrap:

 

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

Note-se que todos os elementos `<dmdSec>` têm que possuir um atributo
ID. Este atributo oferece um nome interno único para cada elemento
`<dmdSec>` que pode ser usado no mapa estrutural para ligar uma divisão
em particular da hierarquia do documento a um elemento `<dmdSec>` em
particular. Isto permite que secções específicas de metadados
descritivos sejam ligadas a partes específicas do objecto digital.

## <span id="admMD"></span>Metadados Administrativos

Elementos `<amdSec>` contêm os metadados administrativos relativos aos
ficheiros que constituem um objecto digital, bem como os relativos ao
material fonte original utilizado para criar o objecto. Existem quatro
formas principais de metadados administrativos disponíveis para
utilização num documento METS: 1. Metadados Técnicos (informação
relativa à criação, formato e características de utilização), 2.
Metadados sobre Direitos e Propriedade Intelectual (copyright e
informação de licenciamento), 3. Metadados sobre a Fonte (metadados
descritivos e administrativos relativos à fonte analógica da qual o
objecto digital é derivado), e 4. Metadados sobre a Origem Digital
(informação respeitante às relações de origem/destino entre ficheiros,
incluindo relações de original/derivado entre ficheiros e informação
respeitante a migrações/transformações aplicadas a ficheiros entre a
digitalização original de um artefacto e a sua incarnação como um
objecto de uma biblioteca digital). Cada um destes quatro tipos
diferentes de metadados administrativos tem um subelemento único dentro
da parte `<amdSec>` de um documento METS no qual essa forma de metadados
pode ser embebida: `<techMD>`, `<rightsMD>`, `<sourceMD>`, e
`<digiprovMD>`. Cada um destes quatro elementos pode ocorrer mais que
uma vez em qualquer documento METS.

Os elementos `<techMD>`, `<rightsMD>`, `<sourceMD>` e `<digiprovMD>`
aplicam o mesmo modelo de conteúdo que o `<dmdSec>`: eles podem conter
um elemento `<mdRef>` para apontar para metadados administrativos
externos, um elemento `<mdWrap>` para usar quando se embebede metadados
administrativos num documento METS, ou ambos. Múltiplas instancias
destes elementos podem ocorrer num documento METS, e todas eles tem que
conter um atributo ID para que outros elementos dentro do documento METS
(tais como divisões dentro do mapa estrutural ou elementos `<file>`)
possam ser ligados aos subelementos `<amdSec>` que os descrevem.
Pode-se, por exemplo, ter um elementos `<techMD>` que inclui metadados
técnicos sobre a preparação de um ficheiro:

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

Um elemento `<file>` dentro de um `<fileGrp>` pode então identificar
estes metadados administrativos como pertencendo ao ficheiro que
identifica através da utilização de um atributo ADMID para apontar para
este elemento `<techMD>`:

```xml
<file ID="FILE001" ADMID="AMD001">
  <FLocat LOCTYPE="URL" xlink:href="http://dlib.nyu.edu/press/testimg.tif" />
</file>
```

## <span id="filegrp"></span>Secção de Ficheiros

A secção de ficheiros (`<fileSec>`) contém um ou mais elementos
`<fileGrp>` usados para agrupar ficheiros relacionados. Um `<fileGrp>`
lista todos os ficheiros que compõem uma única versão electrónica do
objecto da biblioteca digital. Por exemplo, podem existir elementos
`<fileGrp>` separados para as thumbnails, as imagens originais de
arquivo, as versões pdf, as versões de texto codificadas em TEI, etc.

Considere-se o seguinte exemplo de uma secção de ficheiros de uma
objecto de uma biblioteca digital para uma história falada que tem três
versões diferentes: uma transcrição codificada em TEI, um ficheiro de
som original em formato WAV, e um ficheiro áudio derivado em formato
MP3:

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

Neste caso, o `<fileSec>` contém três elementos subordinados
`<fileGrp>`, um para cada versão do objecto. O primeiro é um ficheiro de
transcrição codificado em XML, o segundo é o ficheiro de som original em
formato WAV, e o terceiro é um ficheiro áudio derivado em formato MP3.
Embora neste simples exemplo não aparente haver uma necessidade real
para a utilização de elementos `<fileGrp>` para distinguir as diferentes
versões do objecto, o `<fileGrp>` torna-se muito mais útil para objectos
que consistem num elevado número de imagens digitalizadas, ou mesmo em
qualquer caso onde uma única versão do objecto consista num elevado
número de ficheiros. Nesses casos, ter a possibilidade de separar os
elementos `<file>` em elementos `<fileGrp>` facilita a identificação dos
ficheiros que pertencem a uma versão em particular do documento.

Note-se a presença de atributos GROUPID com valores idênticos nos dois
elementos `<file>` que contêm os ficheiros áudio; estes indicam que os
dois ficheiros, embora pertencendo a versões diferentes do objecto,
contêm basicamente a mesma informação (pode utilizar o GROUPID para o
mesmo fim de indicar ficheiros de imagem de páginas equivalentes num
objecto digital que contenha muitas imagens de páginas digitalizadas).

Note-se também que todos os elementos `<file>` têm um atributo ID único.
Este atributo proporciona um nome interno único para o ficheiro que pode
ser referenciado por outras partes do documento. Irá ver este tipo de
referenciação em acção quando abordar-mos a Secção do Mapa Estrutural.

Deve ser mencionado que os elementos `<file>` podem possuir um elementos
`<FContent>` em vez de um elementos `<FLocat>`. Os elementos
`<FContent>` são utilizados para embeber o conteúdo do ficheiro em si no
documento METS; caso isto seja feito, os conteúdos do ficheiro devem ou
estar em formato XML ou ser codificados em Base64. Embora embeber
ficheiros não seja geralmente utilizado na preparação de um documento
METS para exibir objectos de uma biblioteca digital aos utilizadores,
pode ser ser uma funcionalidade valiosa para troca objectos de
bibliotecas digitais entre repositórios, ou para versões de arquivo de
objectos de bibliotecas digitais para armazenamento.

## <span id="structmap"></span>Mapa Estrutural

A secção do mapa estrutural de um documento METS define uma estrutura
hierárquica que pode ser apresentada a utilizadores do objecto da
biblioteca digital para lhes permitir navegar nele. O elemento
`<structMap>` codifica esta hierarquia como série de elementos `<div>`
encaixados. Cada `<div>` contém informação em atributos que especifica
que tipo de divisão é, e também pode conter múltiplos apontadores METS
(`<mptr>`) e elementos apontadores de ficheiros (`<fptr>`) para
identificar o conteúdo correspondente a esse `<div>`. Apontadores METS
especificam outros documentos METS como contendo a informação relevante
para o `<div>` que os contém. Isto pode ser útil quando se codifica
grandes colecções de material (e.g., todos os números de um jornal
científico) para manter o tamanho de cada ficheiro METS relativamente
pequeno. Apontadores para ficheiros especificam ficheiros (ou, em alguns
casos, tanto grupos de ficheiros ou localizações específicas dentro de
um ficheiro) dentro da secção `<fileSec>` do documento METS actual que
corresponde à porção na hierarquia representada pelo `<div>` actual.

Em seguida encontra-se um exemplo de um mapa estrutural extremamente
simples:

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

Este mapa estrutural mostra que temos ums história falada (com o Mayor
Abraham Beame da cidade de Nova York) que inclui três subsecções: uma
introdução de abertura pelo entrevistador, um pouco de história da
família do Mayor Beame, e uma discussão de com ele se envolveu com o
sindicato dos professores de Nova York. Cada uma destas
subsecções/divisões estão ligadas a três ficheiros (obtidos do
exemplo anterior do agrupamento de ficheiros): uma transcrição em XML, e
os ficheiros áudio original e derivado. Um elemento `<area>` subordinado
é usado em cada `<fptr>` para indicar que esta divisão corresponde com
apenas uma porção do ficheiro ligado, e para identificar a porção exacta
de cada ficheiro ligado. Por exemplo, a primeira divisão (a introdução
do entrevistador) está ligada a uma porção do ficheiro de transcrição em
XML (FILE001) que se encontra entre duas etiquetas no ficheiro da
transcrição com o atributo ID com os valores "INTVWBG" e "INTVWND."
Também está ligado aos dois ficheiros áudio; nestes casos, em vez de
especificar os valores dos atributos ID dentro dos ficheiros ligados, os
pontos de início e fim do material ligado dentro dos ficheiros é
indicado através de um simples valor de tempo codificado na forma
HH:MM:SS. Assim, a introdução do entrevistador pode ser encontrada em
ambos os ficheiros áudio no segmento que começa no tempo 00:00:00 no
ficheiro e estende-se até ao tempo 00:01:47.

## <span id="structlink"></span>Ligações Estruturais

A secção de ligações estruturais do formato METS é a mais simples em
termos de forma de todas as principais secções METS, contendo apenas um
único elemento, `<smLink>` (embora esse elemento possa ser repetido). A
secção de ligações estruturais do METS visa permitir registar a
existência de hiperligações entre itens dentro do mapa estrutural,
geralmente elementos `<div>`. Esta é uma funcionalidade útil caso se
pretenda utilizar o METS para arquivar sites, e se pretenda manter um
registo da estrutura do hipertexto dos sites separadamente dos ficheiros
HTML do site em si.

Como exemplo, considere o caso de um documento METS para uma página da
Internet contendo uma imagem que está hiperligada a outra página. O
elemento `<structMap>` iria provavelmente conter `<divs>` como os
seguintes para as duas páginas:

 

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

Caso pretenda indicar que o ficheiro de imagem no `<div>` contido no
`<div>` da primeira página está hiperligado ao ficheiro HTML no `<div>`
da segunda página, então teria um elemento `<smLink>` dentro da secção
`<structLink>` do documento METS, da seguinte forma:

```xml 
<smLink xlink:from="IMG1" xlink:to="P2" xlink:title="Hyperlink from 
  JPEG Image on Page 1 to Page 2" xlink:show="new"
  xlink:actuate="onRequest" />
```

O elemento de ligação `<smLink>` acima usa um forma ligeiramente
modificada do sintaxe XLink; todos os atributos XLink são usados, mas os
atributos "to" e "from" attributes são declarados como sendo do tipo
IDREF em vez de NMTOKEN como na especificação original do XLink. Isto
permite-lhe indicar a existência de ligações entre quaisquer dois nós do
mapa estrutural, e também usar ferramentas de processamento de XML para
confirmar que os nós ligador realmente existem.

## <span id="behavior"></span>Secção de Comportamento

A secção de comportamento pode ser utilizada para associar
comportamentos executáveis com conteúdos do objecto METS. Uma secção de
comportamento contém um ou mais elementos `<behavior>`, cada um dos
quais tem um elemento de definição de interface que representa uma
definição abstracta do conjunto de comportamentos representado por uma
secção de comportamento em particular. Um `<behavior>` também tem um
elemento `<mechanism>` que é usado para apontar para um módulo de código
executável que implementa e executa o comportamento definido de forma
abstracta pela definição da interface.

Os comportamentos de objectos digitais podem ser implementados como
ligações a web services como no exemplo seguinte originário do projecto
Fedora (apoiado pela Fundação
Mellon):

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

Ver também:

  - [Especificação Técnica do Fedora
    (pdf)](http://www.fedora.info/documents/master-spec-12.20.02.pdf)
  - [Exemplar de um Objecto Digital (codificado em
    METS)](http://www.fedora.info/documents/obj-image-userinput-archdraw.xml)
  - [Exemplar de um Objecto de Definição de Comportamento (codificado em
    METS)](http://www.fedora.info/documents/bdef-image-userinput.xml)
  - [Exemplar de um Objecto de Mecanismo de Comportamento (codificado em
    METS)](http://www.fedora.info/documents/bmech-image-userinput-mrsid.xml)

## Conclusão

O esquema METS oferece um mecanismo flexível para codificar metadados
descritivos, administrativos e estruturais para um objecto de uma
biblioteca digital, e para exprimir as ligações complexas entre estas
várias formas de metadados. Assim o METS oferece uma norma útil para a
troca de objectos digitais entre repositórios. Adicionalmente, o METS
oferece a possibilidade de associar um objecto digital com
comportamentos ou serviços. A discussão anterior descreve as principais
funcionalidades do esquema, mas uma examinação mais detalhada do esquema
e da sua documentação é necessária para compreender todo o alcance das
sua capacidades.

Este documento corresponde à tradução do documento "METS: An Overview &
Tutorial", publicado em 18 de Julho de 2003 em
http://www.loc.gov/standards/mets/METSOverview.v2.html. A tradução é da
responsabilidade de Nuno Freire (INESC-ID), com revisão de José Borbinha
(Biblioteca Nacional da Portugal).
