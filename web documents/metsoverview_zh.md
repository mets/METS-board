# METS: 概要及指南

## 引言

数字对象资源库的常规维护中必定要求维护数字对象的元数据。为了有效管理和使用数字对象而必需的元数据，与用来管理印刷品（或其它实物资料）的元数据不同，而且更广泛。尽管图书馆可能（只）记录了其馆藏某种书的描述型元数据，但该书并不会因为没有记录关于该书如何组织的结构型元数据而分拆成散页，也不会因为图书馆没有注明该书是使用Ryobi胶印机生产的而导致研究人员无法判断该书的价值。不过上述说法对此书的电子版本却不成立。（因为）组成电子文献的图像页和文本文件若离开了结构型元数据，则用处寥寥；研究人员若离开了描述数字化过程的技术元数据，也将无法确定数字化版本对原件的真实反映程度。出于内部管理的需要，资源数据库必须能够存取适当的技术元数据，以便定期更新或迁移数据，从而确保珍贵资源的长期有效。
  

MOA2 项目（The Making of America II project）提出了一种针对文本和图像为主的描述型元数据、管理型元数据和结构型元数据的编码格式，以此逐步展开上述问题的研究。而由美国数字图书馆联盟（Digital Library Federation）倡导的METS，则致力于在MOA2基础上提供一种元数据编码的XML文档格式，不但适于仓储内部数字对象的管理，而且适于这些对象在仓储之间（或仓储与用户之间）的交换。从METS文档的用途可见，它可以担当OAIS模型（Open Archival Information System Reference Model开放档案信息系统参考模型）中SIP（Submission Information Package提交信息包)、 AIP (Archival Information Package存档信息包) 或 DIP（Dissemination Information Package 分发信息包)的角色。

METS文档由七个主要节（section）组成：

1. METS 头（METS Header）—— METS 头包含了描述METS 文档自身的元数据，比如创建者、编辑者等。

2. 描述型元数据（Descriptive Metadata）—— 描述型元数据节，可以指向该METS文档外部的描述型元数据（例如，OPAC中的一条MARC记录，或在WWW服务器中运维的EAD finding aid），也可以在该METS文档内部嵌入描述型元数据，或二者兼有。描述型元数据节可以包含外部描述型元数据和内部描述型元数据的多个实例。

3. 管理型元数据（Administrative Metadata）—— 管理型元数据节提供的信息有如：文件是如何创建和保存的、关于知识产权、关于该数字对象原始资源的元数据、构成该数字对象的文件的起源信息（比如，文件的主体和派生关系，以及迁移和转换信息）。与描述型元数据同样，管理型元数据既可以位于METS文档外部，也可以被编码在文档内部。

4. 文件节（File Section）—— 数字对象电子版是由若干内容文件组成的，这些文件要全部列在文件节中。`<file>` 元素可以被`<fileGrp>`元素划分成组，以便按照对象版本加以细分。

5. 结构图（Structural Map）—— 结构图是METS文档的核心。它勾勒了数字对象的层次结构，并将该结构中元素与相应的内容文件和元数据关联起来。

6. 结构链接（Structural Links）—— METS的结构链接节，允许METS作者把结构图层次节点之间存在的超链接记录下来，特别适于用METS存档web站点。

7. 行为（Behavior）—— 行为节用于关联可执行代码与METS对象内容。行为节的每一个行为（behavior），有接口定义元素（interface definition element），也有机制元素（mechanism element）。前者明确了一组行为的抽象定义，并用特定的行为节（behavior sector）表示，后者则标识了在接口定义中已被抽象定义的若干行为所对应的可执行代码模块。

下面对上述各节及其关系作更详细的说明。

  
## <span id="MHead">METS 头</span>

METS头元素允许你精简地记录关于METS文档中METS对象本身的描述型元数据。这些元数据包括METS文档的创建日期、上次修改日期以及该METS文档的状态。你也可以记录一个或多个承担过与该METS文档有关的某种任务的机构（agent），注明他们扮演的角色，再为其活动附上小说明之类。最后，你还可以记录多种该METS文档的其他标识符，补充METS文档的主要标识符（记录在METS根元素的OBJID属性中）。下面是一个METS头的小例子，形如：

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
  

上例中的 `<metsHdr>` 元素包含两个属性——CREATEDATE 和 RECORDSTATUS，说明了该METS文档的创建日期时间以及记录处理的状态，还列出了曾为该METS记录工作过的两个独立机构（agent），一个负责创建该记录，一个负责存档原始资料。`<agent>` 元素的ROLE 和TYPE 属性都取自受控词表。ROLE的可选值包括 “ARCHIVIST”、“CREATOR”、“CUSTODIAN”、“DISSEMINATOR”、“EDITOR”、 “IPOWNER” 和“OTHER”。TYPE则可取值“INDIVIDUAL”、“ORGANIZATION” 或者“OTHER”。

  
## <span id="descMD">描述型元数据</span>

METS文档的描述型元数据节由一个或多个 `<dmdSec>` 元素（Descriptive Metadata Section）组成。每个 `<dmdSec>` 元素可以包含一个指向外部元数据的指针（`<mdRef>`元素），也可以包含内嵌元数据（嵌在`<mdWrap>`元素内），或二者兼备。

**外部描述型元数据（****mdRef****）：** mdRef 元素给出了用作检索外部元数据的 URI。 下面是一个元数据引用例子，它把一个特定的数字对象的元数据指向了finding aid ：

  
```xml 
<dmdSec ID="dmd001">
  <mdRef LOCTYPE="URN" MIMETYPE="application/xml" MDTYPE="EAD"
    LABEL="Berol Collection Finding Aid" xlink:href="urn:x-nyu:fales1735" />
</dmdSec>       
```

这个 `<dmdSec>` 的 `<mdRef>` 元素内有四个属性。LOCTYPE 属性说明元素内定位器的类型，其有效值包括 “URN”、“URL”、“PURL”、“HANDLE”、“DOI”、和“OTHER” 。MIMETYPE 属性指定外部描述型元数据的MIME 类型。 MDTYPE 指明被引用的元数据格式，其有效值包括 MARC、 MODS、EAD、VRA (VRA Core)、DC (Dublin Core)、NISOIMG (NISO Technical Metadata for Digital Still Images)、LC-AV (Library of Congress Audiovisual Metadata) 、TEIHDR (TEI Header)、DDI (Data Documentation Initiative)、FGDC (Federal Geographic Data Committee Metadata Standard \[FGDC-STD-001-1998\] ) 和OTHER。 LABEL 则为METS 文档浏览者提供该元数据的描述文字，比如显示该METS 文档的目录内容。

**内部描述型元数据（****mdWrap****）：** mdWrap 元素提供封装器给内嵌于METS 文档的元数据，而元数据是如下两种格式之一： 1.  XML编码元数据，它用XML编码表明本身所属于的命名空间不同于该METS文档的命名空间。2.  任何二进制或文本格式元数据，前提是这个元数据是Base64 编码的且被封装在mdWrap 元素内的`<binData>` 元素中。下例示范了mdWrap 元素的用法：

  
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

注意，所有 `<dmdSec>`
元素必须要有ID
属性。此属性为每个`<dmdSec>`
元素提供了唯一的内部名称，它可以应用于结构图中，把文档层次结构中指定的div
与指定的`<dmdSec>`
元素联系起来。这样，指定的描述型元数据节就可以关联数字对象的指定部分。

## <span id="admMD">管理型元数据</span>

`<amdSec>` 元素中的管理型元数据，既可以描述组成该数字对象的文件，也可以描述生成该对象的原始素材。METS 文档中规定了管理型元数据的四种主要格式： 1.  技术元数据 (关于文件的创建、格式和使用特征的信息。) 2. 知识产权元数据 (版权和许可信息。) 3.  来源元数据 (关于该数字对象之模拟来源的描述型元数据和管理型元数据。) 4. 数字起源元数据 (关于作品的原始数字化形态与作为数字对象的当前形态之间的关系信息，包括文件之间的来源/目标关系、主体/派生关系和迁移/转换关系。) 上述四种类型的管理型元数据分别对应于METS 文档 `<amdSec>` 的子元素`<techMD>`、 `<rightsMD>`、`<sourceMD>`、 和 `<digiprovMD>`，其中可以嵌入相应格式的元数据。这四种元素在任何METS 文档中均可以出现一次以上。

`<techMD>`、 `<rightsMD>`、 `<sourceMD>` 和 `<digiprovMD>` 元素与 `<dmdSec>` 共用同一种内容模型： 它们可以用 `<mdRef>` 元素指向外部管理型元数据，也可以用`<mdWrap>` 元素在METS 文档内部嵌入管理型元数据，或兼而有之。 一个METS文档可有以上元素的多个实例，但它们都必须有 ID 属性，以便被该METS文档的其它元素 (比如结构图中的div 或`<file>` 元素) 关联到对应的`<amdSec>` 子元素。下例中，我们用了技术元数据 `<techMD>` 元素来描述某文件的准备过程：

  
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

接着，可能某`<fileGrp>` 的某 `<file>` 元素，就可以把该管理型元数据标识为属于自己的——这是通过标识ADMID 属性做到的（指向这个 `<techMD>` 元素）：
  
```xml
<file ID="FILE001" ADMID="AMD001">
  <FLocat LOCTYPE="URL" xlink:href="http://dlib.nyu.edu/press/testimg.tif" />
</file>
```

## <span id="filegrp">文件节</span>

文件节 (`<fileSec>`) 包含一个或多个`<fileGrp>` 元素，把相关文件划分成组。 `<fileGrp>` 列出的所有文件组成了一个数字对象的电子版本。例如，可以分别设立若干`<fileGrp>` 元素，有缩略图组、主档案影像组、pdf版组、TEI 编码文本版组等等。
  

我们看看下面文件节的例子，选自一个口述历史的数字对象，它有三个版本：一个是TEI-编码的文件，一个是WAV 格式的主要音频文件，还有一个MP3格式的派生文件：

  
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

此例中， `<fileSec>` 包含三个 `<fileGrp>` 子元素，每个子元素对应数字对象的一个不同版本。 第一个是XML-编码的文件，第二个是WAV 格式的主要音频文件，第三个是MP3格式的派生文件。虽然这个简单例子看似并不必需 `<fileGrp>` 元素区别对象的不同版本，但当对象内含大量的扫描页文件时——其实不论何时只要对象包含很多很多文件——`<fileGrp>` 就会非常有用。这时，因为能够用`<fileGrp>` 元素区分 `<file>` 元素，想把文件归类到指定文档版本才易如反掌。

您可能注意到GROUPID 属性了，它有标识值且出现在两个音频 `<file>` 元素中，表明这两个文件虽然属于对象的不同版本，却包含了同样的基本信息(当数字对象包含了很多扫描页面图像时，你同样可以利用GROUPID指示数字对象中同一页面的多个图像文件)。

你应该也注意到了所有的 `<file>` 元素均有唯一的 ID 属性。此属性为该文件提供了唯一的内部名称，以便被文档的其它部分引用。你将可以在结构图节中看到此类引用的实际情形。

应该提到的是，`<file>` 元素拥有一个`<FContent>` 元素可能要比 `<FLocat>` 元素更好一些。`<FContent>` 元素将文件的实际内容嵌入到METS 文档内——此时，文件内容必须是XML 格式或Base64-编码二者其一。 诚然，当METS 文档用来为用户展示数字对象时，内嵌文件并不是非做不可，但当它用来在仓储间交换数字对象或者离线存档数字对象时，其特色就颇有价值了。

## <span id="structmap">结构图</span>

METS 文档的结构图定义了一种把数字对象呈现给用户的层次结构，并允许用户通过结构图导航。 `<structMap>` 元素利用一系列嵌套的`<div>` 元素体现了这种层次结构。 `<div>` 将其所属类型用属性信息标明，而把对应于该div 的内容，用多个METS 指针元素(`<mptr>`) 和文件指针元素(`<fptr>`) 指向。METS 指针指向 METS 文档，其中包含了该`<div>`的相关文件信息——这种机制适于编码大量资料(例如一种完整的期刊)，它可以保持集合内每个METS 文件的尺寸较小。 文件指针则指向当前METS 文档`<fileSec>`节中的文件(有时是指向文件组或某文件内的特定位置) ，当然被指向的文件对应于当前`<div>` 所示的层次结构部分。
  

下面所示是一个非常简单的结构图的例子：

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
  
如结构图所示，我们有一段口述历史 (由纽约前市长 Abraham Beame讲述)， 包括三个片段：采访人的开篇介绍，Beame 市长讲述的家族历史,，还有关于他如何在纽约加入教师联合会的讨论。 每一个片段/div 被链接到三个文件(来自我们前面文件组的示例)：一个XML 格式文件，一个主文件和一个派生音频文件。每个`<fptr>` 中都用到了`<area>` 子元素，指示这个 div 仅对应于被链接文件的一部分，并且详细指明了每个链接文件的准确片段。例如，第一个div (采访人介绍) 被链接到XML文件(FILE001)的一个片段，位于文件内 ID 属性值为“INTVWBG”和“INTVWND” 的两个标签（tag） 之间 。它还可以链接到两个不同的音频文件，此时，用简单的HH:MM:SS格式时间值标识被链材料的起点和终点，比在文件里用ID 属性标识会更好一些。这样，在两个音频文件中也可以找到采访人介绍——起始时间为00:00:00 延续到时间为00:01:47的片段。

## <span id="structlink">结构链接</span>

METS的结构链接节是几个主要的METS节中格式最简单的，只包含一个元素 `<smLink>` (尽管此元素允许重复)。 METS 的结构链接节允许你记录结构图内条目之间（通常是`<div>` 元素）存在的超链接。如果你希望用METS来存档web站点, 并希望维护这个超文本结构站点记录，而且与站点本身的HTML 文件分开的话，那么结构链接就是可用的工具。

设想一个web页METS 文档的例子，web页中包含一个图像，此图像超链到另一页。`<structMap>` 元素很可能如下所示，包含两页的`<div>`：

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

你若想说明第一页`<div>` 内的图像文件与第二页`<div>` 内的HTML文件存在超链接，可在METS 文档的`<structLink>` 节内使用一个 `<smLink>` 元素，如下所示：

```xml 
<smLink xlink:from="IMG1" xlink:to="P2" xlink:title="Hyperlink from 
  JPEG Image on Page 1 to Page 2" xlink:show="new"
  xlink:actuate="onRequest" />
```

上述`<smLink>` 链接元素使用的是改良后的XLink 语法格式，用到了全部 XLink 属性，但是“to” 和 “from” 属性声明成IDREF 类型，比原来XLink 规格说明书中的NMTOKEN 要好。 因为这样不但可以标明任何两个结构图节点之间存在的链接，而且可以利用XML 处理工具确认链接节点的存在。

## <span id="behavior">行为节</span>

行为节用于把可执行的行为与METS 对象的内容联系起来。 行为节包含一个或多个`<behavior>` 元素，每一个`<behavior>` 元素都有接口定义元素，而接口定义元素表示了一系列行为的抽象定义，这些定义由一个特定的行为节表示。 `<behavior>` 元素还有`<mechanism>` 元素，它指向一个可执行代码模块，运行在接口定义中定义的行为。

数字对象行为的实现可以通过链接到分布式web服务的形式，正如下面Mellon Fedora项目中的例子所示。

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

参见：

  - [Fedora 技术说明书（The Fedora Technical Specification）](http://www.autopenhosting.org/fedora/master-spec-12.20.02.pdf) (pdf)

  - [数字对象样例（Sample Digital Object）](http://www.autopenhosting.org/fedora/obj-image-userinput-archdraw.xml) (METS编码)

  - [行为定义样例（Sample Behavior Definition Object）](http://www.autopenhosting.org/fedora/bdef-image-userinput.xml) (METS编码)

  - [行为机制对象样例（Sample Behavior Mechanism Object）](http://www.autopenhosting.org/fedora/bmech-image-userinput-mrsid.xml) (METS编码)

## 结论

METS 模式提出了一种灵活的机制，用于编码数字对象的描述型、
管理型和结构型元数据，并表示出这些不同格式元数据之间存在的连结。因此，它为仓储间数字对象的交换提供了可用的标准。此外，METS
还能够将数字对象与行为或服务关联。上述讨论仅突出了METS模式的主要特色，而全面理解其功能则需要仔细研究METS模式及其文档。

