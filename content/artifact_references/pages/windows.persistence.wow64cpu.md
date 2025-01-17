---
title: Windows.Persistence.Wow64cpu
hidden: true
tags: [Client Artifact]
---

Checks for wow64cpu.dll replacement Autorun in Windows 10.
http://www.hexacorn.com/blog/2019/07/11/beyond-good-ol-run-key-part-108-2/


```yaml
name: Windows.Persistence.Wow64cpu
description: |
  Checks for wow64cpu.dll replacement Autorun in Windows 10.
  http://www.hexacorn.com/blog/2019/07/11/beyond-good-ol-run-key-part-108-2/

author: Matt Green - @mgreen27

parameters:
   - name: TargetRegKey
     default: HKEY_LOCAL_MACHINE\Software\Microsoft\Wow64\**
sources:
  - precondition:
      SELECT OS From info() where OS = 'windows'

    query: |
      SELECT dirname(path=FullPath) as KeyPath,
        Name as KeyName,
        Data.value as Value,
        Mtime AS LastModified
      FROM glob(globs=split(string=TargetRegKey, sep=","), accessor="registry")
      WHERE Data.value and
        not (Name = "@" and (Data.value =~ "(wow64cpu.dll|wowarmhw.dll|xtajit.dll)"))

```
