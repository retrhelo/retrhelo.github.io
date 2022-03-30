---
title: "Status Message in M5"
date: 2022-03-30T10:54:06+08:00
tags: ["gem5", "non-technical"]
---

There's a very interesting part of the gem5 _Coding Style_ [document][1],
talking about different message status from M5. I consider them very useful for
further work.

<!--more-->

## M5 Message Status

By their purpose, M5 message functions are divided into two parts: the **error
functions** and the **alert functions**.

### Error functions

There are two error functions defined in `src/base/misc.hh`, `panic()` and
`fatal()`. They have roughly similar behaviors but differ in purposes and use
case. Here's the description given by the gem5 document.

- `panic()` should be called when something happens that should never ever
  happen regardless of what the user does (i.e., an actual m5 bug).
- `fatal()` should be called when the simulation cannot continue due to some
  condition that is the user's fault (bad configuration, invalid arguments,
  etc.) and not a simulator bug.

### Alert functions

gem5 provides 3 functions as choices to alert its user: `inform()`, `warn()`
and `hack()`.

- `inform()` should be called for informative messages that users should know,
  but not worry about.
- `warn()` should be called when some functionality isn't necessarily
  implemented correctly, but it might work well enough. The idea behind a
- `hack()` should be called when some functionality isn't implemented nearly as
  well as it could or should be but for expediency or history sake hasn't been
  fixed.

## Acknoledgements

Most of the contents are from gem5's _Coding Style_ part documentation. Please
refer to this [site][1] for more detailed informations.

[1]: https://www.gem5.org/documentation/general_docs/development/coding_style/
