---
date: 2021-12-30
draft: false
title: TTPHash - A possible approach for event fingerprinting
categories: [posts]
tags: [post, blog]

---
The lack of a generally accepted method for calculating a telemetry event fingerprint to uniquely identify a specific event represents a gap in current telemetry collection practice. At present, the approach involves utilizing file hashes as a means to fingerprint observable entities, such as files. While file hashes serve as invaluable markers from the perspective of Indicators of Compromise (IOCs), their utility is restricted by their inability to furnish contextual insights regarding the event in question. This deficiency presents significant challenges to detection engineers tasked with detecting and mitigating security threats. Without access to comprehensive contextual data, these professionals have greater difficulty accurately interpreting and responding to potential security breaches.

### TTPHash
While working on QLOG we developed the idea to create a fingerprint method for entire OS security events providing a more reliable and a less false positive prone way to do detection engineering in large scale networks. We decided to implement a PoC for TTPHash in QLOG for process create events to have a working piece of software to experiment with.

### Anatomy of a TTPHash
Technically TTPHash is a SHA 256 Hash generated out of stable parts _stable parts_ of a logged telemetry event on Windows systems. It aims to fingerprint an entire security event including contextual information rather than just fingerprinting PE files or binaries based on their hash or whatever. Generally TTPHash is OS version agnostic, to make sure we get _always the same TTPHash for the same event on different windows systems_, despite of different OS versions. Here is an example PROCESS CREATE Event captured by QLOG:

```json
{
  "EventGuid": "630699d6-ec7b-4e93-8ff8-ebd34a4c1646",
  "StartTime": "2021-12-30T09:13:38.0867746+01:00",
  "QEventID": 100,
  "QType": "Process Create",
  "Username": "TESTPC\\TestUser",
  "UsernameSID": "S-1-5-21-2182592019-244663482-1304351122-1001",
  "Imagefilename": "NOTEPAD.EXE",
  "KernelImagefilename": "NOTEPAD.EXE",
  "OriginalFilename": "NOTEPAD.EXE",
  "Fullpath": "C:\\Program Files\\WindowsApps\\Microsoft.WindowsNotepad_10.2103.6.0_x64__8wekyb3d8bbwe\\Notepad\\Notepad.exe",
  "PID": 11272,
  "Commandline": "\"C:\\Program Files\\WindowsApps\\Microsoft.WindowsNotepad_10.2103.6.0_x64__8wekyb3d8bbwe\\Notepad\\Notepad.exe\" ",
  "Modulecount": 53,
  "TTPHash": "9A0227AC826AA2D8CBA9FCB92AD1465D2ACF8ECE1EF004EA4F36BE6939F253E5",
  "Imphash": "CA88689695B1E2B1E2A85FC73742E524",
  "sha256": "DEB5B0067E4AF84BB07FA3D84631CEE94787FB3BE7CDA11C0016992F54AB4DDA",
  "md5": "62F12AC2CE2C3E90C7D99809FF90AADA",
  "sha1": "48C2E57A9B5BB4D7277A70DD6F0BEFBDF5B23B6C",
  "ProcessIntegrityLevel": "Medium",
  "isOndisk": true,
  "isRunning": true,
  "Signed": "Not signed",
  "AuthenticodeHash": "",
  "Signatures": [],
  "isParent": false,
  "ParentProcess": {
    "EventGuid": null,
    "StartTime": "2021-12-29T12:22:13.6175162+01:00",
    "QEventID": 100,
    "QType": "Process Create",
    "Username": "TESTPC\\TestUser",
    "UsernameSID": "S-1-5-21-2182592019-244663482-1304351122-1001",
    "Imagefilename": "",
    "KernelImagefilename": "",
    "OriginalFilename": "EXPLORER.EXE",
    "Fullpath": "C:\\WINDOWS\\Explorer.EXE",
    "PID": 7008,
    "Commandline": "C:\\WINDOWS\\Explorer.EXE ",
    "Modulecount": 356,
    "TTPHash": "",
    "Imphash": "0DA09DC956F7D38FFB8EE50CCBA217BF",
    "sha256": "4C1D9F1CB41545DD46430130FC3F0E15665BD1948942B0B85410305DAA7AAA9C",
    "md5": "3F786F7D200D0530757B91C5C80BC049",
    "sha1": "FF02A95A759C1880FC580158062D43FDF3B85739",
    "ProcessIntegrityLevel": "Medium",
    "isOndisk": true,
    "isRunning": true,
    "Signed": "Signature valid",
    "AuthenticodeHash": "CD0E17F28F486759625C30A95E78BBC4BC0A3B697AF83B3E1C09267CAEEA51B3",
    "Signatures": [
      {
        "Subject": "CN=Microsoft Windows, O=Microsoft Corporation, L=Redmond, S=Washington, C=US",
        "Issuer": "CN=Microsoft Windows Production PCA 2011, O=Microsoft Corporation, L=Redmond, S=Washington, C=US",
        "NotBefore": "15.12.2020 22:29:13",
        "NotAfter": "02.12.2021 22:29:13",
        "DigestAlgorithmName": "SHA256",
        "Thumbprint": "F7C2F2C96A328C13CDA8CDB57B715BDEA2CBD1D9",
        "TimestampSignatures": [
          {
            "Subject": "CN=Microsoft Time-Stamp Service, OU=Thales TSS ESN:462F-E319-3F20, OU=Microsoft Operations Puerto Rico, O=Microsoft Corporation, L=Redmond, S=Washington, C=US",
            "Issuer": "CN=Microsoft Time-Stamp PCA 2010, O=Microsoft Corporation, L=Redmond, S=Washington, C=US",
            "NotBefore": "14.01.2021 20:02:14",
            "NotAfter": "11.04.2022 21:02:14",
            "DigestAlgorithmName": "SHA256",
            "Thumbprint": "A9C92B731AE7D10B21A6EA338E9FA5F8349F34BF",
            "Timestamp": "29.07.2021 11:39:24 +02:00"
          }
        ]
      }
    ],
    "isParent": false,
    "ParentProcess": null
  }
}
```

OK, here is the struct made of _stable parts_ from the event. This struct is the input we will generate the TTPHash from: 

```c++
TTPHash = Sha256({
  "QType": "Process Create",
  "UsernameSID": "S-1-5-21-2182592019-244663482-1304351122-1001",
  "KernelImagefilename": "NOTEPAD.EXE",
  "OriginalFilename": "NOTEPAD.EXE",
  "Fullpath": "C:\\Program Files\\WindowsApps\\Microsoft.WindowsNotepad_10.2103.6.0_x64__8wekyb3d8bbwe\\Notepad\\Notepad.exe",
  "Commandline": "\"C:\\Program Files\\WindowsApps\\Microsoft.WindowsNotepad_10.2103.6.0_x64__8wekyb3d8bbwe\\Notepad\\Notepad.exe\" ",
  "Imphash": "CA88689695B1E2B1E2A85FC73742E524",
  "sha256": "DEB5B0067E4AF84BB07FA3D84631CEE94787FB3BE7CDA11C0016992F54AB4DDA",
  "ProcessIntegrityLevel": "Medium",
  "isOndisk": true,
  "AuthenticodeHash": "",
  "ParentProcess": {
    "UsernameSID": "S-1-5-21-2182592019-244663482-1304351122-1001",
    "Imagefilename": "",
    "KernelImagefilename": "",
    "OriginalFilename": "EXPLORER.EXE",
    "Fullpath": "C:\\WINDOWS\\Explorer.EXE",
    "Commandline": "C:\\WINDOWS\\Explorer.EXE ",
    "Imphash": "0DA09DC956F7D38FFB8EE50CCBA217BF",
    "sha256": "4C1D9F1CB41545DD46430130FC3F0E15665BD1948942B0B85410305DAA7AAA9C",
    "ProcessIntegrityLevel": "Medium",
    "isOndisk": true,
    "isRunning": true,
    "AuthenticodeHash": "CD0E17F28F486759625C30A95E78BBC4BC0A3B697AF83B3E1C09267CAEEA51B3",
  }
})
```

### Benefits using TTPHash
As shown in the example above, TTPHash provides a deterministic fingerprint for an entire event while NOT loosing the contextual information, like parent-child relationship, event type, command line, full path of the binaries involed, authenticode signature, loaded modules and so on.
You can use the TTPHash to:
- deterministic baselining of security events on your network
- hunting for identical events on different or same Windows versions with a single hash>
- less error prone and more secure way whitelisting of well know security events happening on your network
- share TTPHash with the community, instead of sharing non contextual IOCs

### Next steps
We see huge potential in using TTPHash for hunting, particulary on very large scale networks with many thousands of system where baselining of events is crucial. We will continue evaluation of TTPHash for "process create events". TTPHash can also be applied to any other security events like network connections, image loads, file access and many more. Maybe we will also add TTPHash to [Laurel](https://github.com/threathunters-io/laurel) in future. Stay tuned!

**Credits**: I want to thank _@jdu2600_ for inspiring us to call it TTPHash instead of EventHash.

@securityfreax
