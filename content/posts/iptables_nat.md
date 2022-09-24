---
title: "Setting NAT with iptables"
date: 2022-09-24T16:46:12+08:00
tags: ["network"]
---

## Background

Network Address Translation (NAT) is a common use case in network configuration.
It allows the network manager to translate network address from one into the
other, effectively hiding the original IP address.

<!--more-->

Here is a [document][1] from RedHat giving brief introduction about different
NAT types.

[1]: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/security_guide/sec-configuring_nat_using_nftables

- Masquerading

- Source NAT (SNAT)

- Destination NAT (DNAT)

- Redirect

This article focuses on _Masquerading_ among all these types. It is the very NAT
type to change the source IP address of packets. It's widely used at network
gateways, mapping the source IP address from a certain internal subnet to a
public IP address. Such ability is useful when you don't have enough public IP
addresses for each subnet hosts, or want to hide internal network details for
security reasons.

`iptables` is the classic tool in Linux for packet filtering and NAT. It
provides simple yet powerful means to apply management rules. For a detailed
manual about it, refer to [this site][2].

[2]: https://linux.die.net/man/8/iptables

## Describing the Problem

The problem I try to demonstrate in this article is about setting a gateway for
an internal subnet. There're two hosts in my scenario. They are connected
together via a 100Mbps switch with wired connection, assigned each a static IP
address to form the network.

<div align="center">
<img src="/images/iptables_nat_network_topo.png" width="70%", heights="70%"/>
</div>

In addition, the laptop is also capable of creating wireless connection to the
school's public network. The goal is to make the laptop the gateway of the my
subnet, allowing Internet access for the workstation, while hiding it from the
outside.

## The Solution

### Enable packet forwarding

Packet forwarding is to forward packets from one interface to another. Here in
my case, I should forward all packets from `enp5s0` to `wlp3s0`. Generally, the
Linux kernel is compiled with the functionality to forward packets.

```shell
# Access the system configuration to check if packet forwarding is enabled.
cat /proc/sys/net/ipv4/ip_forward

# The content should be "1" if it's enabled. Otherwise try writing "1" to it in
# order to enable.
echo 1 >/proc/sys/net/ipv4/ip_forward

# In order to make the changes permanent, write following lines into
# /etc/sysctl.conf.
echo "net.ipv4.ip_forward = 1" >>/etc/sysctl.conf
```

### Apply `iptables` rules

The solution can be quite simple with the help of `iptables`. The following
rules should be set.

```shell
# Be noticed that `iptables` must run under `root`

# Forwarding packets from `enp5s0` to `wlp3s0`
iptables -A FORWARD -i enp5s0 -o wlp3s0 -j ACCEPT

# Enabling address masquerade on `wlp3s0`
iptables -t nat -A POSTROUTING -o wlp3s0 -j MASQUERADE

# Save configurations permanently
iptables-save >/etc/sysconfig/iptables
```

These commands should do the work. For a more detailed explanation about the
commands above, please refer to the `iptables` [manual][2].

### `iptables` and `firewalld`

In case you're a Fedora, RedHat, CentOS, etc. user, it becomes a little tricky
to handle the `iptables` rules. Since RedHat is deploying its brand-new
`firewalld` solution for network management, it brings some conflicts between
`iptables` and `firewalld`.

To solve the conflicts, one have to choose between them, enabling one while
disabling the other in `systemctl`. Since it's said that `iptables` is getting
deprecated in the system, it may be better to use `firewalld`. But here I stick
to `iptables` to leave things simple (And it's easier to find tutorials for
`iptables`).

On Fedora 36 what I'm using, because of the existence of `firewalld`, the
package `iptables-services` is by default uninstalled, thus have to be handled
manually with `dnf install`.
