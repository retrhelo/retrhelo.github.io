---
title: "Better subroutine caller tracking with Perl"
date: 2022-08-23T08:51:00+08:00
tags: ["perl"]
draft: false
---

In this article, I note down a very useful approach in Perl, to better track and
locate the caller of certain subroutine.

<!--more-->

## `caller` function

Like the C language, Perl provides things like `__LINE__` and `__FILE__` to
indicate the current program line and file, speeding finding the correct place
where an error may exist.

However, these macros are limited when searching for the exact caller of the
function, since they record only the very line (and file) where the macros lie.

Luckily, in Perl language, it provides a solution to it. The `caller` function
in Perl provides exactly what I want. The calling convention of `caller` can be

```perl
my ($package, $filename, $line) = caller;
```

To understand how `caller` works, an example is provided below.

```perl
# test.pl
sub foo {
  my ($package, $filename, $line) = caller;

  print "$package: $filename: $line\n";
}

foo;  # at line 8

# Do some other work

foo;  # at line 12
```

And here is the output.

```
main: test.pl: 8
main: test.pl: 12
```

## Acknowledgements

This article is written based on [this document][1]. Please refer to it for a
deeper introduction about `caller`.

[1]: https://perldoc.perl.org/functions/caller
