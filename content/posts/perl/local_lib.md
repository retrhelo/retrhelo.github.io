---
title: "Use local modules in Perl5"
date: 2022-06-30T12:03:24+08:00
tags: ["perl"]
draft: false
---

In this article, I explain how to write Perl scripts with multiple files.

<!--more-->

## Background

It's wise to seperate programs into several modules. It makes the project easier
to maintain, and brings extra reusability. When writing Perl scripts, I come
with the idea if I can extra some commonly-used functions as libs, for further
use.

## How-to

It's very strange that Perl doesn't come with native solution for including
other local files like _C_. Instead, it has to bring up 3rd party modules and
some tricks for this purpose.

The trick I'm going to play relies on [FindBin][1], a module that would "locate
directory of original perl script". It provides the ability to locate where
the script that's executed is. As the script's path is available, it's easy to
locate the lib files, which are generally located by relative path.

[1]: https://metacpan.org/pod/FindBin

Let's simulate a simple situation. There're two files, `main.pl` and
`lib/Utils.pm`. For `main.pl` to use functions from `lib/Utils.pm`, the source
code should be:

```Perl
# In main.pl

use FindBin;
use lib "$FindBin::Bin/lib";
use Utils;

function_from_utils();
```
