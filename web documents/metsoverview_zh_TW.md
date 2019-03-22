# METS: 概要及指南

## 引言

數位物件資源庫的常規維護中必定要求維護數位物件的元資料。爲了有效管理和使用數位物件而必需的元資料，與用來管理印刷品（或其他實物資料）的元資料不同，而且更廣泛。儘管圖書館可能（只）記錄了其館藏某種書的描述型元資料，但該書並不會因爲沒有記錄關於該書如何組織的結構型元資料而分拆成散頁，也不會因爲圖書館沒有注明該書是使用Ryobi膠印機生産的而導致研究人員無法判斷該書的價值。不過上述說法對此書的電子版本可不成立。（因爲）組成電子文獻的圖像頁和文字檔案若離開了結構型元資料，則用處寥寥；研究人員若離開了描述數位化過程的技術元資料，也將無法確定數位化版本對原件的真實反映程度。出於內部管理的需要，資源資料庫必須能夠存取適當的技術元資料，以便定期更新或遷移資料，從而確保珍貴資源的長期有效。
  
MOA2 專案（The Making of America II project）提出了一種針對文本和圖像爲主的描述型元資料、管理型元資料和結構型元資料的編碼格式，以此逐步展開上述問題的研究。而由美國數位圖書館聯盟（Digital Library Federation）倡導的METS，則致力於在MOA2基礎上提供一種元資料編碼的XML文檔格式，不但適於倉儲內部數位物件的管理，而且適於這些物件在倉儲之間（或倉儲與用戶之間）的交換。從METS文檔的用途可見，它可以擔當OAIS模型（Open Archival Information System Reference Model開放檔案資訊系統參考模型）中SIP（Submission Information Package提交資訊包)、 AIP (Archival Information Package存檔資訊包) 或 DIP（Dissemination Information Package 分發資訊包)的角色。
  
METS文檔由七個主要節（section）組成：

1. METS 頭（METS Header）—— METS 頭包含了描述METS 文檔自身的元資料，比如創建者、編輯者等。

2. 描述型元資料（Descriptive Metadata）—— 描述型元資料節，可以指向該METS文檔外部的描述型元資料（例如，OPAC中的一條MARC記錄，或在WWW伺服器中運維的EAD finding aid），也可以在該METS文檔內部嵌入描述型元資料，或二者兼有。描述型元資料節可以包含外部描述型元資料和內部描述型元資料的多個實例。

3.  管理型元資料（Administrative Metadata）—— 管理型元資料節提供的資訊有如：文件是如何創建和保存的、關於知識産權、關於該數位物件原始資源的元資料、構成該數位物件的文件的起源資訊（比如，文件的主體和派生關係，以及遷移和轉換資訊）。與描述型元資料同樣，管理型元資料既可以位於METS文檔外部，也可以被編碼在文檔內部。

4. 文件節（File Section）—— 數位物件電子版是由若干內容文件組成的，這些文件要全部列在文件節中。`<file>` 元素可以被`<fileGrp>`元素劃分成組，以便按照物件版本加以細分。

5. 結構圖（Structural Map）—— 結構圖是METS文檔的核心。它勾勒了數位物件的層次結構，並將該結構中元素與相應的內容文件和元資料關聯起來。

6. 結構鏈結（Structural Links）—— METS的結構鏈結節，允許METS作者把結構圖層次節點之間存在的超鏈結記錄下來，特別適於用METS存檔web站點。

7.  行爲（Behavior）—— 行爲節用于關聯可執行代碼與METS物件內容。行爲節的每一個行爲（behavior），有介面定義元素（interface definition element），也有機制元素（mechanism element）。前者明確了一組行爲的抽象定義，並用特定的行爲節（behavior sector）表示，後者則標識了在介面定義中已被抽象定義的若干行爲所對應的可執行代碼模組。

下面對上述各節及其關係作更詳細的說明。
  
## <span id="MHead">METS 頭</span>

METS頭元素允許你精簡地記錄關於METS文檔中METS物件本身的描述型元資料。這些元資料包括METS文檔的創建日期、上次修改日期以及該METS文檔的狀態。你也可以記錄一個或多個承擔過與該METS文檔有關的某種任務的機構（agent），注明他們扮演的角色，再爲其活動附上小說明之類。最後，你還可以記錄多種該METS文檔的其他識別字，補充METS文檔的主要識別字（記錄在METS根元素的OBJID屬性中）。下面是一個METS頭的小例子，形如：

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
  
上例中的 `<metsHdr>` 元素包含兩個屬性——CREATEDATE 和 RECORDSTATUS，說明了該METS文檔的創建日期時間以及記錄處理的狀態，還列出了曾爲該METS記錄工作過的兩個獨立機構（agent），一個負責創建該記錄，一個負責存檔原始資料。`<agent>` 元素的ROLE 和TYPE 屬性都取自受控詞表。ROLE的可選值包括 “ARCHIVIST”、“CREATOR”、“CUSTODIAN”、“DISSEMINATOR”、“EDITOR”、 “IPOWNER” 和“OTHER”。TYPE則可取值“INDIVIDUAL”、“ORGANIZATION” 或者“OTHER”。

## <span id="descMD">描述型元資料</span>

METS文檔的描述型元資料節由一個或多個 `<dmdSec>` 元素（Descriptive Metadata Section）組成。每個 `<dmdSec>` 元素可以包含一個指向外部元資料的指標（`<mdRef>`元素），也可以包含內嵌元資料（嵌在`<mdWrap>`元素內），或二者兼備。

**外部描述型元資料（****mdRef****）：** mdRef 元素給出了用作檢索外部元資料的 URI。 下面是一個元資料引用例子，它把一個特定的數位物件的元資料指向了finding aid ：

```xml 
<dmdSec ID="dmd001">
  <mdRef LOCTYPE="URN" MIMETYPE="application/xml" MDTYPE="EAD"
    LABEL="Berol Collection Finding Aid" xlink:href="urn:x-nyu:fales1735" />
</dmdSec>       
```

這個 `<dmdSec>` 的 `<mdRef>` 元素內有四個屬性。LOCTYPE 屬性說明元素內定位元器的類型，其有效值包括 “URN”、“URL”、“PURL”、“HANDLE”、“DOI”、和“OTHER” 。MIMETYPE 屬性指定外部描述型元資料的MIME 類型。 MDTYPE 指明被引用的元資料格式，其有效值包括 MARC、 MODS、EAD、VRA (VRA Core)、DC (Dublin Core)、NISOIMG (NISO Technical Metadata for Digital Still Images)、LC-AV (Library of Congress Audiovisual Metadata) 、TEIHDR (TEI Header)、DDI (Data Documentation Initiative)、FGDC (Federal Geographic Data Committee Metadata Standard \[FGDC-STD-001-1998\] ) 和OTHER。 LABEL 則爲METS 文檔瀏覽者提供該元資料的描述文字，比如顯示該METS 文檔的目錄內容。
  
**內部描述型元資料（****mdWrap****）：** mdWrap 元素提供封裝器給內嵌于METS 文檔的元資料，而元資料是如下兩種格式之一： 1.  XML編碼元資料，它用XML編碼表明本身所屬於的命名空間不同于該METS文檔的命名空間。2.  任何二進位或文本格式元資料，前提是這個元資料是Base64 編碼的且被封裝在mdWrap 元素內的`<binData>` 元素中。下例示範了mdWrap 元素的用法：

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

注意，所有 `<dmdSec>` 元素必須要有ID 屬性。此屬性爲每個`<dmdSec>` 元素提供了唯一的內部名稱，它可以應用於結構圖中，把文檔層次結構中指定的div 與指定的`<dmdSec>` 元素聯繫起來。這樣，指定的描述型元資料節就可以關聯數位物件的指定部分。
  
## <span id="admMD">管理型元資料</span>

`<amdSec>` 元素中的管理型元資料，既可以描述組成該數位物件的文件，也可以描述生成該物件的原始素材。METS 文檔中規定了管理型元資料的四種主要格式： 1.  技術元資料 (關於文件的創建、格式和使用特徵的資訊。) 2.  知識産權元資料 (版權和許可資訊。) 3. 來源元資料 (關於該數位物件之類比來源的描述型元資料和管理型元資料。) 4.  數位起源元資料 (關於作品的原始數位化形態與作爲數位物件的當前形態之間的關係資訊，包括文件之間的來源/目標關係、主體/派生關係和遷移/轉換關係。) 上述四種類型的管理型元資料分別對應于METS 文檔 `<amdSec>` 的子元素`<techMD>`、 `<rightsMD>`、`<sourceMD>`、 和 `<digiprovMD>`，其中可以嵌入相應格式的元資料。這四種元素在任何METS 文檔中均可以出現一次以上。

`<techMD>`、 `<rightsMD>`、 `<sourceMD>` 和 `<digiprovMD>` 元素與 `<dmdSec>` 共用同一種內容模型： 它們可以用 `<mdRef>` 元素指向外部管理型元資料，也可以用`<mdWrap>` 元素在METS 文檔內部嵌入管理型元資料，或兼而有之。 一個METS文檔可有以上元素的多個實例，但它們都必須有 ID 屬性，以便被該METS文檔的其他元素 (比如結構圖中的div 或`<file>` 元素) 關聯到對應的`<amdSec>` 子元素。下例中，我們用了技術元資料 `<techMD>` 元素來描述某文件的準備過程：

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

接著，可能某`<fileGrp>` 的某 `<file>` 元素，就可以把該管理型元資料標識爲屬於自己的——這是通過標識ADMID 屬性做到的（指向這個 `<techMD>` 元素）：
  

```xml
<file ID="FILE001" ADMID="AMD001">
  <FLocat LOCTYPE="URL" xlink:href="http://dlib.nyu.edu/press/testimg.tif" />
</file>
```

## <span id="filegrp">文件節</span>

文件節 (`<fileSec>`) 包含一個或多個`<fileGrp>` 元素，把相關文件劃分成組。
`<fileGrp>`
列出的所有文件組成了一個數位物件的電子版本。例如，可以分別設立若干`<fileGrp>`
元素，有縮略圖組、主檔案影像組、pdf版組、TEI 編碼文本版組等等。

我們看看下面文件節的例子，選自一個口述歷史的數位物件，它有三個版本：一個是TEI-編碼的文件，一個是WAV
格式的主要音頻文件，還有一個MP3格式的派生文件：

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

此例中， `<fileSec>` 包含三個 `<fileGrp>` 子元素，每個子元素對應數位物件的一個不同版本。 第一個是XML-編碼的文件，第二個是WAV 格式的主要音頻文件，第三個是MP3格式的派生文件。雖然這個簡單例子看似並不必需 `<fileGrp>` 元素區別物件的不同版本，但當物件內含大量的掃描頁文件時——其實不論何時只要物件包含很多很多文件——`<fileGrp>` 就會非常有用。這時，因爲能夠用`<fileGrp>` 元素區分 `<file>` 元素，想把文件歸類到指定文檔版本才易如反掌。

您可能注意到GROUPID 屬性了，它有標識值且出現在兩個音頻 `<file>` 元素中，表明這兩個文件雖然屬於物件的不同版本，卻包含了同樣的基本資訊(當數位物件包含了很多掃描頁面圖像時，你同樣可以利用GROUPID指示數位物件中同一頁面的多個圖像文件)。

你應該也注意到了所有的 `<file>` 元素均有唯一的 ID 屬性。此屬性爲該文件提供了唯一的內部名稱，以便被文檔的其他部分引用。你將可以在結構圖節中看到此類引用的實際情形。

應該提到的是，`<file>` 元素擁有一個`<FContent>` 元素可能要比 `<FLocat>` 元素更好一些。`<FContent>` 元素將文件的實際內容嵌入到METS 文檔內——此時，文件內容必須是XML 格式或Base64-編碼二者其一。 誠然，當METS 文檔用來爲用戶展示數位物件時，內嵌文件並不是非做不可，但當它用來在倉儲間交換數位物件或者離線存檔數位物件時，其特色就頗有價值了。

## <span id="structmap">結構圖</span>

METS 文檔的結構圖定義了一種把數位物件呈現給用戶的層次結構，並允許用戶通過結構圖導航。 `<structMap>` 元素利用一系列嵌套的`<div>` 元素體現了這種層次結構。 `<div>` 將其所屬類型用屬性資訊標明，而把對應於該div 的內容，用多個METS 指標元素(`<mptr>`) 和文件指標元素(`<fptr>`) 指向。METS 指標指向 METS 文檔，其中包含了該`<div>`的相關文件資訊——這種機制適於編碼大量資料(例如一種完整的期刊)，它可以保持集合內每個METS 文件的尺寸較小。 文件指標則指向當前METS 文檔`<fileSec>`節中的文件(有時是指向文件組或某文件內的特定位置) ，當然被指向的文件對應於當前`<div>` 所示的層次結構部分。

下面所示是一個非常簡單的結構圖的例子：

  
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

如結構圖所示，我們有一段口述歷史 (由紐約前市長 Abraham Beame講述)， 包括三個片段：採訪人的開篇介紹，Beame 市長講述的家族歷史,，還有關於他如何在紐約加入教師聯合會的討論。 每一個片段/div 被鏈結到三個文件(來自我們前面文件組的示例)：一個XML 格式文件，一個主文件和一個派生音頻文件。每個`<fptr>` 中都用到了`<area>` 子元素，指示這個 div 僅對應於被鏈結文件的一部分，並且詳細指明了每個鏈結文件的準確片段。例如，第一個div (採訪人介紹) 被鏈結到XML文件(FILE001)的一個片段，位於文件內 ID 屬性值爲“INTVWBG”和“INTVWND” 的兩個標簽（tag） 之間 。它還可以鏈結到兩個不同的音頻文件，此時，用簡單的HH:MM:SS格式時間值標識被鏈材料的起點和終點，比在文件裏用ID 屬性標識會更好一些。這樣，在兩個音頻文件中也可以找到採訪人介紹——起始時間爲00:00:00 延續到時間爲00:01:47的片段。
  

## <span id="structlink">結構鏈結</span>

METS的結構鏈結節是幾個主要的METS節中格式最簡單的，只包含一個元素 `<smLink>` (儘管此元素允許重復)。 METS 的結構鏈結節允許你記錄結構圖內條目之間（通常是`<div>` 元素）存在的超鏈結。如果你希望用METS來存檔web站點, 並希望維護這個超文本結構站點記錄，而且與站點本身的HTML 文件分開的話，那麽結構鏈結就是可用的工具。

設想一個web頁METS 文檔的例子，web頁中包含一個圖像，此圖像超鏈到另一頁。`<structMap>` 元素很可能如下所示，包含兩頁的`<div>`：

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
  
你若想說明第一頁`<div>` 內的圖像文件與第二頁`<div>` 內的HTML文件存在超鏈結，可在METS 文檔的`<structLink>` 節內使用一個 `<smLink>` 元素，如下所示：
  
```xml 
<smLink xlink:from="IMG1" xlink:to="P2" xlink:title="Hyperlink from 
  JPEG Image on Page 1 to Page 2" xlink:show="new"
  xlink:actuate="onRequest" />
```

上述`<smLink>` 鏈結元素使用的是改良後的XLink 語法格式，用到了全部 XLink 屬性，但是“to” 和 “from” 屬性聲明成IDREF 類型，比原來XLink 規格說明書中的NMTOKEN 要好。 因爲這樣不但可以標明任何兩個結構圖節點之間存在的鏈結，而且可以利用XML 處理工具確認鏈結節點的存在。

## <span id="behavior">行爲節</span>

行爲節用於把可執行的行爲與METS 物件的內容聯繫起來。 行爲節包含一個或多個`<behavior>` 元素，每一個`<behavior>` 元素都有介面定義元素，而介面定義元素表示了一系列行爲的抽象定義，這些定義由一個特定的行爲節表示。 `<behavior>` 元素還有`<mechanism>` 元素，它指向一個可執行代碼模組，運行在介面定義中定義的行爲。

數位物件行爲的實現可以通過鏈結到分散式web服務的形式，正如下面Mellon Fedora專案中的例子所示。

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
  
參見：

  - [Fedora 技術說明書](http://www.autopenhosting.org/fedora/master-spec-12.20.02.pdf) （The Fedora Technical Specification）(pdf)

 - [數位對像樣例](http://www.autopenhosting.org/fedora/obj-image-userinput-archdraw.xml)（Sample Digital Object）
 (METS編碼)
 
 - [行爲定義樣例](http://www.autopenhosting.org/fedora/bdef-image-userinput.xml)（Sample Behavior Definition Object）
 (METS編碼)
 
 - [行爲機制對像樣例](http://www.autopenhosting.org/fedora/bmech-image-userinput-mrsid.xml)（Sample Behavior Mechanism Object） (METS編碼)

## 結論

METS 模式提出了一種靈活的機制，用於編碼數位物件的描述型、 管理型和結構型元資料，並表示出這些不同格式元資料之間存在的連結。因此，它爲倉儲間數位物件的交換提供了可用的標準。此外，METS 還能夠將數位物件與行爲或服務關聯。上述討論僅突出了METS模式的主要特色，而全面理解其功能則需要仔細研究METS模式及其文檔。
