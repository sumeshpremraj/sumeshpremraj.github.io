---
title: "Linux Completely Fair Scheduler (CFS)"
date: 2021-11-22
tags: ["linux"]
---

Recently, I have been reading about Kubernetes in preparation for some upcoming tasks at work. As an SRE, my job involves thinking a lot about reliability and handling failures, and I stumbled upon [k8s.af](https://k8s.af) which is a constantly growing collection of Kubernetes failure stories.

And from there I found Omio's story of [CPU limits and throttling](https://medium.com/omio-engineering/cpu-limits-and-aggressive-throttling-in-kubernetes-c5b20bd8a718), which involves a CFS quota bug. I had some shallow and mostly forgotten knowledge of process schedulers from my Operating Systems course in university, so I decided to read up more about it before reading Omio's story.

### Process scheduler
A process scheduler assigns resources (like CPU time) to tasks (like a process). The scheduler decides which tasks get how much CPU time, and is also responsible for prioritizing and preempting.

CFS was merged into the Linux kernel in 2007, and is the default desktop scheduler. Unlike some other schedulers which provide fixed time slices and allows priorities for tasks, CFS aims to provide a fair share of CPU time to all tasks.

### How does CFS algorithm work?
CFS implements per-CPU task queues. Each task descriptor contains a `sched_entity` field, and these are sorted into a red-black tree by execution time. 

The left-most node of the red-black tree has the least execution time, and whenever the scheduler is invoked it executes the corresponding task. In other words, this means the tasks which have previously spent the least execution time get scheduled sooner.

If the task is completed, it is removed from the tree. If it is preempted, its execution time is updated and it is again inserted into the rb-tree.

Then, the left-most node is again executed and so on.

This is an O log(n) algorithm. Check out [bigocheatsheet.com](http://bigocheatsheet.com) to remind yourself of the algorithm complexity analysis.

I have skipped over a lot of the finer details for brevity's sake. You can read the [kernel documentation for CFS](https://www.kernel.org/doc/html/latest/scheduler/sched-design-CFS.html) for more details.