---
title: Linux.Users.InteractiveUsers
hidden: true
tags: [Client Artifact]
---

Gets the interactive users from a Linux host.


```yaml
name: Linux.Users.InteractiveUsers

description: |
  Gets the interactive users from a Linux host.

author: George-Andrei Iosif (@iosifache)

type: CLIENT

parameters:
  - name: NonInteractiveExecutables
    description: Non-interactive executables that may appear in user's details
    type: str
    default: "/usr/sbin/nologin,/bin/false,/sbin/nologin,/bin/sync"

sources:
  - precondition: |
      SELECT OS
      FROM info()
      WHERE OS = 'linux'

    query: |
      SELECT Fqdn AS Host,
              User,
              Description,
              Uid,
              Gid,
              Homedir,
              Shell 
      FROM Artifact.Linux.Sys.Users()
      WHERE NOT Shell IN split(string=NonInteractiveExecutables, sep_string=",")

```
