---
title: freeze and mem in power state 
date: 2023-03-31T10:34:00+08:00
categories: ["worklog"]
tags: ["kernel"]
draft: false
---
# What's the meanings of power state

https://www.kernel.org/doc/Documentation/power/states.txt

https://www.kernel.org/doc/Documentation/power/interface.txt

* In /sys/power/state
  - "mem" is controlled by the /sys/power/mem_sleep file
  - "standby" - Power-On Suspend (if supported)
  - "freeze" - Suspend-To-Idle
  - "disk", hibernation (Suspend-To-Disk).
* In /sys/power/mem_sleep
  - "s2idle" (Suspend-To-Idle)
  - "shallow" (Power-On Suspend)
  - "deep" (Suspend-To-RAM)
* In /sys/power/disk
  - 'platform' (put the system into sleep using a platform-provided method)
  - 'shutdown' (shut the system down)
  - 'reboot' (reboot the system)
  - 'suspend' (trigger a Suspend-to-RAM transition)
  - 'test_resume' (resume-after-hibernation test mode)
