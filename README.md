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

