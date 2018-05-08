# Table of Contents

- [XML](#xml)
    * [XML Key terminology](#xml-key-terminology)
    * [Well-formed XML](#well-formed-xml)
    * [XML DTD](#xml-dtd)
    * [XSD](#xsd)
    * [SAX](#sax)
    * [DOM](#dom)
    * [StAX](#stax)
    
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