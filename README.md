# Table of Contents

- [XML](#xml)
    * [XML Key terminology](#xml-key-terminology)
    * [Well-formed XML](#well-formed-xml)
    * [XML DTD](#xml-dtd)
    * [XSD](#xsd)
    * [SAX](#sax)
    * [DOM](#dom)
    * [StAX](#stax)
    * [XSLT](#xslt)
    * [XPath](#xpath)
    * [XQuery](#xquery)
- [JSON](#json)
    * [Syntax](#syntax)
    * [JSON Schema](#json-schema)
    * [JSON vs XML](#json-vs-xml)
- [YAML](#yaml)
- [IDL](#idl)
- [Protobuf](#protobuf)
    * [How it works](#how-it-works)
    * [Protobuf vs XML](#protobuf-vs-xml)
    * [proto3](#proto3)
    * [Schema evolution](#schema-evolution)
- [Thrift](#thrift)
    * [Architecture](#architecture)
    * [Schema evolution](#schema-evolution)
    * [Thrift vs Protobuf](#thrift-vs-protobuf)
- [ASN.1](#asn1)
- [Avro](#avro)
    * [Schemas](#schemas)
    * [Schema evolution](#schema-evolution)
    * [Comparison with other systems](#comparison-with-other-systems)
   
# XML

In computing, Extensible Markup Language (XML) is a markup language that defines a set of rules for encoding documents in a format that is both human-readable and machine-readable. 

The design goals of XML emphasize simplicity, generality, and usability across the Internet. It is a textual data format with strong support via Unicode for different human languages. Although the design of XML focuses on documents, the language is widely used for the representation of arbitrary data structures such as those used in web services.

Several schema systems exist to aid in the definition of XML-based languages, while programmers have developed many application programming interfaces (APIs) to aid the processing of XML data.

Some bulet points about XML:

* XML stands for eXtensible Markup Language
* XML is a markup language much like HTML
* XML was designed to store and transport data
* XML was designed to be self-descriptive
* XML is a W3C Recommendation

## XML Key terminology

**Tag**

A tag is a markup construct that begins with < and ends with >. Tags come in three flavors:

* start-tag, such as <section>;
* end-tag, such as </section>;
* empty-element tag, such as <line-break />.

**Element**

An element is a logical document component that either begins with a start-tag and ends with a matching end-tag or consists only of an empty-element tag. The characters between the start-tag and end-tag, if any, are the element's content, and may contain markup, including other elements, which are called child elements. 

An example is 
```
<greeting>Hello, world!</greeting>
<line-break />
```

**Attribute**

An attribute is a markup construct consisting of a name–value pair that exists within a start-tag or empty-element tag. 

An example is 

```
<img src="madonna.jpg" alt="Madonna" />, 
```

where the names of the attributes are "src" and "alt", and their values are "madonna.jpg" and "Madonna" respectively. 

Another example is 

```
<step number="3">Connect A to B.</step>
``` 

where the name of the attribute is "number" and its value is "3". An XML attribute can only have a single value and each attribute can appear at most once on each element. In the common situation where a list of multiple values is desired, this must be done by encoding the list into a well-formed XML attribute with some format beyond what XML defines itself. Usually this is either a comma or semi-colon delimited list or, if the individual values are known not to contain spaces, a space-delimited list can be used. 

```
<div class="inner greeting-box">Welcome!</div>
```

where the attribute "class" has both the value "inner greeting-box" and also indicates the two CSS class names "inner" and "greeting-box".

## Well-formed XML

A well-formed document in XML is a document that "adheres to the syntax rules specified by the XML 1.0 specification in that it must satisfy both physical and logical structures"

Some key points in the fairly lengthy list include:

* The document contains only properly encoded legal Unicode characters.
* None of the special syntax characters such as < and & appear except when performing their markup-delineation roles.
* The start-tag, end-tag, and empty-element tag that delimit elements are correctly nested, with none missing and none overlapping.
* Tag names are case-sensitive; the start-tag and end-tag must match exactly.
* Tag names cannot contain any of the characters !"#$%&'()*+,/;<=>?@[\]^`{|}~, nor a space character, and cannot begin with "-", ".", or a numeric digit.
* A single root element contains all the other elements.

```
<?xml version="1.0" encoding="UTF-8"?>
<note>
    <to>Tove</to>
    <from>Jani</from>
    <heading>Reminder</heading>
    <body>Don't forget me this weekend!</body>
</note>
```

Well formed XML documents simply markup pages with descriptive tags. You don't need to describe or explain what these tags mean. In other words a well formed XML document does not need a DTD, but it must conform to the XML syntax rules. If all tags in a document are correctly formed and follow XML guidelines, then a document is considered as well formed

## XML DTD

A document type definition (DTD) is a set of markup declarations that define a document type for an SGML-family markup language (SGML, XML, HTML).

A Document Type Definition (DTD) defines the legal building blocks of an XML document. It defines the document structure with a list of legal elements and attributes. A DTD can be declared inline inside an XML document, or as an external reference.

Document Type Definition (DTD) is the oldest schema language for XML.

The purpose of a DTD is to define the structure of an XML document. It defines the structure with a list of legal elements:

```
<!DOCTYPE note
[
<!ELEMENT note (to,from,heading,body)>
<!ELEMENT to (#PCDATA)>
<!ELEMENT from (#PCDATA)>
<!ELEMENT heading (#PCDATA)>
<!ELEMENT body (#PCDATA)>
]>

#PCDATA means parse-able text data.
```

The DTD above is interpreted like this:

* !DOCTYPE note defines that the root element of the document is note
* !ELEMENT note defines that the note element must contain the elements: "to, from, heading, body"
* !ELEMENT to defines the to element to be of type "#PCDATA"
* !ELEMENT from defines the from element to be of type "#PCDATA"
* !ELEMENT heading defines the heading element to be of type "#PCDATA"
* !ELEMENT body defines the body element to be of type "#PCDATA"

An XML document with **correct syntax** is called **"Well Formed"**.

An XML document **validated against a DTD** is both **"Well Formed"** and **"Valid"**.

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE note SYSTEM "Note.dtd">
<note>
    <to>Tove</to>
    <from>Jani</from>
    <heading>Reminder</heading>
    <body>Don't forget me this weekend!</body>
</note>
```

**When to use a DTD**

* With a DTD, independent groups of people can agree to use a standard DTD for interchanging data.
* With a DTD, you can verify that the data you receive from the outside world is valid.
* You can also use a DTD to verify your own data.

**When NOT to use a DTD**

* XML does not require a DTD/Schema.
* When you are experimenting with XML, or when you are working with small XML files, creating DTDs may be a waste of time.
* If you develop applications, wait until the specification is stable before you add a document definition. Otherwise, your software might stop working because of validation errors.

## XSD

XSD (XML Schema Definition), a recommendation of the World Wide Web Consortium (W3C), specifies how to formally describe the elements in an Extensible Markup Language (XML) document. It can be used by programmers to verify each piece of item content in a document. They can check if it adheres to the description of the element it is placed in.

Like all XML schema languages, XSD can be used to express a set of rules to which an XML document must conform in order to be considered "valid" according to that schema. However, unlike most other schema languages, XSD was also designed with the intent that determination of a document's validity would produce a collection of information adhering to specific data types. Such a post-validation infoset can be useful in the development of XML document processing software.

XSDs are far more powerful than DTDs in describing XML languages. They use a rich datatyping system and allow for more detailed constraints on an XML document's logical structure. XSDs also use an XML-based format, which makes it possible to use ordinary XML tools to help process them.

The purpose of an XML Schema is to define the legal building blocks of an XML document:

* the elements and attributes that can appear in a document
* the number of (and order of) child elements
* data types for elements and attributes
* default and fixed values for elements and attributes

XML Schema is an XML-based alternative to DTD:

```
<xs:element name="note">

<xs:complexType>
  <xs:sequence>
    <xs:element name="to" type="xs:string"/>
    <xs:element name="from" type="xs:string"/>
    <xs:element name="heading" type="xs:string"/>
    <xs:element name="body" type="xs:string"/>
  </xs:sequence>
</xs:complexType>

</xs:element>
```

The Schema above is interpreted like this:

* <xs:element name="note"> defines the element called "note"
* <xs:complexType> the "note" element is a complex type
* <xs:sequence> the complex type is a sequence of elements
* <xs:element name="to" type="xs:string"> the element "to" is of type string (text)
* <xs:element name="from" type="xs:string"> the element "from" is of type string
* <xs:element name="heading" type="xs:string"> the element "heading" is of type string
* <xs:element name="body" type="xs:string"> the element "body" is of type string

One of the greatest strength of XML Schemas is the support for data types.

* It is easier to describe allowable document content
* It is easier to validate the correctness of data
* It is easier to define data facets (restrictions on data)
* It is easier to define data patterns (data formats)
* It is easier to convert data between different data types

Another great strength about XML Schemas is that they are written in XML.

* You don't have to learn a new language
* You can use your XML editor to edit your Schema files
* You can use your XML parser to parse your Schema files
* You can manipulate your Schema with the XML DOM
* You can transform your Schema with XSLT
* XML Schemas are extensible, because they are written in XML.

## SAX

**Simple API for XML (SAX)** is an event-based API for parsing an XML document sequentially from start to finish. 

When a SAX-oriented parser encounters an item from the document’s infoset (an abstract data model describing an XML document’s information;), it makes this item available to an application as an event by calling one of the methods in one of the application’s handlers (objects whose methods are called by the parser to make event information available), which the application has previously registered with the parser. The application can then consume this event by processing the infoset item in some manner.

A SAX parser is more memory efficient than a DOM parser in that it doesn’t require the entire document to fit into memory. This benefit becomes a drawback for using XPath and XSLT, which require that the entire document be stored in memory.

A SAX parser only needs to report each parsing event as it happens, and normally discards almost all of that information once reported (it does, however, keep some things, for example a list of all elements that have not been closed yet, in order to catch later errors such as end-tags in the wrong order). Thus, the minimum memory required for a SAX parser is proportional to the maximum depth of the XML file (i.e., of the XML tree) and the maximum data involved in a single XML event (such as the name and attributes of a single start-tag, or the content of a processing instruction, etc.).

This much memory is usually considered negligible. A DOM parser, in contrast, has to build a tree representation of the entire document in memory to begin with, thus using memory that increases with the entire document length. This takes considerable time and space for large documents (memory allocation and data-structure construction take time). The compensating advantage, of course, is that once loaded any part of the document can be accessed in any order.

Because of the event-driven nature of SAX, processing documents is generally far faster than DOM-style parsers, so long as the processing can be done in a start-to-end pass. Many tasks, such as indexing, conversion to other formats, very simple formatting, and the like, can be done that way. Other tasks, such as sorting, rearranging sections, getting from a link to its target, looking up information on one element to help process a later one, and the like, require accessing the document structure in complex orders and will be much faster with DOM than with multiple SAX passes.

SAX exists in two major versions. Java implements SAX 1 through the javax.xml.parsers package’s abstract SAXParser and SAXParserFactory classes, and implements SAX 2 through the org.xml.sax package’s XMLReader interface and through the org.xml.sax.helpers package’s XMLReaderFactory class. The org.xml.sax, org.xml.sax.ext, and org.xml.sax.helpers packages provide various types that augment both Java implementations.

XMLReader makes available several methods for configuring the parser and parsing a document’s content. Some of these methods get and set the content handler, DTD handler, entity resolver, and error handler, which are described by the ContentHandler, DTDHandler, EntityResolver, and ErrorHandler interfaces. After learning about XMLReader’s methods and these interfaces, you learned about the nonstandard LexicalHandler interface and how to create a custom entity resolver.

![sax](https://github.com/rgederin/data-formats/blob/master/img/sax.jpg)

## DOM

**Document Object Model (DOM)** is an API for parsing an XML document into an in-memory tree of nodes, and for creating an XML document from a node tree. After a DOM parser creates a tree, an application uses the DOM API to navigate over and extract infoset items from the tree’s nodes.

DOM has two big advantages over SAX:

* DOM permits random access to a document’s infoset items, whereas SAX only permits serial access.
* DOM also lets you create XML documents, whereas you can only parse documents with SAX .

However, SAX is advantageous over DOM in that it can parse documents of arbitrary sizes, whereas the size of documents parsed or created by DOM is limited by the amount of available memory for storing the document’s node-based tree structure.

DOM views an XML document as a tree that is composed of several kinds of nodes. This tree has a single root node and all nodes, except for the root, have a parent node. Also, each node has a list of child nodes. When this list is empty, the child node is known as a leaf node.

DOM views an XML document as a tree that’s composed of several kinds of nodes: attribute, CDATA section, comment, document, document fragment, document type, element, entity, entity reference, notation, processing instruction, and text.

![dom](https://github.com/rgederin/data-formats/blob/master/img/dom.jpg)

## StAX

**Streaming API for XML (StAX)** is an application programming interface (API) to read and write XML documents, originating from the Java programming language community.

Traditionally, XML APIs are either:

* DOM based - the entire document is read into memory as a tree structure for random access by the calling application
* event based - the application registers to receive events as entities are encountered within the source document.

Both have advantages: DOM, for example, allows for random access to the document, and event driven algorithm like SAX has a small memory footprint and is typically much faster.

These two access metaphors can be thought of as polar opposites. A tree based API allows unlimited, random access and manipulation, while an event based API is a 'one shot' pass through the source document.

StAX was designed as a median between these two opposites. In the StAX metaphor, the programmatic entry point is a cursor that represents a point within the document. The application moves the cursor forward - 'pulling' the information from the parser as it needs. This is different from an event based API - such as SAX - which 'pushes' data to the application - requiring the application to maintain state between events as necessary to keep track of location within the document.

### StAX vs. SAX and DOM

* StAX (like SAX) can be used to parse documents of arbitrary sizes. In contrast, the maximum size of documents parsed by DOM is limited by the available memory, which makes DOM unsuitable for mobile devices with limited amounts of memory.

* StAX (like DOM) can be used to create documents. In contrast to DOM, which can create documents whose maximum size is constrained by available memory, StAX can create documents of arbitrary sizes. SAX cannot be used to create documents.

* StAX (like SAX) makes infoset items available to applications almost immediately. In contrast, these items are not made available by DOM until after it finishes building the tree of nodes.

* StAX (like DOM) adopts the pull model, in which the application tells the parser when it’s ready to receive the next infoset item. This model is based on the iterator design pattern, which results in an application that’s easier to write and debug. In contrast, SAX adopts the push model, in which the parser passes infoset items via events to the application, whether or not the application is ready to receive them. This model is based on the observer design pattern, which results in an application that’s often harder to write and debug.

Summing up, StAX can parse or create documents of arbitrary sizes, makes infoset items available to applications almost immediately, and uses the pull model to put the application in charge. Neither SAX nor DOM offers all of these advantages.

![stax](https://github.com/rgederin/data-formats/blob/master/img/stax.jpg)

![pull](https://github.com/rgederin/data-formats/blob/master/img/pull-push.jpg)

### Stream-Based vs. Event-Based Readers and Writers

StAX parsers are known as document readers, and StAX document creators are known as document writers. StAX classifies document readers and document writers as stream-based or event-based.

A stream-based reader extracts the next infoset item from an input stream via a cursor (infoset item pointer). Similarly, a stream-based writer writes the next infoset item to an output stream at the cursor position. The cursor can point to only one item at a time, and always moves forward, typically by one infoset item.

Stream-based readers and writers are appropriate when writing code for memory-constrained environments such as Java ME because you can use them to create smaller and more efficient code. They also offer better performance for low-level libraries, where performance is important.

An event-based reader extracts the next infoset item from an input stream by obtaining an event. Similarly, an event-based writer writes the next infoset item to the stream by adding an event to the output stream. In contrast to stream-based readers and writers, event-based readers and writers have no concept of a cursor.

Event-based readers and writers are appropriate for creating XML processing pipelines (sequences of components that transform the previous component’s input and pass the transformed output to the next component in the sequence), for mo difying an event sequence, and more.

## XSLT

XSLT (Extensible Stylesheet Language Transformations) is a language for transforming XML documents into other XML documents, or other formats such as HTML for web pages, plain text or XSL Formatting Objects, which may subsequently be converted to other formats, such as PDF, PostScript and PNG

XSLT accomplishes its work by using XSLT processors and stylesheets . An XSLT processor is a software component that applies an XSLT stylesheet (an XML-based template consisting of content and transformation instructions) to an input document (without modifying the document), and copies the transformed result to a result tree, which can be output to a file or output stream, or even piped into another XSLT processor for additional transformations. 

The beauty of XSLT is that you don’t need to develop custom software applications to perform the transformations. Instead, you simply create an XSLT stylesheet and input it along with the XML document needing to be transformed to an XSLT processor.

![xslt](https://github.com/rgederin/data-formats/blob/master/img/xslt.png)

Basic XML document

```
<?xml version="1.0" ?>
<persons>
  <person username="JS1">
    <name>John</name>
    <family-name>Smith</family-name>
  </person>
  <person username="MI1">
    <name>Morka</name>
    <family-name>Ismincius</family-name>
  </person>
</persons>
```

This XSLT stylesheet provides templates to transform the XML document:

```
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" version="1.0">
  <xsl:output method="xml" indent="yes"/>

  <xsl:template match="/persons">
    <root>
      <xsl:apply-templates select="person"/>
    </root>
  </xsl:template>

  <xsl:template match="person">
    <name username="{@username}">
      <xsl:value-of select="name" />
    </name>
  </xsl:template>

</xsl:stylesheet>
```

Its evaluation results in a new XML document, having another structure:

```
<?xml version="1.0" encoding="UTF-8"?>
<root>
  <name username="JS1">John</name>
  <name username="MI1">Morka</name>
</root>
```

Processing the following example XSLT file

```
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet
  version="1.0"
  xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
  xmlns="http://www.w3.org/1999/xhtml">

  <xsl:output method="xml" indent="yes" encoding="UTF-8"/>

  <xsl:template match="/persons">
    <html>
      <head> <title>Testing XML Example</title> </head>
      <body>
        <h1>Persons</h1>
        <ul>
          <xsl:apply-templates select="person">
            <xsl:sort select="family-name" />
          </xsl:apply-templates>
        </ul>
      </body>
    </html>
  </xsl:template>

  <xsl:template match="person">
    <li>
      <xsl:value-of select="family-name"/><xsl:text>, </xsl:text><xsl:value-of select="name"/>
    </li>
  </xsl:template>

</xsl:stylesheet>
```

with the XML input file shown above results in the following XHTML (whitespace has been adjusted here for clarity):

```
<?xml version="1.0" encoding="UTF-8"?>
<html xmlns="http://www.w3.org/1999/xhtml">
  <head> <title>Testing XML Example</title> </head>
  <body>
    <h1>Persons</h1>
      <ul>
        <li>Ismincius, Morka</li>
        <li>Smith, John</li>
      </ul>
  </body>
</html>
```

Java implements XSLT through the types in the javax.xml.transform, javax.xml.transform.dom, javax.xml.transform.sax, javax.xml.transform.stax, and javax.xml.transform.stream packages. The javax.xml.transform package defines the generic APIs for processing transformation instructions and for performing a transformation from a source (where the XSLT processor’s input originates) to a result (where the processor’s output is sent). The remaining packages define the APIs for obtaining different kinds of sources and results.

## XPath

XPath is a nonXML declarative query language (defined by the W3C) for selecting an XML document’s infoset items as one or more nodes. 

The XPath language is based on a tree representation of the XML document, and provides the ability to navigate around the tree, selecting nodes by a variety of criteria

As well as simplifying access to a DOM tree’s nodes, XPath is commonly used in the context of XSLT where it’s typically employed to select (via XPath expressions) those input document elements that are to be copied to an output document. 

Given a sample XML document

```
<?xml version="1.0" encoding="utf-8"?>
<Wikimedia>
  <projects>
    <project name="Wikipedia" launch="2001-01-05">
      <editions>
        <edition language="English">en.wikipedia.org</edition>
        <edition language="German">de.wikipedia.org</edition>
        <edition language="French">fr.wikipedia.org</edition>
        <edition language="Polish">pl.wikipedia.org</edition>
        <edition language="Spanish">es.wikipedia.org</edition>
      </editions>
    </project>
    <project name="Wiktionary" launch="2002-12-12">
      <editions>
        <edition language="English">en.wiktionary.org</edition>
        <edition language="French">fr.wiktionary.org</edition>
        <edition language="Vietnamese">vi.wiktionary.org</edition>
        <edition language="Turkish">tr.wiktionary.org</edition>
        <edition language="Spanish">es.wiktionary.org</edition>
      </editions>
    </project>
  </projects>
</Wikimedia>
```

Select name attributes for all projects:

```
/Wikimedia/projects/project/@name
```

Select all editions of all projects

```
/Wikimedia//editions
```

Select addresses of all English Wikimedia projects (text of all edition elements where language attribute is equal to English):

```
/Wikimedia/projects/project/editions/edition[@language='English']/text()
```

## XQuery

XQuery (XML Query) is a query and functional programming language that queries and transforms collections of structured and unstructured data, usually in the form of XML, text and with vendor-specific extensions for other data formats (JSON, binary, etc.). The language is developed by the XML Query working group of the W3C. The work is closely coordinated with the development of XSLT by the XSL Working Group; the two groups share responsibility for XPath, which is a subset of XQuery.

* XQuery 1.0 became a W3C Recommendation on January 23, 2007.
* XQuery 3.0 became a W3C Recommendation on April 8, 2014.
* XQuery 3.1 became a W3C Recommendation on March 21, 2017.

The mission of the XML Query project is to provide flexible query facilities to extract data from real and virtual documents on the World Wide Web, therefore finally providing the needed interaction between the Web world and the database world. Ultimately, collections of XML files will be accessed like databases

XQuery provides the means to extract and manipulate data from XML documents or any data source that can be viewed as XML, such as relational databases[9] or office documents.

XQuery contains a superset of XPath expression syntax to address specific parts of an XML document. It supplements this with a SQL-like "FLWOR expression" for performing joins. A FLWOR expression is constructed from the five clauses after which it is named: FOR, LET, WHERE, ORDER BY, RETURN.

The language also provides syntax allowing new XML documents to be constructed. Where the element and attribute names are known in advance, an XML-like syntax can be used; in other cases, expressions referred to as dynamic node constructors are available. All these constructs are defined as expressions within the language, and can be arbitrarily nested.

The language is based on the XQuery and XPath Data Model (XDM) which uses a tree-structured model of the information content of an XML document, containing seven kinds of nodes: document nodes, elements, attributes, text nodes, comments, processing instructions, and namespaces.

XDM also models all values as sequences (a singleton value is considered to be a sequence of length one). The items in a sequence can either be XML nodes or atomic values. Atomic values may be integers, strings, booleans, and so on: the full list of types is based on the primitive types defined in XML Schema.

**Applications**

Below are a few examples of how XQuery can be used:

* Extracting information from a database for use in a web service.
* Generating summary reports on data stored in an XML database.
* Searching textual documents on the Web for relevant information and compiling the results.
* Selecting and transforming XML data to XHTML to be published on the Web.
* Pulling data from databases to be used for the application integration.
* Splitting up an XML document that represents multiple transactions into multiple XML documents.

### XQuery and XSLT compared

**Scope**

Although XQuery was initially conceived as a query language for large collections of XML documents, it is also capable of transforming individual documents. As such, its capabilities overlap with XSLT, which was designed expressly to allow input XML documents to be transformed into HTML or other formats.

The XSLT 2.0 and XQuery standards were developed by separate working groups within W3C, working together to ensure a common approach where appropriate. They share the same data model (XDM), type system, and function library, and both include XPath 2.0 as a sublanguage.

**Origin**

The two languages, however, are rooted in different traditions and serve the needs of different communities. XSLT was primarily conceived as a stylesheet language whose primary goal was to render XML for the human reader on screen, on the web (as web template language), or on paper. XQuery was primarily conceived as a database query language in the tradition of SQL.

Because the two languages originate in different communities, XSLT is stronger in its handling of narrative documents with more flexible structure, while XQuery is stronger in its data handling (for example, when performing relational joins).

### XQuery API for Java (XQJ) 

XQuery API for Java (XQJ) refers to the common Java API for the W3C XQuery 1.0 specification.

The XQJ API enables Java programmers to execute XQuery against an XML data source (e.g. an XML database) while reducing or eliminating vendor lock in.

The XQJ API provides Java developers with an interface to the XQuery Data Model. Its design is similar to the JDBC API which has a client/server feel and as such lends itself well to Server-based XML Databases and less well to client-side XQuery processors, although the "connection" part is a very minor part of the entire API. Users of the XQJ API can bind Java values to XQuery expressions, preventing code injection attacks. Also, multiple XQuery expressions can be executed as part of an atomic transaction.

# JSON

In computing, JavaScript Object Notation or JSON is an open-standard file format that uses human-readable text to transmit data objects consisting of attribute–value pairs and array data types (or any other serializable value). It is a very common data format used for asynchronous browser–server communication, including as a replacement for XML in some AJAX-style systems.

JSON is a language-independent data format. It was derived from JavaScript, but as of 2017 many programming languages include code to generate and parse JSON-format data. The official Internet media type for JSON is application/json. JSON filenames use the extension .json.

JSON is also used with NoSQL database management systems such as MongoDb and CouchDb; with apps from social media web sites such as Twitter, Facebook, LinkedIn, and Flickr; and even with the popular Google Maps API.
 
## Syntax

JSON's basic data types are:

* Number: a signed decimal number that may contain a fractional part and may use exponential E notation, but cannot include non-numbers such as NaN. The format makes no distinction between integer and floating-point. JavaScript uses a double-precision floating-point format for all its numeric values, but other languages implementing JSON may encode numbers differently.
* String: a sequence of zero or more Unicode characters. Strings are delimited with double-quotation marks and support a backslash escaping syntax.
* Boolean: either of the values true or false
* Array: an ordered list of zero or more values, each of which may be of any type. Arrays use square bracket notation and elements are comma-separated.
* Object: an unordered collection of name–value pairs where the names (also called keys) are strings. Since objects are intended to represent associative arrays, it is recommended, though not required, that each key is unique within an object. Objects are delimited with curly brackets and use commas to separate each pair, while within each pair the colon ':' character separates the key or name from its value.
* null: An empty value, using the word null

Limited whitespace is allowed and ignored around or between syntactic elements (values and punctuation, but not within a string value). Only four specific characters are considered whitespace for this purpose: space, horizontal tab, line feed, and carriage return. In particular, the byte order mark must not be generated by a conforming implementation (though it may be accepted when parsing JSON). JSON does not provide syntax for comments.

```
{
  "firstName": "John",
  "lastName": "Smith",
  "isAlive": true,
  "age": 27,
  "address": {
    "streetAddress": "21 2nd Street",
    "city": "New York",
    "state": "NY",
    "postalCode": "10021-3100"
  },
  "phoneNumbers": [
    {
      "type": "home",
      "number": "212 555-1234"
    },
    {
      "type": "office",
      "number": "646 555-4567"
    },
    {
      "type": "mobile",
      "number": "123 456-7890"
    }
  ],
  "children": [],
  "spouse": null
}
```

## JSON Schema

JSON Schema specifies a JSON-based format to define the structure of JSON data for validation, documentation, and interaction control. It provides a contract for the JSON data required by a given application, and how that data can be modified.

JSON Schema is based on the concepts from XML Schema (XSD), but is JSON-based. As in XSD, the same serialization/deserialization tools can be used both for the schema and data; and is self-describing. It is described in an Internet Draft currently in its 6th draft, which was released on April 15, 2017. Draft 4 expired on August 4, 2013, but continued to be used in the lapse of more than 3 years between its expiration and the release of Draft 5. There are several validators available for different programming languages, each with varying levels of conformance.

There is no standard file extension, but some have suggested .schema.json.

Example:

```
{
  "$schema": "http://json-schema.org/schema#",
  "title": "Product",
  "type": "object",
  "required": ["id", "name", "price"],
  "properties": {
    "id": {
      "type": "number",
      "description": "Product identifier"
    },
    "name": {
      "type": "string",
      "description": "Name of the product"
    },
    "price": {
      "type": "number",
      "minimum": 0
    },
    "tags": {
      "type": "array",
      "items": {
        "type": "string"
      }
    },
    "stock": {
      "type": "object",
      "properties": {
        "warehouse": {
          "type": "number"
        },
        "retail": {
          "type": "number"
        }
      }
    }
  }
}
```

The JSON Schema above can be used to test the validity of the JSON code below:

```
{
  "id": 1,
  "name": "Foo",
  "price": 123,
  "tags": [
    "Bar",
    "Eek"
  ],
  "stock": {
    "warehouse": 300,
    "retail": 20
  }
}
```

![synt](https://github.com/rgederin/data-formats/blob/master/img/syntax.png)

![synt](https://github.com/rgederin/data-formats/blob/master/img/semantics.png)

## JSON vs XML

JSON is Like XML Because

* Both JSON and XML are "self describing" (human readable)
* Both JSON and XML are hierarchical (values within values)
* Both JSON and XML can be parsed and used by lots of programming languages
* Both JSON and XML can be fetched with an XMLHttpRequest

JSON is Unlike XML Because

* JSON doesn't use end tag
* JSON is shorter
* JSON is quicker to read and write
* JSON can use arrays

![jvsx](https://github.com/rgederin/data-formats/blob/master/img/jvsx.jpg)

![jvsx](https://github.com/rgederin/data-formats/blob/master/img/json.png)

# YAML

YAML is a superset of JSON, and as such is a very convenient format for specifying hierarchical configuration data.

YAML (YAML Ain't Markup Language) is a human-readable data serialization language. It is commonly used for configuration files, but could be used in many applications where data is being stored (e.g. debugging output) or transmitted (e.g. document headers). YAML targets many of the same communications applications as XML but has a minimal syntax which intentionally breaks compatibility with SGML. It uses both Python-style indentation to indicate nesting, and a more compact format that uses [] for lists and {} for maps making YAML 1.2 a superset of JSON.

![yaml](https://github.com/rgederin/data-formats/blob/master/img/yaml.png)

# IDL

An interface description language or interface definition language (IDL), is a specification language used to describe a software component's application programming interface (API). IDLs describe an interface in a language-independent way, enabling communication between software components that do not share one language. For example, between those written in C++ and those written in Java.

IDLs are commonly used in remote procedure call software. In these cases the machines at either end of the link may be using different operating systems and computer languages. IDLs offer a bridge between the two different systems.

# Protobuf

Protocol buffers are a flexible, efficient, automated mechanism for serializing structured data – think XML, but smaller, faster, and simpler. You define how you want your data to be structured once, then you can use special generated source code to easily write and read your structured data to and from a variety of data streams and using a variety of languages. You can even update your data structure without breaking deployed programs that are compiled against the "old" format.

The method involves an interface description language that describes the structure of some data and a program that generates source code from that description for generating or parsing a stream of bytes that represents the structured data.

Protocol Buffers offers a concrete RPC protocol stack to use for defined services called gRPC.

Canonically, messages are serialized into a binary wire format which is compact, forward- and backward-compatible, but not self-describing (that is, there is no way to tell the names, meaning, or full datatypes of fields without an external specification).

## How it works

You specify how you want the information you're serializing to be structured by defining protocol buffer message types in .proto files. Each protocol buffer message is a small logical record of information, containing a series of name-value pairs. Here's a very basic example of a .proto file that defines a message containing information about a person:

```
message Person {
  required string name = 1;
  required int32 id = 2;
  optional string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber {
    required string number = 1;
    optional PhoneType type = 2 [default = HOME];
  }

  repeated PhoneNumber phone = 4;
}
```

The message format is simple – each message type has one or more uniquely numbered fields, and each field has a name and a value type, where value types can be numbers (integer or floating-point), booleans, strings, raw bytes, or even (as in the example above) other protocol buffer message types, allowing you to structure your data hierarchically. You can specify optional fields, required fields, and repeated fields. 

You can find more information about writing .proto files in the Protocol Buffer Language Guide.

Once you've defined your messages, you run the protocol buffer compiler for your application's language on your .proto file to generate data access classes.

You can then use this class in your application to populate, serialize, and retrieve Person protocol buffer messages.

You can add new fields to your message formats without breaking backwards-compatibility; old binaries simply ignore the new field when parsing. So if you have a communications protocol that uses protocol buffers as its data format, you can extend your protocol without having to worry about breaking existing code.

## Protobuf vs XML

Protocol buffers have many advantages over XML for serializing structured data. Protocol buffers:

* are simpler
* are 3 to 10 times smaller
* are 20 to 100 times faster
* are less ambiguous
* generate data access classes that are easier to use programmatically

However, protocol buffers are not always a better solution than XML – for instance, protocol buffers would not be a good way to model a text-based document with markup (e.g. HTML), since you cannot easily interleave structure with text. In addition, XML is human-readable and human-editable; protocol buffers, at least in their native format, are not. XML is also – to some extent – self-describing. A protocol buffer is only meaningful if you have the message definition (the .proto file).

## proto3

The most recent version 3 release introduces a new language version - Protocol Buffers language version 3 (aka proto3), as well as some new features in our existing language version (aka proto2). Proto3 simplifies the protocol buffer language, both for ease of use and to make it available in a wider range of programming languages: our current release lets you generate protocol buffer code in Java, C++, Python, Java Lite, Ruby, JavaScript, Objective-C, and C#. In addition you can generate proto3 code for Go using the latest Go protoc plugin, available from the golang/protobuf Github repository. More languages are in the pipeline.
    
Note that the two language version APIs are not completely compatible. To avoid inconvenience to existing users, we will continue to support the previous language version in new protocol buffers releases.

## Schema evolution
    
For understanding schema evolution we need to review how protobuf encode data into bytes. 

The example I will use is a little object describing a person. In JSON I would write it like this:

```
{
    "userName": "Martin",
    "favouriteNumber": 1337,
    "interests": ["daydreaming", "hacking"]
}
```
If I remove all the whitespace it consumes 82 bytes.

The Protocol Buffers schema for the person object might look something like this:

message Person {
    required string user_name        = 1;
    optional int64  favourite_number = 2;
    repeated string interests        = 3;
}

When we encode the data above using this schema, it uses 33 bytes, as follows:

![proto](https://github.com/rgederin/data-formats/blob/master/img/protobuf.png)

Look exactly at how the binary representation is structured, byte by byte. The person record is just the concatentation of its fields. Each field starts with a byte that indicates its tag number (the numbers 1, 2, 3 in the schema above), and the type of the field. If the first byte of a field indicates that the field is a string, it is followed by the number of bytes in the string, and then the UTF-8 encoding of the string. If the first byte indicates that the field is an integer, a variable-length encoding of the number follows. There is no array type, but a tag number can appear multiple times to represent a multi-valued field.

This encoding has consequences for schema evolution:

* There is no difference in the encoding between optional, required and repeated fields (except for the number of times the tag number can appear). This means that you can change a field from optional to repeated and vice versa (if the parser is expecting an optional field but sees the same tag number multiple times in one record, it discards all but the last value). required has an additional validation check, so if you change it, you risk runtime errors (if the sender of a message thinks that it’s optional, but the recipient thinks that it’s required).

* An optional field without a value, or a repeated field with zero values, does not appear in the encoded data at all — the field with that tag number is simply absent. Thus, it is safe to remove that kind of field from the schema. However, you must never reuse the tag number for another field in future, because you may still have data stored that uses that tag for the field you deleted.

* You can add a field to your record, as long as it is given a new tag number. If the Protobuf parser parser sees a tag number that is not defined in its version of the schema, it has no way of knowing what that field is called. But it does roughly know what type it is, because a 3-bit type code is included in the first byte of the field. This means that even though the parser can’t exactly interpret the field, it can figure out how many bytes it needs to skip in order to find the next field in the record.

* You can rename fields, because field names don’t exist in the binary serialization, but you can never change a tag number.

# Thrift

Thrift is an interface definition language and binary communication protocol used for defining and creating services for numerous languages. It forms a remote procedure call (RPC) framework and was developed at Facebook for "scalable cross-language services development". It combines a software stack with a code generation engine to build cross-platform services which can connect applications written in a variety of languages and frameworks, including ActionScript, C, C++, C#, Cappuccino, Cocoa, Delphi, Erlang, Go, Haskell, Java, Node.js, Objective-C, OCaml, Perl, PHP, Python, Ruby and Smalltalk.Although developed at Facebook, it is now an open source project in the Apache Software Foundation. The implementation was described in an April 2007 technical paper released by Facebook, now hosted on Apache.

## Architecture

Thrift includes a complete stack for creating clients and servers. The top part is generated code from the Thrift definition. From this file, the services generate client and processor code. In contrast to built-in types, created data structures are sent as result in generated code. The protocol and transport layer are part of the runtime library. With Thrift, it is possible to define a service and change the protocol and transport without recompiling the code. Besides the client part, Thrift includes server infrastructure to tie protocols and transports together, like blocking, non-blocking, and multi-threaded servers. The underlying I/O part of the stack is implemented differently for different languages.

Thrift supports a number of protocols:

* TBinaryProtocol – A straightforward binary format, simple, but not optimized for space efficiency. Faster to process than the text protocol but more difficult to debug.
* TCompactProtocol – More compact binary format; typically more efficient to process as well
* TDebugProtocol – A human-readable text format to aid in debugging.
* TDenseProtocol – Similar to TCompactProtocol, stripping off the meta information from what is transmitted.
* TJSONProtocol – Uses JSON for encoding of data.
* TSimpleJSONProtocol – A write-only protocol that cannot be parsed by Thrift because it drops metadata using JSON. Suitable for parsing by scripting languages.[9]

The supported transports are:

* TFileTransport – This transport writes to a file.
* TFramedTransport – This transport is required when using a non-blocking server. It sends data in frames, where each frame is preceded by length information.
* TMemoryTransport – Uses memory for I/O. The Java implementation uses a simple ByteArrayOutputStream internally.
* TSocket – Uses blocking socket I/O for transport.
* TZlibTransport – Performs compression using zlib. Used in conjunction with another transport.

Thrift also provides a number of servers, which are

* TNonblockingServer – A multi-threaded server using non-blocking I/O (Java implementation uses NIO channels). TFramedTransport must be used with this server.
* TSimpleServer – A single-threaded server using standard blocking I/O. Useful for testing.
* TThreadPoolServer – A multi-threaded server using standard blocking I/O.

![thrift](https://github.com/rgederin/data-formats/blob/master/img/thrift.png)

## Schema evolution

Thrift is a much bigger project than Avro or Protocol Buffers, as it’s not just a data serialization library, but also an entire RPC framework. It also has a somewhat different culture: whereas Avro and Protobuf standardize a single binary encoding, Thrift embraces a whole variety of different serialization formats (which it calls “protocols”).

Indeed, Thrift has two different JSON encodings, and no fewer than three different binary encodings. (However, one of the binary encodings, DenseProtocol, is only supported in the C++ implementation; since we’re interested in cross-language serialization, I will focus on the other two.)

All the encodings share the same schema definition, in Thrift IDL:

```
struct Person {
  1: string       userName,
  2: optional i64 favouriteNumber,
  3: list<string> interests
}
```

The BinaryProtocol encoding is very straightforward, but also fairly wasteful (it takes 59 bytes to encode our example record):

![binaryprotocol](https://github.com/rgederin/data-formats/blob/master/img/binaryprotocol.png)

The CompactProtocol encoding is semantically equivalent, but uses variable-length integers and bit packing to reduce the size to 34 bytes:

![compactprotocol](https://github.com/rgederin/data-formats/blob/master/img/compactprotocol.png)

As you can see, Thrift’s approach to schema evolution is the same as Protobuf’s: each field is manually assigned a tag in the IDL, and the tags and field types are stored in the binary encoding, which enables the parser to skip unknown fields. Thrift defines an explicit list type rather than Protobuf’s repeated field approach, but otherwise the two are very similar.

## Thrift vs Protobuf

**Same Typical Operation Model**

The typical model of Thrift/Protobuf use is

* Write down a bunch of struct-like message formats in an IDL-like language.
* Run a tool to generate Java/C++/whatever boilerplate code. Example: thrift --gen java MyProject.thrift
* Outputs thousands of lines - but they remain fairly readable in most languages
* Link against this boilerplate when you build your application.
* DO NOT EDIT!

**Similar IDLs**

![pbthriftidl](https://github.com/rgederin/data-formats/blob/master/img/pbthriftidl.png)

**Similar IDL rules**

* Every field must have a unique, positive integer identifier ("= 1"," = 2" or " 1:", " 2:" )
* Fields may be marked as ’required’ or ’optional’
* structs/messages may contain other structs/messages
* You may specify an optional "default" value for a field
* Multiple structs/messages can be defined and referred to within the same .thrift/.proto file
* We should not change tagging integer - it will breaks compatibility

**Similar versioning approach**

* The system must be able to support reading of old data, as well as requests from out-of-date clients to new servers, and vice versa.
* Versioning in Thrift and Protobuf is implemented via field identifiers.
* The combination of this field identifiers and its type specifier is used to uniquely identify the field.
* A new compiling isn't necessary.
* Statically typed systems like CORBA or RMI would require an update of all clients in this case

**Overal comparison**

![pbthriftidl](https://github.com/rgederin/data-formats/blob/master/img/pvst1.png)

![pbthriftidl](https://github.com/rgederin/data-formats/blob/master/img/pvst2.png)

![pbthriftidl](https://github.com/rgederin/data-formats/blob/master/img/pvst3.png)

**Pros/Cons**

![pbthriftidl](https://github.com/rgederin/data-formats/blob/master/img/pvst4.png)

# ASN.1

Abstract Syntax Notation One (ASN.1) is an interface description language for defining data structures that can be serialized and deserialized in a standard, cross-platform way. It is broadly used in telecommunications and computer networking, and especially in cryptography.

Protocol developers define data structures in ASN.1 modules, which are generally a section of a broader standards document written in the ASN.1 language. Because the language is both human-readable and machine-readable, modules can be automatically turned into libraries that process their data structures, using an ASN.1 compiler.

ASN.1 is similar in purpose and use to protocol buffers and Apache Thrift, which are also interface description languages for cross-platform data serialization. Like those languages, it has a schema (in ASN.1, called a "module"), and a set of encodings, typically type-length-value encodings. However, ASN.1, defined in 1984, predates them by many years. It also includes a wider variety of basic data types, some of which are obsolete, and has more options for extensibility. A single ASN.1 message can include data from multiple modules defined in multiple standards, even standards defined years apart.

ASN.1 is used in X.509, which defines the format of certificates used in the HTTPS protocol for securely browsing the web, and in many other cryptographic systems.

It's also used in the PKCS group of cryptography standards, X.400 electronic mail, X.500 and Lightweight Directory Access Protocol (LDAP), H.323 (VoIP), Kerberos, BACnet and simple network management protocol (SNMP), and third- and fourth-generation wireless communications technologies (UMTS, LTE, and WiMAX 2).

# Avro

Avro is a remote procedure call and data serialization framework developed within Apache's Hadoop project. It uses JSON for defining data types and protocols, and serializes data in a compact binary format. Its primary use is in Apache Hadoop, where it can provide both a serialization format for persistent data, and a wire format for communication between Hadoop nodes, and from client programs to the Hadoop services.

It is similar to Thrift and Protocol Buffers, but does not require running a code-generation program when a schema changes (unless desired for statically-typed languages).

Avro provides:

* Rich data structures.
* A compact, fast, binary data format.
* A container file, to store persistent data.
* Remote procedure call (RPC).
* Simple integration with dynamic languages. Code generation is not required to read or write data files nor to use or implement RPC protocols. Code generation as an optional optimization, only worth implementing for statically typed languages.

Full specification availabel on [official web site](https://avro.apache.org/docs/current/spec.html)

## Schemas

Avro relies on schemas. When Avro data is read, the schema used when writing it is always present. This permits each datum to be written with no per-value overheads, making serialization both fast and small. This also facilitates use with dynamic, scripting languages, since data, together with its schema, is fully self-describing.

When Avro data is stored in a file, its schema is stored with it, so that files may be processed later by any program. If the program reading the data expects a different schema this can be easily resolved, since both schemas are present.

When Avro is used in RPC, the client and server exchange schemas in the connection handshake. (This can be optimized so that, for most calls, no schemas are actually transmitted.) Since both client and server both have the other's full schema, correspondence between same named fields, missing fields, extra fields, etc. can all be easily resolved.

Avro schemas are defined with JSON . This facilitates implementation in languages that already have JSON libraries.

Schema example

```
 {
   "namespace": "example.avro",
   "type": "record",
   "name": "User",
   "fields": [
      {"name": "name", "type": "string"},
      {"name": "favorite_number",  "type": ["int", "null"]},
      {"name": "favorite_color", "type": ["string", "null"]}
   ] 
 }
```

**Avro IDL**

In addition to supporting JSON for type and protocol definitions, Avro includes experimental support for an alternative interface description language (IDL) syntax known as Avro IDL. Previously known as GenAvro, this format is designed to ease adoption by users familiar with more traditional IDLs and programming languages, with a syntax similar to C/C++, Protocol Buffers and others.

## Schema evolution

Avro schemas can be written in two ways, either in a JSON format:
```
{
    "type": "record",
    "name": "Person",
    "fields": [
        {"name": "userName",        "type": "string"},
        {"name": "favouriteNumber", "type": ["null", "long"]},
        {"name": "interests",       "type": {"type": "array", "items": "string"}}
    ]
}
```
…or in an IDL:

```
record Person {
    string               userName;
    union { null, long } favouriteNumber;
    array<string>        interests;
}
```

Notice that there are no tag numbers in the schema! 

Here is the same example data encoded in just 32 bytes:

![avro](https://github.com/rgederin/data-formats/blob/master/img/avro.png)

Strings are just a length prefix followed by UTF-8 bytes, but there’s nothing in the bytestream that tells you that it is a string. It could just as well be a variable-length integer, or something else entirely. The only way you can parse this binary data is by reading it alongside the schema, and the schema tells you what type to expect next. You need to have the exact same version of the schema as the writer of the data used. If you have the wrong schema, the parser will not be able to make head or tail of the binary data.

So how does Avro support schema evolution? Well, although you need to know the exact schema with which the data was written (the writer’s schema), that doesn’t have to be the same as the schema the consumer is expecting (the reader’s schema). You can actually give two different schemas to the Avro parser, and it uses resolution rules to translate data from the writer schema into the reader schema.

This has some interesting consequences for schema evolution:

* The Avro encoding doesn’t have an indicator to say which field is next; it just encodes one field after another, in the order they appear in the schema. Since there is no way for the parser to know that a field has been skipped, there is no such thing as an optional field in Avro. Instead, if you want to be able to leave out a value, you can use a union type, like union { null, long } above. This is encoded as a byte to tell the parser which of the possible union types to use, followed by the value itself. By making a union with the null type (which is simply encoded as zero bytes) you can make a field optional.
* Union types are powerful, but you must take care when changing them. If you want to add a type to a union, you first need to update all readers with the new schema, so that they know what to expect. Only once all readers are updated, the writers may start putting this new type in the records they generate.
* You can reorder fields in a record however you like. Although the fields are encoded in the order they are declared, the parser matches fields in the reader and writer schema by name, which is why no tag numbers are needed in Avro.
* Because fields are matched by name, changing the name of a field is tricky. You need to first update all readers of the data to use the new field name, while keeping the old name as an alias (since the name matching uses aliases from the reader’s schema). Then you can update the writer’s schema to use the new field name.
* You can add a field to a record, provided that you also give it a default value (e.g. null if the field’s type is a union with null). The default is necessary so that when a reader using the new schema parses a record written with the old schema (and hence lacking the field), it can fill in the default instead.
* Conversely, you can remove a field from a record, provided that it previously had a default value. (This is a good reason to give all your fields default values if possible.) This is so that when a reader using the old schema parses a record written with the new schema, it can fall back to the default.

This leaves us with the problem of knowing the exact schema with which a given record was written. The best solution depends on the context in which your data is being used:

* In Hadoop you typically have large files containing millions of records, all encoded with the same schema. Object container files handle this case: they just include the schema once at the beginning of the file, and the rest of the file can be decoded with that schema.
* In an RPC context, it’s probably too much overhead to send the schema with every request and response. But if your RPC framework uses long-lived connections, it can negotiate the schema once at the start of the connection, and amortize that overhead over many requests.
* If you’re storing records in a database one-by-one, you may end up with different schema versions written at different times, and so you have to annotate each record with its schema version. If storing the schema itself is too much overhead, you can use a hash of the schema, or a sequential schema version number. You then need a schema registry where you can look up the exact schema definition for a given version number.

One way of looking at it: in Protocol Buffers, every field in a record is tagged, whereas in Avro, the entire record, file or network connection is tagged with a schema version.

At first glance it may seem that Avro’s approach suffers from greater complexity, because you need to go to the additional effort of distributing schemas. However, I am beginning to think that Avro’s approach also has some distinct advantages:

* Object container files are wonderfully self-describing: the writer schema embedded in the file contains all the field names and types, and even documentation strings (if the author of the schema bothered to write some). This means you can load these files directly into interactive tools like Pig, and it Just Works™ without any configuration.
* As Avro schemas are JSON, you can add your own metadata to them, e.g. describing application-level semantics for a field. And as you distribute schemas, that metadata automatically gets distributed too.
* A schema registry is probably a good thing in any case, serving as documentation and helping you to find and reuse data. And because you simply can’t parse Avro data without the schema, the schema registry is guaranteed to be up-to-date. Of course you can set up a protobuf schema registry too, but since it’s not required for operation, it’ll end up being on a best-effort basis.

## Comparison with other systems

Avro provides functionality similar to systems such as Thrift, Protocol Buffers, etc. Avro differs from these systems in the following fundamental aspects.

* **Dynamic typing**: Avro does not require that code be generated. Data is always accompanied by a schema that permits full processing of that data without code generation, static datatypes, etc. This facilitates construction of generic data-processing systems and languages.

* **Untagged data**: Since the schema is present when data is read, considerably less type information need be encoded with data, resulting in smaller serialization size.

* **No manually-assigned field IDs**: When a schema changes, both the old and new schema are always present when processing data, so differences may be resolved symbolically, using field names.

## Parquet

Apache Parquet is a free and open-source column-oriented data store of the Apache Hadoop ecosystem. It is similar to the other columnar storage file formats available in Hadoop namely RCFile and Optimized RCFile. It is compatible with most of the data processing frameworks in the Hadoop environment. It provides efficient data compression and encoding schemes with enhanced performance to handle complex data in bulk.

The open-source project to build Apache Parquet began as a joint effort between Twitter and Cloudera. The first version—Apache Parquet 1.0—was released in July 2013. Since April 27, 2015, Apache Parquet is a top-level Apache Software Foundation (ASF)-sponsored project

Apache Parquet is implemented using the record-shredding and assembly algorithm, which accommodates the complex data structures that can be used to store the data. The values in each column are physically stored in contiguous memory locations and this columnar storage provides the following benefits:

* Column-wise compression is efficient and saves storage space
* Compression techniques specific to a type can be applied as the column values tend to be of the same type
* Queries that fetch specific column values need not read the entire row data thus improving performance
* Different encoding techniques can be applied to different columns

![p1](https://github.com/rgederin/data-formats/blob/master/img/p1.png)

![p2](https://github.com/rgederin/data-formats/blob/master/img/p2.png)

![p3](https://github.com/rgederin/data-formats/blob/master/img/p3.png)