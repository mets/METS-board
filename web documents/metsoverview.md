# METS: An Overview & Tutorial

## Introduction

Maintaining a library of digital objects of necessity requires
maintaining metadata about those objects. The metadata necessary for
successful management and use of digital objects is both more extensive
than and different from the metadata used for managing collections of
printed works and other physical materials. While a library may record
descriptive metadata regarding a book in its collection, the book will
not dissolve into a series of unconnected pages if the library fails to
record structural metadata regarding the book's organization, nor will
scholars be unable to evaluate the book's worth if the library fails to
note that the book was produced using a Ryobi offset press. The same
cannot be said for a digital version of the same book. Without
structural metadata, the page image or text files comprising the digital
work are of little use, and without technical metadata regarding the
digitization process, scholars may be unsure of how accurate a
reflection of the original the digital version provides. For internal
management purposes, a library must have access to appropriate technical
metadata in order to periodically refresh and migrate the data, ensuring
the durability of valuable resources.

The [Making of America II](http://sunsite.berkeley.edu/MOA2/) project
(MOA2) attempted to address these issues in part by providing an
encoding format for descriptive, administrative, and structural metadata
for textual and image-based works. METS, a [Digital Library
Federation](http://www.diglib.org/) initiative, attempts to build upon
the work of MOA2 and provide an XML document format for encoding
metadata necessary for both management of digital library objects within
a repository and exchange of such objects between repositories (or
between repositories and their users). Depending on its use, a METS
document could be used in the role of Submission Information Package
(SIP), Archival Information Package (AIP), or Dissemination Information
Package (DIP) within the [Open Archival Information System (OAIS)
Reference Model.](http://nssdc.gsfc.nasa.gov/nost/isoas/ref_model.html)

A METS document consists of seven major sections:

1.  [**METS Header**](#MHead) - The METS Header contains metadata
    describing the METS document itself, including such information as
    creator, editor, etc.

2.  [**Descriptive Metadata**](#descMD) - The descriptive metadata
    section may point to descriptive metadata external to the METS
    document (e.g., a MARC record in an OPAC or an EAD finding aid
    maintained on a WWW server), or contain internally embedded
    descriptive metadata, or both. Multiple instances of both external
    and internal descriptive metadata may be included in the descriptive
    metadata section.

3.  [**Administrative Metadata**](#admMD) - The administrative metadata
    section provides information regarding how the files were created
    and stored, intellectual property rights, metadata regarding the
    original source object from which the digital library object
    derives, and information regarding the provenance of the files
    comprising the digital library object (i.e., master/derivative file
    relationships, and migration/transformation information). As with
    descriptive metadata, administrative metadata may be either external
    to the METS document, or encoded internally.

4.  [**File Section**](#filegrp) - The file section lists all files
    containing content which comprise the electronic versions of the
    digital object. \<file\> elements may be grouped within \<fileGrp\>
    elements, to provide for subdividing the files by object version.

5.  [**Structural Map**](#structmap) - The structural map is the heart
    of a METS document. It outlines a hierarchical structure for the
    digital library object, and links the elements of that structure to
    content files and metadata that pertain to each element.

6.  [**Structural Links**](#structlink) - The Structural Links section
    of METS allows METS creators to record the existence of hyperlinks
    between nodes in the hierarchy outlined in the Structural Map. This
    is of particular value in using METS to archive Websites.

7.  [**Behavior**](#behavior) - A behavior section can be used to
    associate executable behaviors with content in the METS object. Each
    behavior within a behavior section has an interface definition
    element that represents an abstract definition of the set of
    behaviors represented by a particular behavior section. Each
    behavior also has a mechanism element which identifies a module of
    executable code that implements and runs the behaviors defined
    abstractly by the interface definition.

A more detailed explanation of each section and their inter-relations
follows.

## <span id="MHead">METS Header</span>

The METS Header element allows you to record minimal descriptive
metadata about the METS object itself within the METS document. This
metadata includes the date of creation for the METS document, the date
of its last modification, and a status for the METS document. You may
also record the names of one or more agents who have played some role
with respect to the METS document, specify the role they have played,
and add a small note regarding their activity. Finally, you may record a
variety of alternative identifiers for the METS document to supplement
the primary identifier for the METS document recorded in the OBJID
attribute on the METS root element. A small example of a METS Header
might look like the following:  

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

This example contains two attributes on the \<metsHdr\> element,
CREATEDATE and RECORDSTATUS, which are used to indicate the date and
time the METS record was created, and indicate the status of the
record's processing. Two individual agents are listed who have worked on
this METS record, the person responsible for creating the record and an
archivist responsible for the original material. Both the ROLE and TYPE
attributes on the \<agent\> element employ controlled vocabularies.
Allowable values for ROLE include "ARCHIVIST," "CREATOR," "CUSTODIAN,"
"DISSEMINATOR," "EDITOR," "IPOWNER" and "OTHER." Allowable values for
the TYPE attribute are "INDIVIDUAL," "ORGANIZATION" or "OTHER."

## <span id="descMD">Descriptive Metadata</span>

The descriptive metadata section of a METS document consists of one or
more \<dmdSec\> (Descriptive Metadata Section) elements. Each \<dmdSec\>
element may contain a pointer to external metadata (an \<mdRef\>
element), internally embedded metadata (within an \<mdWrap\> element),
or both.

**External Descriptive Metadata (mdRef):** an mdRef element provides a
URI which may be used in retrieving the external metadata. For example,
the following metadata reference points to the finding aid for a
particular digital library object:

```xml 
      <dmdSec ID="dmd001">
          <mdRef LOCTYPE="URN" MIMETYPE="application/xml" MDTYPE="EAD" 
          LABEL="Berol Collection Finding Aid">urn:x-nyu:fales1735</mdRef>
      </dmdSec>
            
```

The \<mdRef\> element of this \<dmdSec\> contains four attributes. The
LOCTYPE attribute specifies the type of locator contained in body of the
element; valid values for LOCTYPE include 'URN,' 'URL,' 'PURL,'
'HANDLE,' 'DOI,' and 'OTHER.' The MIMETYPE attribute allows you to
specify the MIME type for the external descriptive metadata, and the
MDTYPE allows you to indicate what form of metadata is being referenced.
Valid values for the MDTYPE element include MARC, MODS, EAD, VRA (VRA
Core), DC (Dublin Core), NISOIMG (NISO Technical Metadata for Digital
Still Images), LC-AV (Library of Congress Audiovisual Metadata) , TEIHDR
(TEI Header), DDI (Data Documentation Initiative), FGDC (Federal
Geographic Data Committee Metadata Standard \[FGDC-STD-001-1998\] ), and
OTHER. LABEL provides a mechanism for describing this metadata to those
viewing a METS document, in a 'Table of Contents' display of the METS
document, for example.

**Internal Descriptive Metadata (mdWrap):** An mdWrap element provides a
wrapper around metadata embedded within a METS document. Such metadata
can be in one of two forms: 1. XML-encoded metadata, with the
XML-encoding identifying itself as belonging to a namespace other than
the METS document namespace, or 2. any arbitrary binary or textual form,
PROVIDED that the metadata is Base64 encoded and wrapped in a
\<binData\> element within the mdWrap element. The following examples
demonstrate the use of the mdWrap element:

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
      <binData>MDI0ODdjam0gIDIyMDA1ODkgYSA0NU0wMDAxMDA...(etc.)
      </binData>
    </mdWrap>
      </dmdSec>
            
```

Note that all \<dmdSec\> elements must possess an ID attribute. This
attribute provides a unique, internal name for each \<dmdSec\> element
which can be used in the structural map to link a particular division of
the document hierarchy to a particular \<dmdSec\> element. This allows
specific sections of descriptive metadata to be linked to specific parts
of the digital object.

## <span id="admMD">Administrative Metadata</span>

\<amdSec\> elements contain the administrative metadata pertaining to
the files comprising a digital library object, as well as that
pertaining to the original source material used to create the object.
There are four main forms of administrative metadata provided for in a
METS document: 1. Technical Metadata (information regarding files'
creation, format, and use characteristics), 2. Intellectual Property
Rights Metadata (copyright and license information), 3. Source Metadata
(descriptive and administrative metadata regarding the analog source
from which a digital library object derives), and 4. Digital Provenance
Metadata (information regarding source/destination relationships between
files, including master/derivative relationships between files and
information regarding migrations/transformations employed on files
between original digitization of an artifact and its current incarnation
as a digital library object). Each of these four different types of
administrative metadata has a unique subelement within the \<amdSec\>
portion of a METS document in which that form of metadata can be
embedded: \<techMD\>, \<rightsMD\>, \<sourceMD\>, and \<digiprovMD\>.
Each of these four elements may occur more than once in any METS
document.

The \<techMD\>, \<rightsMD\>, \<sourceMD\> and \<digiprovMD\> elements
employ the same content model as \<dmdSec\>: they may contain an
\<mdRef\> element to point to external administrative metadata, an
\<mdWrap\> element to use when embedding administrative metadata within
a METS document, or both. Multiple instances of these elements may occur
within a METS document, and all of them must carry an ID attribute so
that other elements within the METS document (such as divisions within
the structural map or \<file\> elements) may be linked to the \<amdSec\>
subelements which describe them. One might, for example, have an
\<techMD\> element which includes technical metadata regarding a file's
preparation:

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

A \<file\> element within a \<fileGrp\> might then identify this
administrative metadata as pertaining to the file it identifies by using
an ADMID attribute to point to this \<techMD\> element:

```xml
      <file ID="FILE001" ADMID="AMD001">
    <FLocat LOCTYPE="URL">http://dlib.nyu.edu/press/testimg.tif</FLocat>
      </file>
            
```

## <span id="filegrp">File Section</span>

The file section (\<fileSec\>) contains one or more \<fileGrp\> elements
used to group together related files. A \<fileGrp\> lists all of the
files which comprise a single electronic version of the digital library
object. For example, there might be separate \<fileGrp\> elements for
the thumbnails, the master archival images, the pdf versions, the TEI
encoded text versions, etc.

Consider the following example of a file section from a digital library
object for an oral history which has three different versions: a
TEI-encoded transcript, a master audio file in WAV format, and a
derivative audio file in MP3 format:

```xml 
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

In this case, the \<fileSec\> contains three subsidiary \<fileGrp\>
elements, one for each different version of the object. The first is an
XML-encoded transcription file, the second is a master audio file in WAV
format, and the third is a derivative audio file in MP3 format. While
such a basic example does not really seem to need the \<fileGrp\>
elements to distinguish the different versions of the object,
\<fileGrp\> becomes much more useful for objects consisting of large
numbers of scanned page images, or indeed any case where a single
version of the object consists of a large number of files. In those
cases, being able to separate \<file\> elements into \<fileGrp\>s makes
identifying the files belonging to a particular version of the document
a simple task.

You may note the presence of the GROUPID attributes with identical
values on the two audio \<file\> elements; these indicate that the two
files, while belonging to different versions of the object, contain the
same basic information (you can use the GROUPID for the same purpose to
indicate equivalent page image files in digital library objects
containing many scanned page images).

You should also note that all of the \<file\> elements have a unique ID
attribute. This attribute provides a unique, internal name for this file
which can be referenced by other portions of the document. You’ll see
this type of referencing in action when we look at the Structural Map
Section.

It should be mentioned that \<file\> elements may possess an
\<FContent\> element rather than an \<FLocat\> element. \<FContent\>
elements are used to embed the actual contents of the file within the
METS document; if this is done, the file contents must either be in XML
format or be Base64-encoded. While embedding files is not something one
would typically do when preparing a METS document for use in displaying
a digital library objects to users, it can be a valuable feature for
exchanging digital library objects between repositories, or for
archiving versions of digital library objects for off-site storage.

## <span id="structmap">Structural Map</span>

The structural map section of a METS document defines a hierarchical
structure which can be presented to users of the digital library object
to allow them to navigate through it. The \<structMap\> element encodes
this hierarchy as a nested series of \<div\> elements. Each \<div\>
carries attribute information specifying what kind of division it is,
and also may contain multiple METS pointer (\<mptr\>) and file pointer
(\<fptr\>) elements to identify content corresponding with that \<div\>.
METS pointers specify separate METS documents as containing the relevant
file information for the \<div\> containing them. This can be useful
when encoding large collections of material (e.g., an entire journal
run) to keep the size of each METS file in the set relatively small.
File pointers specify files (or in some cases either groups of files or
specific locations within a file) within the current METS document's
\<fileSec\> section that correspond to the portion in the hierarchy
represented by the current \<div\>.

The following provides an example of an extremely simple structural map:

```xml 
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

This structural map shows that we have an oral history (with Mayor
Abraham Beame of New York City) that includes three subsections: an
opening introduction by the interviewer, some family history from Mayor
Beame, and a discussion of how he came to be involved with the teachers'
union in New York. Each of these subsections/divisions is linked to
three files (taken from our earlier example of file groups): an XML
transcription, and a master and derivative audio file. A subsidiary
\<area\> element is used in each \<fptr\> to indicate that this division
corresponds with only a portion of the linked file, and to identify the
exact portion of each linked file. For example, the first division (the
interviewer introduction) is linked to a portion of the XML
transcription file (FILE001) which is found between the two tags in the
transcription file with ID attribute values of "INTVWBG" and "INTVWND."
It is also linked to the two different audio files; in these cases,
rather than specifying ID attribute values within the linked files, the
begin and end points of the linked material within the files is
indicated by a simple time code value of the form HH:MM:SS. So, the
interviewer introduction can be found in both audio files in the segment
beginning at time 00:00:00 in the file and extending through time
00:01:47.

## <span id="structlink">Structural Links</span>

The structural links section of the METS format is the simplest in form
of any of the major METS sections, containing only a single element,
\<smLink\> (although that element may be repeated). The structural links
section of METS is intended to allow you to record the existence of
hyperlinks between items within the structural map, usually \<div\>
elements. This is a useful facility if you wish to use METS to archive
web sites, and wish to maintain a record of the hypertext structure of
the sites separately from the HTML files of the site itself.

As an example, consider the case of a METS document for a web page
containing an image which is hyperlinked to another page. The
\<structMap\> element would probably contain \<divs\> like the following
for the two pages:

```xml 
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

If you wished to indicate that the image file in the \<div\> contained
with the first page \<div\> is hyperlinked to the HTML file in the
second page \<div\>, you would have a \<smLink\> element within the
\<structLink\> section of the METS document as
follows:

```xml 
      <smLink xlink:from="IMG1" xlink:to="P2" xlink:title="Hyperlink from 
      JPEG Image on Page 1 to Page 2" xlink:show="new"
      xlink:actuate="onRequest" />
            
```

The \<smLink\> link element above uses a slightly modified form of the
XLink syntax; all of the XLink attributes are used, but the "to" and
"from" attributes are declared to be of type IDREF rather than NMTOKEN
as in the original XLink specification. This allows you to indicate the
existence of links between any two nodes in the structural map, and also
use XML processing tools to confirm that the linked nodes actually
exist.

## <span id="behavior">Behavior Section</span>

A behavior section can be used to associate executable behaviors with
content in the METS object. A behavior section contains one or more
\<behavior\> elements, each of which has an interface definition element
that represents an abstract definition of the set of behaviors
represented by a particular behavior section. A \<behavior\> also has a
\<mechanism\> element which is used to point to a module of executable
code that implements and runs the behavior defined abstractly by the
interface definition.

Digital object behaviors can be implemented as linkages to distributed
web services as in the following example from the Mellon
[Fedora](http://www.fedora.info/)
project.

```xml 
         <METS:behavior ID="DISS1.1" STRUCTID="S1.1" BTYPE="uva-bdef:stdImage"
      CREATED="2002-05-25T08:32:00" LABEL="UVA Std Image Disseminator"
      GROUPID="DISS1" ADMID="AUDREC1">
           <METS:interfaceDef LABEL="UVA Standard Image Behavior Definition"
        LOCTYPE="URN" xlink:href="uva-bdef:stdImage"/>
           <METS:mechanism LABEL="A NEW AND IMPROVED Image Mechanism"
        LOCTYPE="URN" xlink:href="uva-bmech:BETTER-imageMech"/>
         </METS:behavior>
            
```

See also:

  - [The Fedora Technical
    Specification](http://www.autopenhosting.org/fedora/master-spec-12.20.02.pdf)
    (pdf)

  - [Sample Digital
    Object](http://www.autopenhosting.org/fedora/obj-image-userinput-archdraw.xml)
    (encoded using METS)

  - [Sample Behavior Definition
    Object](http://www.autopenhosting.org/fedora/bdef-image-userinput.xml)
    (encoded using METS)

  - [Sample Behavior Mechanism
    Object](http://www.autopenhosting.org/fedora/bmech-image-userinput-mrsid.xml)
    (encoded using METS)

## Conclusion

The METS schema provides a flexible mechanism for encoding descriptive,
administrative, and structural metadata for a digital library object,
and for expressing the complex links between these various forms of
metadata. It can therefore provide a useful standard for the exchange of
digital library objects between repositories. In addition, METS provides
the ability to associate a digital object with behaviours or services.
The above discussion highlights the major features of the schema, but a
thorough examination of the schema and its included documentation is
necessary to understand the full range of its capabilities.
