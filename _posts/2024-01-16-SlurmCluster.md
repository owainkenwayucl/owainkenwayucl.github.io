---
layout: post
title: Multi-node Slurm cluster
---

I've been doing some teaching of students, specifically a discipline we've called "data engineering" and the documentation of Slurm is quite bad and obfuscates how to do a very simple thing - namely - how to set up a Slurm *cluster*.

And so I thought I'd put this up here so that it's google-able.

1. Make sure you have two or more nodes (you can do with with one but that's trivial) on a private address range with the same users defined (preferrably through a directory service but whatever) and for sanity sake a shared filesystem.

2. Install `slurmctld` on the "head" node, and `slurmd` on any compute nodes as well as `munge` on all nodes. On AlmaLinux 9 these are all in your repos.

3. Generate a munge key with `create-munge-key` and copy it to all the nodes (`/etc/munge/munge.key`).

4. Enable and start `munge` with `systemctl`.

5. Edit `/etc/slurm/slurm.conf` to make sure all the nodes are in it at the end. There should be *one* node name line - the default one in AlmaLinux has `localhost` in it so replace it with a comma separated list of compute nodes and their shape (number of cores). You also need to change the `SlurmctldHost` to an external interface on the "head" node.

6. Make sure `/etc/slurm/slurm.conf` is identical on all nodes (shared filesystem can help here).

7. Enable and start `slurmd` on each compute node (with `systemctl`).

8. Enable and start `slurmctld` on head node (with `systemctl`)..

You should now be able to use `sinfo`/`squeue` etc and submit jobs.

To add a new node, add it to `slurm.conf` (synchronise on every node) and then either restart every instance of `slurmd` and `slurmctld` or run `scontrol reconfigure`.
