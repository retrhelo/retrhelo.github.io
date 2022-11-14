---
title: "Set up your own Git server with SSH service"
tags: ["network"]
date: 2022-11-14T10:27:02+08:00
---

This article introduces how to set up a Git server at your own remote machine.
Git repository can be easily set up at local machine, referred to by its path.
The approach applied almostly the same when it comes to a remote server,
requiring only the SSH service as the extra stuff.

<!--more-->

## SSH Server

SSH service is quite common for remote servers, especially whose running Linux.
It gives users the access to interact with the server via command line shell,
which pretty much feels the same as Linux running locally.

SSH service also enables data transfer between the server and your client
machine. Commands like `scp` uses SSH as an approach to copy files through the
network.

### Configure the SSH server

There're some configurations to be done before we head for Git. First, you
should have a passwordless access to your server via SSH. If you're not,
there're plenty of tutorials online. Google and check it out.

## Set up Git server

The way to set up a Git repository at the remote machine is pretty much the same
as it's at local: create a folder and `git init` it. Let's say we create a Git
repository in `/data/git/repo_example`. In the local case, you can easily clone
the repository with `git clone /data/git/repo_example`, with Git remote set as
`/data/git/repo_example`.

As the rule changes little with a remote SSH access. The command to clone is now
`git clone ssh://alice@1.2.3.4:2222/data/git/repo_example` (In this example, we
assume `1.2.3.4` as the IP address of the remote machine, `2222` as the port
where sshd listens, and `alice` as the username).

### Configure the file permission

However, you may not be able to clone the repository down to your local machine,
or later not be able to push your commits. That's because the your remtoe
account doesn't have the right read/write permission.

Linux uses three classes of file permissions: user, group and other, and each
class contains three types: read, write and execute (marked as `rwx`). To clone
the repository with account `alice`, `alice` must have a write permission to it.
And to push, `alice` must have the write permission.

It's recommended letting a special user, say `git`, to have the read/write
permission of your remote Git directory. And user `git` would be managing all
Git repositories at the server. Repositories can be cloned via the unified user
`git`.

However, this approach requires to setup passwordless login every time adding a
new user. And it's hard to trace the creator/modifier of certain files. And
since every one is as the user `git`, it's difficult to create a finer-grained
permission control among users.

I make use of the permission group system that comes with Linux. My scheme is to
have the group own the directory (In our case, it's `/data/git`), and every one
involved is added to the group. The group has the read/write permission, so does
its members. By this way, every one can access the repositories from their own
accounts, and the access permission is directly controlled by the groups that
they're in.

### Set the directory suitable for multi-user environment

With multiple users involved, you would want every file created in the
repositories belong to the very group that owns its repository. Unfortunately,
if not configured, the file by default belongs the creator's group (the one
that's created sharing the same name with the user). It leads to permission
problems when multiple users are trying to push/pull commits to/from the
repository (since not every one belongs the creator's group).

To solve this problem, we must change the default group for all files created in
it. It's done by command `chmod g+s <directory>`. For example, if `/data/git`
belongs to group `git`, then `chmod g+s /data/git` makes every subdirectories
created later in it belong to `git`, too. And these subdirectories adopt this
attribute.

It's worth attention that `chmod g+s` only applies to subdirectories created
after the command's taking effect. Subdirectories that are created before it are
not affected.
