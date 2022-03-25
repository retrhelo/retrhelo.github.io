---
title: "One step into gem5"
date: 2022-03-25T20:10:41+08:00
draft: false
tags: ["gem5"]
---

"The gem5 simulator is a modular platform for computer-system architecture
research, encompassing system-level architecture as well as processor
microarchitecture. gem5 is a *community led*."

Trying to learn such a big system is sometimes difficult. Luckily, gem5 has a
good bunch of documents as tutorials and advanced documents. In this article, I
note down some points that considered markable during my learning.

## How to get gem5 source code?

Unlike many open-source projects, the source code of gem5 is stored remotely on
google source instead of github. Use `git clone` to get the source code.

```shell
git clone https://gem5.googlesource.com/public/gem5
```

## How do we build gem5?

Of course, building something like gem5 requires a lot of softwares as
requirements. However, since the [document][2] gives a detailed list, and most
of the tools should be pre-installed in most of the Linux distributions. I'll
save the time listing.

However, to test if your environment is ready for a building, try this command.
According to the document, the command is "to build and run all the unit tests".

```shell
scons build/NULL/unittests.opt
```

gem5 uses python SConstruct scripts for the building flow. So make sure `scons`
is installed. To build a RISC-V gem5, try:

```shell
scons build/RISCV/gem5.opt
```

Please notice that the building takes a really long time. `-jN` flag can be used
for multi-tasking to save time, just like the Makefile.

## Acknoledgements

Most of the contents in this article are from gem5's [official site][1] and the
[document][2]. Please refer to the document for more detailed information.

[1]: https://www.gem5.org/
[2]: https://www.gem5.org/documentation/
