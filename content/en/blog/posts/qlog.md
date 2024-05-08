---
date: 2021-12-29
draft: false
title: QLOG - ETW logging for process creation events
categories: [posts]
tags: [post, blog]

---
QLOG provides lightweigth userland logging of process create events on Windows written in C#. It's under development and currently in experimental state. QLOG uses ETW to collect telemetry, it doesn’t use API hooks and it doesn’t require a driver to be installed on the target system, Currently QLOG supports “process create” events, but other enriched events may follow soon. QLOG runs as a Windows Services, but can also run in console mode, if you want to output to console directly. I've started QLOG to get a better understanding of Windows ETW and some Windows Eventlog APIs.

QLOG is a research experiment, please don't use it on production systems.

Update July, 2022

We started QLOG as an experiment to learn more about Windows ETW. QLOG provides enriched Logging for process creation events on Windows. We have stopped development of QLOG because in the meantime free tools were published which use ETW for telemetry and do a way better job than QLOG. Curious? Have a look a [Aurora Agent from Nextron]("https://www.nextron-systems.com/aurora/") 

### How does it work

QLOG reads from ETW, enriches events and writes enriched events to Event log channel “QLOG”.

### Development & License

QLOG is being developed by threathunters.io community and will be open sourced once it reaches production grade maturity.

### Why we created QLOG?

Sysmon does a great job, but we wanted to try to create a tool which is open source and doesn't require drivers to be installed on target systems. So we started this experiment.

### Usage & install

Download latest version of [QLOG](https://github.com/threathunters-io/QLOG/releases)
- QLOG requires .NET Framework >= 4.7.2 to be installed.
- To run in interactive console mode, just run _qlog.exe_
- install / deinstall as Windows service, run:
    - install service: _qlog.exe -i_
    - deinstall service: _qlog.exe -u_

Example output of enriched PROCESS CREATE events:
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

