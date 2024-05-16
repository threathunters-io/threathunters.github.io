---
draft: true
weight: 20
title: Rules
categories: [Examples]
tags: [test, sample, docs]
description: >
  **Rules:** This element consists of a list of Extended Key Usages (EKUs) that can be applied to signers.
---

This element consists of a list of Extended Key Usages (EKUs) that can be applied to signers. When incorporated into a signer rule, the EKU specified in the certificate must match that in the certificate used for signing. Each EKU instance possesses a "Value" attribute, comprising an encoded OID. For instance, to mandate WHQL signing, the "Windows Hardware Driver Verification" EKU (1.3.6.1.4.1.311.10.3.5) must be assigned to those drivers. The encoded "Value" attribute would be "010A2B0601040182370A0305" (where the first byte, typically 0x06 for absolute OID, is replaced with 0x01). The process of OID encoding is detailed here. ConvertTo-CIPolicy decodes and resolves the original FriendlyName attribute for encoded OID values.


> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<xs:element name="EKUs">
  <xs:annotation>
      <xs:documentation> Collection of EKUs. </xs:documentation>
  </xs:annotation>
  <xs:complexType>
      <xs:choice minOccurs="1" maxOccurs="255">
          <xs:element ref="EKU" minOccurs="0" maxOccurs="255" />
      </xs:choice>
  </xs:complexType>
</xs:element>
```

> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<xs:element name="EKU">
  <xs:annotation>
      <xs:documentation> Define an EKU </xs:documentation>
  </xs:annotation>
  <xs:complexType>
      <xs:attribute name="ID" type="EKUType" use="required" />
      <xs:attribute name="Value" type="xs:hexBinary" use="required" />
      <xs:attribute name="FriendlyName" type="xs:string" use="optional" />
  </xs:complexType>
</xs:element>
```
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

> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

example
```xml
<EKUs>
  <EKU ID="ID_EKU_A_1" Value="010A2B0601040182374C0801" FriendlyName="1.3.6.1.4.1.311.76.8.1" />
</EKUs>
```









## First Header

This is a normal paragraph following a header. Bacon ipsum dolor sit amet t-bone doner shank drumstick, pork belly porchetta chuck sausage brisket ham hock rump pig. Chuck kielbasa leberkas, pork bresaola ham hock filet mignon cow shoulder short ribs biltong.  Bacon ipsum dolor sit amet t-bone doner shank drumstick, pork belly porchetta chuck sausage brisket ham hock rump pig. Chuck kielbasa leberkas, pork bresaola ham hock filet mignon cow shoulder short ribs biltong.  Bacon ipsum dolor sit amet t-bone doner shank drumstick, pork belly porchetta chuck sausage brisket ham hock rump pig. Chuck kielbasa leberkas, pork bresaola ham hock filet mignon cow shoulder short ribs biltong.