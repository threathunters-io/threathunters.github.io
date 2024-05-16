---
draft: true
weight: 20
title: Macros
categories: [Examples]
tags: [test, sample, docs]
description: >
  **Macros:** This element consists of a list of Extended Key Usages (EKUs) that can be applied to signers.
---

This element consists of a list of Extended Key Usages (EKUs) that can be applied to signers. When incorporated into a signer rule, the EKU specified in the certificate must match that in the certificate used for signing. Each EKU instance possesses a "Value" attribute, comprising an encoded OID. For instance, to mandate WHQL signing, the "Windows Hardware Driver Verification" EKU (1.3.6.1.4.1.311.10.3.5) must be assigned to those drivers. The encoded "Value" attribute would be "010A2B0601040182370A0305" (where the first byte, typically 0x06 for absolute OID, is replaced with 0x01). The process of OID encoding is detailed here. ConvertTo-CIPolicy decodes and resolves the original FriendlyName attribute for encoded OID values.


> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<xs:element name="Macros">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="Macro" minOccurs="1" maxOccurs="unbounded">
        <xs:annotation>
          <xs:documentation> A Macro element defines a text substitution macro that
              can be used in other elements. Macros are referenced using NMAKE syntax,
              i.e. $(runtime.windows). </xs:documentation>
        </xs:annotation>
        <xs:complexType>
          <xs:attribute name="Id" type="MacroIdType" use="required">
            <xs:annotation>
              <xs:documentation> Required. The Id for this macro, used in macro
                  references. For example, if the Id for this macro is
                  "runtime.windows", the macro would be referenced as
                  $(runtime.windows). </xs:documentation>
            </xs:annotation>
          </xs:attribute>
          <xs:attribute name="Value" type="MacroValueStringType" use="required">
            <xs:annotation>
              <xs:documentation> Required. The value that will be substituted for
                  macro references in macro- enabled XML attributes. </xs:documentation>
            </xs:annotation>
          </xs:attribute>
        </xs:complexType>
      </xs:element>
    </xs:sequence>
  </xs:complexType>
  <xs:unique name="UniqueMacroId">
    <xs:selector xpath="ps:Macro" />
    <xs:field xpath="@Id" />
  </xs:unique>
</xs:element>
```

> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<!-- Macro Type -->
<xs:simpleType name="MacroIdType">
  <xs:restriction base="xs:string">
    <xs:pattern value="[a-zA-Z_][a-zA-Z_0-9.]*" />
  </xs:restriction>
</xs:simpleType>
```
> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<!-- Macro Value String Type -->
<xs:simpleType name="MacroValueStringType">
  <xs:restriction base="xs:string">
    <xs:pattern value="[a-zA-Z0-9\-_!@#%\^\.,;:=\+~`'\{\}\(\)\[\]\$ \\]*" />
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