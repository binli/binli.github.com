---
title: The D-Bus interface of systemd-logind
date: 2014-10-31T16:34:00+08:00
categories: ["worklog"]
tags: ["systemd"]
draft: true
---
# wake request

_internal_sleep ==> impl_manager_sleep ==> Sleep (org.freedesktop.NetworkManager)

```bash
# Turn off network
$ gdbus call --system --dest org.freedesktop.NetworkManager --object-path /org/freedesktop/NetworkManager --method org.freedesktop.NetworkManager.Enable false

# Suspend
$ gdbus call --system --dest org.freedesktop.login1 --object-path /org/freedesktop/login1 --method  org.freedesktop.login1.Manager.Suspend true
```

# Systemd's login1

In manager_message_handler() of src/login/logind-dbus.c

```C
} else if (dbus_message_is_method_call(message, "org.freedesktop.login1.Manager", "Hibernate")) {
        r = bus_manager_do_shutdown_or_sleep(
                        m, connection, message,
                        SPECIAL_HIBERNATE_TARGET,
                        INHIBIT_SLEEP,
                        "org.freedesktop.login1.hibernate",
                        "org.freedesktop.login1.hibernate-multiple-sessions",
                        "org.freedesktop.login1.hibernate-ignore-inhibit",
                        "hibernate",
                        &error, &reply);
        if (r < 0)
                return bus_send_error_reply(connection, message, &error, r);

```
```C
} else if (dbus_message_is_method_call(message, "org.freedesktop.login1.Manager", "HybridSleep")) {
        r = bus_manager_do_shutdown_or_sleep(
                        m, connection, message,
                        SPECIAL_HYBRID_SLEEP_TARGET,
                        INHIBIT_SLEEP,
                        "org.freedesktop.login1.hibernate",
                        "org.freedesktop.login1.hibernate-multiple-sessions",
                        "org.freedesktop.login1.hibernate-ignore-inhibit",
                        "hybrid-sleep",
                        &error, &reply);
        if (r < 0)
                return bus_send_error_reply(connection, message, &error, r);

```
