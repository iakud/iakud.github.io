---
layout: post
title: "Linux进程状态"
date: 2021-06-23 10:59:00 +0800
category: note
typora-root-url: ..
---

| 状态 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| D    | uninterruptible sleep (usually IO)                           |
| R    | running or runnable (on run queue)                           |
| S    | interruptible sleep (waiting for an event to complete)       |
| T    | stopped by job control signal                                |
| t    | stopped by debugger during the tracing                       |
| W    | paging (not valid since the 2.6.xx kernel)                   |
| X    | dead (should never be seen)                                  |
| Z    | defunct ("zombie") process, terminated but not reaped by its parent |
| <    | high-priority (not nice to other users)                      |
| N    | low-priority (nice to other users)                           |
| L    | has pages locked into memory (for real-time and custom IO)   |
| s    | is a session leader                                          |
| l    | is multi-threaded (using CLONE_THREAD, like NPTL pthreads do) |
| +    | is in the foreground process group                           |

