---
title: "Develop a Perl module: Create and Test"
date: 2022-08-06T15:35:35+08:00
tags: ["perl"]
---

Perl module is a common approach for providing custom sets of functions. But how
exactly a module is created, developed, and maintained? In this article, I write
down my experience of developing a tiny module named `Camellia::Glue`.

<!--more-->

## The scafford

There are several' external modules for building your own module. The simplest is
[_Module::Build::Tiny_][1]. It's a simple-yet-powerful tool for building,
testing and distributing your module.

In my case, I use [_Dist::Zilla_][2] for module management. It's a widely used
framework for Perl module development, with detailed tutorials.

[1]: https://metacpan.org/pod/Module::Build::Tiny
[2]: https://metacpan.org/pod/Dist::Zilla

## Create the module

[_Dist::Zilla_][2] comes with its own command: `dzil`, which provides various
subcommand to use. For example, to start a new module, say `Camellia::Glue`, use
the command `dzil new Camellia::Glue`. It creates directory named
`Camellia-Glue` with some default files in it.

```
$ tree Camellia-Glue/
Camellia-Glue/
├── dist.ini
└── lib
    └── Camellia
        └── Glue.pm

2 directories, 2 files
```

Two files exists in the newly-created workspace, `dist.ini` and `Glue.pm`. The
former one stands for the configuration file for `dzil` to read, and the latter
one is very root file of the module.

## Export module's subroutines

Perl uses a CPP-like namespace mechanism to hold all its symbols. To use the
symbols defined in the module, one must get them exported first. Perl provides
[_Exporter_][3] for this job.

[3]: https://perldoc.perl.org/Exporter

For demonstration purpose, I create a `hello` subroutine in
`lib/Camellia/Glue.pm`. The file is shown as follow.

```perl
# ABSTRACT: An abstract is required at the head of the file.

use strict;
use warnings;
package Camellia::Glue;   # Claim the package

require Exporter;
our @ISA = qw(Exporter);
our @EXPORT = qw(hello);  # Export hello so it can be called outside.

sub hello {
  return "hello world";
}

1;    # 1 is required at the end of a module file
```

## Write tests

It's reasonable to write some tests for the module, to test if it's implemented
correctly.

Perl modules follow a fixed file hierarchy. Tests are put in the folder `t/`,
besides `lib/` at the root. All tests files use the suffix `.t`.

Here, I write a simple test to determine if `hello` is exported correctly.

```perl
# t/01_hello.t

use warnings;
use strict;

use Test::Simple tests => 2;
use Camellia::Glue;

ok(0 == ("hello world" cmp hello()));
ok(0 != ("world hello" cmp hello()));
```

`ok()` is a term of assertion used to determine if expression inside meets
programmer's desire.

Please pay attention to the line `use Test::Simple tests => 2`. This line
imports a module `Test::Simple` that's commonly used to write Perl tests. The
term `tests => 2` claims that there are 2 tests (or 2 `ok()`s, to be specific)
to be checked before the test file runs to its end. An error is raised when less
`ok()`s are run.

`dzil` provides the subcommand `dzil test` to run all the tests in `t/` folder.
By doing so, I receive the following result. It says all tests are passed.

```
$ dzil test
[DZ] building distribution under .build/w82pfJhgyz for installation
[DZ] beginning to build Camellia-Glue
[DZ] guessing dist's main_module is lib/Camellia/Glue.pm
[DZ] writing Camellia-Glue in .build/w82pfJhgyz
Checking if your kit is complete...
Looks good
Generating a Unix-style Makefile
Writing Makefile for Camellia::Glue
Writing MYMETA.yml and MYMETA.json
cp lib/Camellia/Glue.pm blib/lib/Camellia/Glue.pm
PERL_DL_NONLAZY=1 "/home/retrhelo/Programming/wheel/perl5/bin/perl" "-MExtUtils::Command::MM" "-MTest::Harness" "-e" "undef *Test::Harness::Switches; test_harness(0, 'blib/lib', 'blib/arch')" t/*.t
t/01_hellotest.t .. ok
All tests successful.
Files=1, Tests=2,  0 wallclock secs ( 0.01 usr  0.00 sys +  0.02 cusr  0.00 csys =  0.03 CPU)
Result: PASS
[DZ] all's well; removing .build/w82pfJhgyz
```
