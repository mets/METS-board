# METS: An Overview & Tutorial

## Introduction

METS is a standard for the encoding of descriptive, administrative, and
structural metadata for complex digital objects, whether these are text-,
video- or image-based. A [Digital Library Federation](http://www.diglib.org/)
initiative, METS provides an XML document format for encoding metadata
necessary for both the management of digital objects within a repository and
the exchange of such objects between repositories, or between repositories and
their users. Depending on its use, a METS document can  be used in the role of
Submission Information Package (SIP), Archival Information Package (AIP), or
Dissemination Information Package (DIP) within the [Open Archival Information
System (OAIS) Reference
Model](http://nssdc.gsfc.nasa.gov/nost/isoas/ref_model.html).

METS provides a coherent integrated framework for the complex set of metadata
needed for maintaining a library of digital objects. This metadata is both more
extensive than and different from that used for managing collections of printed
works and other physical materials. A book, for instance, will not dissolve
into a series of unconnected pages if its owner fails to record structural
metadata regarding the book's organization, nor will scholars be unable to
evaluate the book's worth if the creator fails to note how it was produced. The
same cannot be said for a digital version of the same book. Without structural
metadata, the page image or text files comprising the digital work are of
little use, and without technical metadata regarding the digitization process,
scholars may be unsure of how accurate a reflection of the original the digital
version provides. An organization must also have access to appropriate
technical metadata for internal management purposes in order, for instance, to
periodically refresh and migrate the data, so ensuring the durability of
valuable resources.

A METS document consists of four main sections:

1.  [**METS Header**](#MHead) - The METS Header contains metadata
    describing the METS document itself, including such information as
    creator, editor, last modification date, etc.

2.  [**Metadata Section**](#mdSec) - The metadata section can provide information regarding descriptive metadata,
    technical metadata describing how the files were created and stored,
    intellectual property rights, metadata regarding the original source object
    from which the digital object derives, and information regarding the provenance
    of the files comprising the digital object (i.e., master/derivative file relationships, and migration/transformation information). The
    metadata element can point to metadata external to the METS document (e.g., a
    MARC record in an OPAC or an EAD finding aid maintained on a WWW server), or
    contain internally embedded metadata, or both. Multiple instances of both
    external and internal metadata may be included in the metadata section.

3.  [**File Section**](#filegrp) - The file section lists all files
    containing content which comprise the electronic versions of the
    digital object. `<file>` elements may be grouped within `<fileGrp>`
    elements, to provide for subdividing the files by object version.

4.  [**Structural Section**](#structSec) - The structural section contains the `structMap` elements that provide hierarchical organizations of the components of the digital object, and links the elements of that structure to content files and metadata that pertain to each element. Multiple structural maps are allowed.

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
the primary identifier for the METS document recorded in the `OBJID`
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

This example contains two attributes on the `<metsHdr>` element,
`CREATEDATE` and `RECORDSTATUS`, which are used to indicate the date and
time the METS record was created, and indicate the status of the
record's processing. Two individual agents are listed who have worked on
this METS record, the person responsible for creating the record and an
archivist responsible for the original material. Both the `ROLE` and `TYPE`
attributes on the `<agent>` element best employ controlled vocabularies.
==TODO: external link to suggested values==

## <span id="mdSec">Metadata</span>

The metadata section of a METS document consists of one or
more `<metadata>` elements. Each `<metadata>`
element may contain a pointer to external metadata (an `<mdRef>`
element), internally embedded metadata (within an `<mdWrap>` element),
or both. Metadata may pertain to the object described in the METS document as a
whole, the original source material used to create the object, or the
individual files comprising the object.

There are five main forms of metadata provided for in a METS document: 
1. Descriptive metadata about the digital object, such as MARC, MODS, EAD,
   Dublin Core, etc;
2. Technical Metadata (information regarding files' creation, format, and use
   characteristics), 3. Intellectual Property Rights Metadata (copyright and
license information), 4. Source Metadata (descriptive and administrative
metadata regarding the analog source from which a digital object derives), and
5. Digital Provenance Metadata (information regarding source/destination
   relationships between files, including master/derivative relationships
between files and information regarding migrations/transformations employed on
files between original digitization of an artifact and its current incarnation
as a digital object).  Each of these five different types of metadata has a
distinct `USE` attribute in the `<metadata>` element: `DESCRIPTIVE`, `TECHNICAL`,
`RIGHTS`, `SOURCE`, or `DIGIPROV`.  `<metadata>` elements may occur as many
times as needed in any METS document with any combination of `USE` attributes.

**External Metadata (mdRef):** an `mdRef` element provides a
URI which may be used in retrieving the external metadata. For example,
the following metadata reference points to the finding aid for a
particular digital object:

```xml 
<metadata USE="DESCRIPTIVE" ID="dmd001">
  <mdRef MIMETYPE="application/xml" MDTYPE="EAD"
    LABEL="Berol Collection Finding Aid" href="urn:x-nyu:fales1735" />
</metadata>       
```

The `<mdRef>` element of this `<metadata>` contains three attributes. The
MIMETYPE attribute allows you to specify the MIME type for the external
descriptive metadata, and the MDTYPE allows you to indicate what form of
metadata is being referenced. Suggested values for the MDTYPE element are
listed ELSEWHERE - TODO.  LABEL provides a mechanism for describing this
metadata to those viewing a METS document, in a 'Table of Contents' display of
the METS document, for example.

**Internal Descriptive Metadata (mdWrap):** An mdWrap element provides a
wrapper around metadata embedded within a METS document. Such metadata
can be in one of two forms: 1. XML-encoded metadata, with the
XML-encoding identifying itself as belonging to a namespace other than
the METS document namespace, or 2. any arbitrary binary or textual form,
PROVIDED that the metadata is Base64 encoded and wrapped in a
`<binData>` element within the mdWrap element. The following examples
demonstrate the use of the mdWrap element:

```xml
<metadata ID="dmd002" USE="DESCRIPTIVE">
  <mdWrap MIMETYPE="text/xml" MDTYPE="DC" LABEL="Dublin Core Metadata">
    <xmlData>
      <dc:title>Alice's Adventures in Wonderland</dc:title>
      <dc:creator>Lewis Carroll</dc:creator>
      <dc:date>between 1872 and 1890</dc:date>
      <dc:publisher>McCloughlin Brothers</dc:publisher>
      <dc:type>text</dc:type>
    </xmlData>
  </mdWrap>
</metadata>           
```

```xml 
<metadata ID="dmd003" USE="DESCRIPTIVE">
  <mdWrap MIMETYPE="application/marc" MDTYPE="MARC" LABEL="OPAC Record">
    <binData>MDI0ODdjam0gIDIyMDA1ODkgYSA0NU0wMDAxMDA...(etc.)</binData>
  </mdWrap>
</metadata>           
```

Note that all `<metadata>` elements must possess an ID attribute. This
attribute provides a unique, internal name for each `<metadata>` element which
can be used in the structural map to link a particular division of the document
hierarchy to a particular `<metadata>` element. This allows specific sections
of metadata to be linked to specific parts of the digital object.

Technical, rights, source, and digital provenance metadata can be included 
in the same way:

```xml
<metadata USE="TECHNICAL" ID="techmd001">
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

A `<file>` element within a `<fileGrp>` might then identify this
administrative metadata as pertaining to the file it identifies by using
an MDID attribute to point to this `<metadata>` element:

```xml
<file ID="FILE001" MDID="techmd001">
  <FLocat href="http://dlib.nyu.edu/press/testimg.tif" />
</file>
```

## <span id="filegrp">File Section</span>

The file section (`<fileSec>`) contains one or more `<fileGrp>` elements
used to group together related files. A `<fileGrp>` lists all of the
files which comprise a single electronic version of the digital
object. For example, there might be separate `<fileGrp>` elements for
the thumbnails, the master archival images, the pdf versions, the TEI
encoded text versions, etc.

Consider the following example of a file section from a digital
object for an oral history which has three different versions: a
TEI-encoded transcript, a master audio file in WAV format, and a
derivative audio file in MP3 format:

```xml 
<fileSec>
  <fileGrp ID="VERS1">
    <file ID="FILE001" MIMETYPE="application/xml" SIZE="257537" CREATED="2001-06-10T00:00:00Z">
      <FLocat href="http://dlib.nyu.edu/tamwag/beame.xml" />
    </file>
  </fileGrp>
  <fileGrp ID="VERS2">
    <file ID="FILE002" MIMETYPE="audio/wav" SIZE="64232836"
      CREATED="2001-05-17T00:00:00Z" GROUPID="AUDIO1">
      <FLocat href="http://dlib.nyu.edu/tamwag/beame.wav" />
    </file>
  </fileGrp>
  <fileGrp ID="VERS3" VERSDATE="2001-05-18T00:00:00Z">
    <file ID="FILE003" MIMETYPE="audio/mpeg" SIZE="8238866"
      CREATED="2001-05-18T00:00:00Z" GROUPID="AUDIO1">
      <FLocat href="http://dlib.nyu.edu/tamwag/beame.mp3" />
    </file>
  </fileGrp>
</fileSec>
```

In this case, the `<fileSec>` contains three subsidiary `<fileGrp>`
elements, one for each different version of the object. The first is an
XML-encoded transcription file, the second is a master audio file in WAV
format, and the third is a derivative audio file in MP3 format. While
such a basic example does not really seem to need the `<fileGrp>`
elements to distinguish the different versions of the object,
`<fileGrp>` becomes much more useful for objects consisting of large
numbers of scanned page images, or indeed any case where a single
version of the object consists of a large number of files. In those
cases, being able to separate `<file>` elements into `<fileGrp>`s makes
identifying the files belonging to a particular version of the document
a simple task.

You may note the presence of the GROUPID attributes with identical
values on the two audio `<file>` elements; these indicate that the two
files, while belonging to different versions of the object, contain the
same basic information (you can use the GROUPID for the same purpose to
indicate equivalent page image files in digital objects
containing many scanned page images).

You should also note that all of the `<file>` elements have a unique ID
attribute. This attribute provides a unique, internal name for this file
which can be referenced by other portions of the document. You’ll see
this type of referencing in action when we look at the Structural Map
Section.

It should be mentioned that `<file>` elements may possess an
`<FContent>` element rather than an `<FLocat>` element. `<FContent>`
elements are used to embed the actual contents of the file within the
METS document; if this is done, the file contents must either be in XML
format or be Base64-encoded. While embedding files is not something one
would typically do when preparing a METS document for use in displaying
a digital objects to users, it can be a valuable feature for
exchanging digital objects between repositories, or for
archiving versions of digital objects for off-site storage.

## <span id="structSec">Structural Section</span>

The structural map section of a METS document defines a hierarchical
structure which can be presented to users of the digital  object
to allow them to navigate through it. The `<structMap>` element encodes
this hierarchy as a nested series of `<div>` elements. Each `<div>`
carries attribute information specifying what kind of division it is,
and also may contain multiple METS pointer (`<mptr>`) and file pointer
(`<fptr>`) elements to identify content corresponding with that `<div>`.
METS pointers specify separate METS documents as containing the relevant
file information for the `<div>` containing them. This can be useful
when encoding large collections of material (e.g., an entire journal
run) to keep the size of each METS file in the set relatively small.
File pointers specify files (or in some cases either groups of files or
specific locations within a file) within the current METS document's
`<fileSec>` section that correspond to the portion in the hierarchy
represented by the current `<div>`.

The following provides an example of an extremely simple structural map:

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

This structural map shows that we have an oral history (with Mayor
Abraham Beame of New York City) that includes three subsections: an
opening introduction by the interviewer, some family history from Mayor
Beame, and a discussion of how he came to be involved with the teachers'
union in New York. Each of these subsections/divisions is linked to
three files (taken from our earlier example of file groups): an XML
transcription, and a master and derivative audio file. A subsidiary
`<area>` element is used in each `<fptr>` to indicate that this division
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

## Conclusion

The METS schema provides a flexible mechanism for encoding descriptive,
administrative, and structural metadata for a digital object and its parts, and
for expressing the complex links between these various forms of metadata and
constituent parts of the object. It can therefore provide a useful standard for
the exchange of digital objects between repositories.  The above discussion
highlights the major features of the schema, but a thorough examination of the
schema and its included documentation is necessary to understand the full range
of its capabilities.
