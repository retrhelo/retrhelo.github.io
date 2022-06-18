---
title: "It's Time for OS to Rediscover Hardware"
date: 2022-06-18T16:54:14+0800
tags: []
draft: false
---

This is a casual notes from the a lecture given by Timothy Roscoe, from ETH
Zurich. The recording video of the lecture can be found [here][1].

[1]: https://www.usenix.org/conference/osdi21/presentation/fri-keynote

<!--more-->

# It's Time for Operating Systems to Rediscover Hardware

Timothy Roscoe from ETH Zurich

## What is OS?

That body of software that

- Multiplexes machine's hardware
- Abstraction of hardware
- Protect software using the hardware

## Modern machine

- ccNUMA machines is not what modern machine looks like

Cache-coherent can't be guaranteed.
Different cache decoding room
Modern computer is far away complicated, especially about SoC.

- Linux is not all about an Operating System that works on a real machine, but
just a small part of it.

## Is it really a problem?

- For security concerns: a "cross SoC attack"

Linux cannot control all hardware on the SoC, leaving large space for potential
attack.

- Power management

- Linux is hiding more and more hardware features from the kernel, since it
cannot control it

## Current new work about OS kernel

It's Linux, it looks like Linux, or making the same assumption of hardware with
Linux...

## Some opinions

- Ignorance: too many people don't know what modern hardware looks like.
- Denial: too many people find it more comfortable to focus on Linux

- Interest from industry

Despite HarmonyOS, fushica and other operating systems are showing increasing
interest from the Industry.

- Hardware is designed to sandbox Linux in a corner!

## Suggestions

- Understand your hardware
- It's hard for hardware engineers to communicate with OS developers

- Build our own computers

It's easier to get hardware built, with the support from modern hardware
platform.

## Final Summary

- OS research is really needed right now

Hardware is different in scary ways.
Companies are writing new kernels

- An amazing opportunity

We can rethink OS structures for new h/w

## Questions

- The barrier to get into the hardware? Do you have any advise on entry barrier?

Figuring the things you need for the first time.
It's hard, work harder then.
Collaboration with hardware.

- Why should there be one OS controlling all hardware? Since they can be treated
as co-workers.

An interesting open problem, but current OS needs some changes.

- The right solution for OS research's dilemma

A new dedicated operating system, security. There should be more people to work
on this.

A cultural change for operating system community.

The misunderstanding on hardware matters.
