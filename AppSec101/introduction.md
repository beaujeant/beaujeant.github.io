---
permalink: /appsec101/introduction/
title: Introduction
parent: Application Security 101
nav_order: 1
---

[Central Processing Unit >>](cpu/){: .btn .btn-outline }

Introduction
============

This course will try to explain you the most common vulnerabilities identified in binary applications, i.e. buffer overflow, use-after-free, format string and integer under/overflow.

What will this course teach you?
* Understanding the structure and purpose of a CPU
* Basics of assembly
* Understanding and exploiting basic buffer overflow
* Understanding and exploiting basic use-after-free vulnerability
* Understanding and exploiting basic format string vulnerability
* Understanding and exploiting basic integer overflow and underflow vulnerabilities

What this course won't teach you?
* Reverse engineering
* Coding in C or Assembly (although we will briefly cover both)
* Explain, use and create fuzzers
* Create you own shellcode

Minimum requirements:
* Being familiar with computer
* Being able to read C code (or similar language)

Before we start with the course, we will need to clarify and set the limit of what is a binary application and what are security vulnerabilities in the context of this course.

A __binary application__, also known as __software__, is a collection of instructions and data that tells a computer how to work. Instructions are machine language supported by a an individual processor – typically a __C__​entral __P__​rocessor __U__​nit. Machine language consists of groups of binary values signifying processor instructions that change the state of the computer from its preceding state. For example, an instruction may change the value stored in a particular memory location in the computer. Softwares are usually written in high-level programming languages, such as C, C++ or .NET. They are easier and more efficient for programmers because they are closer to natural languages than machine languages. High-level languages are translated into machine language using a compiler [[1](https://en.wikipedia.org/wiki/Web_application)].

A __security vulnerability__ is a weakness which allows an attacker to reduce a system's information assurance [[2](https://en.wikipedia.org/wiki/Vulnerability_%28computing%29)]. Typically, a vulnerability will allow an attacker to compromise the __C__​onfidentiality and/or the __I__​ntegrity and/or the __A__​vailability of a system, hence the [CIA](https://en.wikipedia.org/wiki/Information_security#Key_concepts) concept in IT security.

For the sake of simplicity, this course will only cover application running on [i386](https://en.wikipedia.org/wiki/Intel_80386) (32-bit architecture). The high-level language used for demonstrations will be __C__ since it is a widely known language that produces binary application relatively easy to understand. As for the [lab](lab/), we will exercise in Linux (Ubuntu) environment.


* [[1](https://en.wikipedia.org/wiki/Web_application)] https://en.wikipedia.org/wiki/Web_application
* [[2](https://en.wikipedia.org/wiki/Vulnerability_%28computing%29)] https://en.wikipedia.org/wiki/Vulnerability_%28computing%29
* [CIA](https://en.wikipedia.org/wiki/Information_security#Key_concepts) https://en.wikipedia.org/wiki/Information_security#Key_concepts
* [i386](https://en.wikipedia.org/wiki/Intel_80386) https://en.wikipedia.org/wiki/Intel_80386


[Central Processing Unit >>](cpu/){: .btn .btn-outline }
