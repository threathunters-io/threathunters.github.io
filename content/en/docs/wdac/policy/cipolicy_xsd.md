---
weight: 10
title: General Layout
categories: [Examples]
tags: [test, sample, docs]
description: >
  **General Policy Layout:** A policy relevant to WDAC (Windows Defender Application Control) is defined within an XML file. To understand the policy considered by WDAC, you can examine the policy XML schema stored in the _cipolicy.xsd_ file at _C:\Windows\schemas\CodeIntegrity\\_. 
---

A policy relevant to WDAC (Windows Defender Application Control) is defined within an XML file. To understand the policy considered by WDAC, you can examine the policy XML schema stored in the _cipolicy.xsd_ file at _C:\Windows\schemas\CodeIntegrity\\_. This schema, describes the structure, constraints, and data types of a WDAC XML policy files. It defines the elements and attributes that can appear in an policy file, their order, relationships, and any restrictions or rules that apply to them.

> The following provides an overview of the basic elements and attributes within the _SiPolicy_ element. In each policy file, _SiPolicy_ serves as the root element, signifying its role as the highest-level element that encapsulates all other elements and attributes within the file.

```xsd
<xs:element name="SiPolicy">
  <xs:complexType>
    <xs:all>
      <xs:element name="VersionEx" type="VersionExType" minOccurs="1" maxOccurs="1" />
      <xs:element name="PolicyTypeID" type="GuidType" minOccurs="0" maxOccurs="1" />
      <xs:element name="PlatformID" type="GuidType" minOccurs="1" maxOccurs="1" />
      <xs:element name="PolicyID" type="GuidType" minOccurs="0" maxOccurs="1" />
      <xs:element name="BasePolicyID" type="GuidType" minOccurs="0" maxOccurs="1" />
      <xs:element name="Rules">
        <xs:complexType>
          <xs:sequence>
            <xs:element name="Rule" type="RuleType" minOccurs="0" maxOccurs="65535" />
          </xs:sequence>
        </xs:complexType>
      </xs:element>
      <xs:element ref="EKUs" minOccurs="0" maxOccurs="1" />
      <xs:element ref="FileRules" minOccurs="0" maxOccurs="1" />
      <xs:element ref="Signers" minOccurs="0" maxOccurs="1" />
      <xs:element ref="SigningScenarios" minOccurs="0" maxOccurs="1" />
      <xs:element ref="UpdatePolicySigners" minOccurs="0" maxOccurs="1" />
      <xs:element ref="CiSigners" minOccurs="0" maxOccurs="1" />
      <xs:element name="HvciOptions" type="DWordType" minOccurs="0" maxOccurs="1" />
      <xs:element ref="Settings" minOccurs="0" maxOccurs="1" />
      <xs:element ref="Macros" minOccurs="0" maxOccurs="1" />
      <xs:element ref="SupplementalPolicySigners" minOccurs="0" maxOccurs="1" />
    </xs:all>
    <xs:attribute name="FriendlyName" type="xs:string" use="optional" />
    <xs:attribute name="PolicyType" type="PolicyType" use="optional" />
  </xs:complexType>
</xs:element>
```
**Code Block 1:** _SiPolicy_ elements and attributes
> Table 1 presents the names of the elements and attributes within _SiPolicy_ as well as a brief description of their core functionalities.

| Name    | Description |
| :--------: | :---------------: |
| VersionEx   | ...               |
| PolicyTypeID   | ...               |
| PlatformID   | ...               |
| PolicyID   | ...               |
| BasePolicyID   | ...               |
| Rules   | ...               |
| EKUs | This element consists of a list of Extended Key Usages (EKUs) that can be applied to signers. When incorporated into a signer rule, the EKU specified in the certificate must match that in the certificate used for signing. Each EKU instance possesses a "Value" attribute, comprising an encoded OID. For instance, to mandate WHQL signing, the "Windows Hardware Driver Verification" EKU (1.3.6.1.4.1.311.10.3.5) must be assigned to those drivers. The encoded "Value" attribute would be "010A2B0601040182370A0305" (where the first byte, typically 0x06 for absolute OID, is replaced with 0x01). The process of OID encoding is detailed here. ConvertTo-CIPolicy decodes and resolves the original FriendlyName attribute for encoded OID values. |
| FileRules   | ...               |
| Signers | This element encompasses all signing certificates to be utilized in the rules outlined in the _SigningScenarios_ segment. Each signer entry mandates a CertRoot property, wherein the Value attribute denotes the hash of the cbData blob of the certificate. The hashing algorithm employed aligns with the algorithm specified in the certificate. This hash functions as a distinctive identifier for the certificate. |
| SigningScenarios   | ...               |
| UpdatePolicySigners   | ...               |
| HvciOptions   | ...               |
| Settings   | ...               |
| Macros   | ...               |
| SupplementalPolicySigners   | ...               |
| ...   | ...               |
| ...   | ...               |
{.table-bordered}
**Table 1:** Names of the elements and attributes and brief description
