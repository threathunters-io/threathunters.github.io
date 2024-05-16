---
draft: true
weight: 20
title: BasePolicyID
categories: [Examples]
tags: [test, sample, docs]
description: >
  **BasePolicyID:** This element consists of a list of Extended Key Usages (EKUs) that can be applied to signers.
---


This element consists of a list of Extended Key Usages (EKUs) that can be applied to signers. When incorporated into a signer rule, the EKU specified in the certificate must match that in the certificate used for signing. Each EKU instance possesses a "Value" attribute, comprising an encoded OID. For instance, to mandate WHQL signing, the "Windows Hardware Driver Verification" EKU (1.3.6.1.4.1.311.10.3.5) must be assigned to those drivers. The encoded "Value" attribute would be "010A2B0601040182370A0305" (where the first byte, typically 0x06 for absolute OID, is replaced with 0x01). The process of OID encoding is detailed here. ConvertTo-CIPolicy decodes and resolves the original FriendlyName attribute for encoded OID values.


> There should be no margin above this first sentence.
>
> Blockquotes should be a lighter gray with a border along the left side in the secondary color.
>
> There should be no margin below this final sentence.

```xsd
<xs:element name="SiPolicy">
  [...]
        <xs:element name="BasePolicyID" type="GuidType" minOccurs="0" maxOccurs="1" />
  [...]
</xs:element>
```

```xsd
 <!-- A {00000000-0000-0000-0000-000000000000} GUID type -->
    <xs:simpleType name="GuidType">
        <xs:restriction base="xs:string">
            <xs:pattern
                value="\{[a-fA-F0-9]{8}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{12}\}" />
        </xs:restriction>
    </xs:simpleType>
```



example
```xml
<EKUs>
  <EKU ID="ID_EKU_A_1" Value="010A2B0601040182374C0801" FriendlyName="1.3.6.1.4.1.311.76.8.1" />
</EKUs>
```

## First Header

This is a normal paragraph following a header. Bacon ipsum dolor sit amet t-bone doner shank drumstick, pork belly porchetta chuck sausage brisket ham hock rump pig. Chuck kielbasa leberkas, pork bresaola ham hock filet mignon cow shoulder short ribs biltong.  Bacon ipsum dolor sit amet t-bone doner shank drumstick, pork belly porchetta chuck sausage brisket ham hock rump pig. Chuck kielbasa leberkas, pork bresaola ham hock filet mignon cow shoulder short ribs biltong.  Bacon ipsum dolor sit amet t-bone doner shank drumstick, pork belly porchetta chuck sausage brisket ham hock rump pig. Chuck kielbasa leberkas, pork bresaola ham hock filet mignon cow shoulder short ribs biltong.