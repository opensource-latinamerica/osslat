---
title: "eBPF Debugging in Practice: Intermediate Techniques"
authors:
  - oss.lat
date: 2026-02-09
description: "Take your eBPF debugging skills to the next level with BCC tools and more advanced bpftrace scripts."
images:
    - images/blog/ebpf-debugging-intermediate.jpeg
tags:
    - eBPF
    - Debugging
    - Linux
    - Tracing
    - BCC
    - Intermediate
series: ["eBPF Debugging"]
---

Welcome back to our eBPF debugging series! In the first post, we introduced the basics of eBPF and `bpftrace`. Now, it's time to dive deeper and explore the powerful tools offered by the BPF Compiler Collection (BCC), as well as more advanced `bpftrace` techniques.

### Introducing the BPF Compiler Collection (BCC)

BCC is a rich toolkit that provides a set of ready-to-use command-line tools for performance analysis and debugging. These tools are built on top of eBPF and provide a user-friendly interface to complex tracing scenarios.

Most BCC tools follow a similar naming convention, ending with `-bpfcc`. Let's explore some of the most useful ones.

**1. `execsnoop-bpfcc`:**
This tool is perfect for understanding what processes are being executed on your system. It's similar to the `bpftrace` one-liner we saw in the previous post, but it provides more detailed output.

To trace all new processes executed by the `root` user, you can run:
```bash
sudo execsnoop-bpfcc -Uu root -T
```
The `-T` flag adds a timestamp, which is very useful for correlating events.

**2. `opensnoop-bpfcc`:**
If you need to know which files a process is opening, `opensnoop-bpfcc` is the tool for the job. This is incredibly useful for debugging permission issues or understanding an application's file access patterns.

To trace all files opened by a process named `qemu-system-x86`, you can use:
```bash
sudo /usr/sbin/opensnoop-bpfcc --full-path --name qemu-system-x86
```

**3. `tcplife-bpfcc`:**
For network-related debugging, `tcplife-bpfcc` provides a summary of TCP sessions, including the process ID, source and destination addresses, and the duration of the session.

To trace all TCP sessions, simply run:
```bash
sudo tcplife-bpfcc
```

### Advanced `bpftrace` Scripts

While BCC tools are great for common scenarios, sometimes you need to write your own custom logic. `bpftrace` allows you to write more complex scripts to do just that.

**Example: Tracing slow file system operations**

Let's say you want to investigate slow file system operations. You can write a `bpftrace` script to measure the latency of `write()` system calls and only print the ones that take longer than a certain threshold.

Create a file named `slow_writes.bt` with the following content:
```
kprobe:vfs_write
{
    @start[tid] = nsecs;
}

kretprobe:vfs_write /@start[tid]/
{
    $duration = (nsecs - @start[tid]) / 1000; // microseconds
    if ($duration > 100) {
        printf("tid %d: slow write (%d us)\n", tid, $duration);
    }
    delete(@start[tid]);
}
```

You can then run this script with:
```bash
sudo bpftrace slow_writes.bt
```
This script will print a message every time a `write()` operation takes longer than 100 microseconds.

### What's Next?

In this post, we've only scratched the surface of what's possible with BCC and `bpftrace`. We encourage you to explore the other tools available in the `bcc-tools` package and experiment with writing your own `bpftrace` scripts.

In the final post of this series, we'll take it one step further and show you how to write custom eBPF tools in Python using the BCC framework. Stay tuned!
