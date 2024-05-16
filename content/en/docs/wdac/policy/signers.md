---
weight: 20
title: Signers
categories: [Examples]
tags: [test, sample, docs]
description: >
  **Signers:** This element encompasses all signing certificates to be utilized in the rules outlined in the _SigningScenarios_ segment.
---

This element encompasses all signing certificates to be utilized in the rules outlined in the _SigningScenarios_ segment. Each signer entry mandates a CertRoot property, wherein the Value attribute denotes the hash of the cbData blob of the certificate. The hashing algorithm employed aligns with the algorithm specified in the certificate. This hash functions as a distinctive identifier for the certificate.



> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<xs:element name="Signers">
    <xs:annotation>
        <xs:documentation> Collection of signers. </xs:documentation>
    </xs:annotation>
    <xs:complexType>
        <xs:choice minOccurs="1" maxOccurs="10000000">
          <xs:element ref="Signer" minOccurs="0" maxOccurs="10000000" />
        </xs:choice>
    </xs:complexType>
</xs:element>
```
**Code Block 1:** _SiPolicy_ elements and attributes
> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<xs:element name="Signer">
  <xs:annotation>
    <xs:documentation> A Signer </xs:documentation>
  </xs:annotation>
  <xs:complexType>
    <xs:sequence minOccurs="1" maxOccurs="1">
      <xs:element ref="CertRoot" minOccurs="1" maxOccurs="1" />
      <xs:element ref="CertEKU" minOccurs="0" maxOccurs="255" />
      <xs:element ref="CertIssuer" minOccurs="0" maxOccurs="1" />
      <xs:element ref="CertPublisher" minOccurs="0" maxOccurs="1" />
      <xs:element ref="CertOemID" minOccurs="0" maxOccurs="1" />
        <xs:element ref="FileAttribRef" minOccurs="0" maxOccurs="10000000" />
    </xs:sequence>
    <xs:attribute name="Name" type="SignerNameType" use="required" />
    <xs:attribute name="ID" type="SignerIdType" use="required" />
    <xs:attribute name="SignTimeAfter" type="SignTimeType" use="optional" />
  </xs:complexType>
</xs:element>
```
**Code Block 2:** _SiPolicy_ elements and attributes
> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<xs:element name="CertRoot">
  <xs:complexType>
    <xs:attribute name="Type" type="CertEnumType" use="required" />
    <!-- Value is either wellknow Root ID or TBS hash, both in hexBinary form-->
    <xs:attribute name="Value" type="xs:hexBinary" use="required" />
  </xs:complexType>
</xs:element>
```
**Code Block 3:** _SiPolicy_ elements and attributes
> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<xs:simpleType name="CertEnumType">
  <xs:restriction base="xs:string">
    <xs:enumeration value="TBS" />
    <xs:enumeration value="Wellknown" />
  </xs:restriction>
</xs:simpleType>
```
**Code Block 4:** _SiPolicy_ elements and attributes
> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<xs:element name="CertEKU">
  <xs:complexType>
    <xs:attribute name="ID" type="EKUType" use="required" />
  </xs:complexType>
</xs:element>
```
**Code Block 5:** _SiPolicy_ elements and attributes
> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<xs:simpleType name="EKUType">
  <xs:annotation>
      <xs:documentation> EKU ID type starts with ID_EKU_ </xs:documentation>
  </xs:annotation>
  <xs:restriction base="xs:string">
      <xs:pattern value="ID_EKU_[A-Z][_A-Z0-9]*" />
      <xs:minLength value="1" />
  </xs:restriction>
</xs:simpleType>
```
**Code Block 6:** _SiPolicy_ elements and attributes
> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<xs:element name="CertIssuer">
  <xs:complexType>
    <xs:attribute name="Value" type="xs:string" use="required" />
  </xs:complexType>
</xs:element>
```
**Code Block 7:** _SiPolicy_ elements and attributes
> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<xs:element name="CertPublisher">
  <xs:complexType>
    <xs:attribute name="Value" type="xs:string" use="required" />
  </xs:complexType>
</xs:element>
```
**Code Block 8:** _SiPolicy_ elements and attributes
> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<xs:element name="CertOemID">
  <xs:complexType>
    <xs:attribute name="Value" type="xs:string" use="required" />
  </xs:complexType>
</xs:element>
```
**Code Block 9:** _SiPolicy_ elements and attributes
> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<xs:element name="FileAttribRef">
  <xs:annotation>
    <xs:documentation> A FileAttribRef is used to reference a FILE_ATTRIB rule through ID </xs:documentation>
  </xs:annotation>
  <xs:complexType>
    <xs:attribute name="RuleID" type="FileAttribType" use="required" />
  </xs:complexType>
</xs:element>
```
**Code Block 10:** _SiPolicy_ elements and attributes
```xsd
<xs:simpleType name="FileAttribType">
  <xs:annotation>
    <xs:documentation> Generic file rule ID should start with ID_FILEATTRIB_ or ID_FILE_ </xs:documentation>
  </xs:annotation>
  <xs:restriction base="xs:string">
    <xs:pattern value="((ID_FILEATTRIB_[A-Z][_A-Z0-9]*))|((ID_FILE_[A-Z][_A-Z0-9]*))" />
    <xs:minLength value="10" />
  </xs:restriction>
</xs:simpleType>
```
**Code Block 11:** _SiPolicy_ elements and attributes



```cpp
struct _KGATE
{
  _DISPATCHER_HEADER Header;
  ULONG ProviderId;
  GUID  ProviderGuid;
};
```


> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

## First Header

This is a normal paragraph following a header. Bacon ipsum dolor sit amet t-bone doner shank drumstick, pork belly porchetta chuck sausage brisket ham hock rump pig. Chuck kielbasa leberkas, pork bresaola ham hock filet mignon cow shoulder short ribs biltong.  Bacon ipsum dolor sit amet t-bone doner shank drumstick, pork belly porchetta chuck sausage brisket ham hock rump pig. Chuck kielbasa leberkas, pork bresaola ham hock filet mignon cow shoulder short ribs biltong.  Bacon ipsum dolor sit amet t-bone doner shank drumstick, pork belly porchetta chuck sausage brisket ham hock rump pig. Chuck kielbasa leberkas, pork bresaola ham hock filet mignon cow shoulder short ribs biltong.