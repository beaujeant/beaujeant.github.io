---
permalink: /appsec101/memory/
title: Memory
parent: Application Security 101
nav_order: 3
---

[<< Central Processing Unit](https://beaujeant.github.io/appsec101/cpu/){: .btn .btn-outline }
[Lab setup >>](https://beaujeant.github.io/appsec101/lab/){: .btn .btn-outline }

Memory
======

Memory is a core component of a computer system. Without memory, there is no place to store the instructions read and executed by the CPU and there is no place to store the arguments and output of a operations. A CPU without memory would be thus useless. But what is _memory_? Before going further, it is important to differentiate __storage__ and __memory__. While those two terms are interchangeable in some literature, this course will differentiate them.

* __Storage__ is the component of your computer that allows you to store and access data on a _long-term_ basis [[1](https://www.kingston.com/en/community/articledetail/articleid/29685)]. The most known storage media are SSD, hard drive or USB flash drive. The main difference with memory components is that storage system retains the data, even when not powered. This means when the computer switches off, memory clears it content while storage data remains intact.
* __Memory__ is the component of your computer that allows you to access and store data meant for a _short-term_ use. The mains advantage of memory is the fast access (read and write) compare to storage media: between 200-550MB/s for SSD storage [[2](https://www.storagereview.com/ssd_vs_hdd)] and between 17-25.6GB/s for DDR4 memory[[3](https://www.transcend-info.com/Support/FAQ-292)].

While the storage contains the data at rest, the memory contains the data currently in used, including the instructions of all applications running and the data associated.


ELF/PE file
-----------

Programs are packed in a specific format so that whenever you run it (double-click on the icon or execute in a terminal), the operating system knows how to load it in memory and where to start (first instruction). The two most known application formats are __ELF__ (Executable and Linkable Format) for Linux application and __PE__ (Portable Executable) for Windows application.

ELF and PE formats are different but still share common main features. The loading process of an application is quite similar from Windows to Linux, therefore, this course will stay generic and give an overview of the process in general.

Once you run an application, the operating system will first allocated some space in memory to load the application. It is important to understand the concept of virtual memory............

The executable (ELF or PE file) contains (among others) the following part:

* __Mapping__:
* __Entry Point__: The address where the start once the application loaded in memory
*


General structure
-----------------

Growing memory. When run, it map the 2Go.
Endian and little endian.


Stack
-----

Static/hard coded data
----------------------

Heap
----

Instruction
-----------

Imported libraries
------------------

Kernel and userland
-------------------

Registers
---------

* [[1](https://www.kingston.com/en/community/articledetail/articleid/29685)] https://www.kingston.com/en/community/articledetail/articleid/29685
* [[2](https://www.storagereview.com/ssd_vs_hdd)] https://www.storagereview.com/ssd_vs_hdd
* [[3](https://www.transcend-info.com/Support/FAQ-292)] https://www.transcend-info.com/Support/FAQ-292

[<< Central Processing Unit](https://beaujeant.github.io/appsec101/cpu/){: .btn .btn-outline }
[Lab setup >>](https://beaujeant.github.io/appsec101/lab/){: .btn .btn-outline }
