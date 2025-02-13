---
tags:
  - tool
---
# SysWhispers

[SysWhispers](https://github.com/klezVirus/SysWhispers3) helps with evasion by generating header/ASM files implants can use to make direct system calls.

## Capabilities

```bash
# Generate required files for classic injection
python syswhispers.py -a x64 -c msvc -m jumper_randomized -f NtAllocateVirtualMemory,NtProtectVirtualMemory,NtWriteVirtualMemory,NtCreateThreadEx -o SysWhispers -v

# Generate required files for mapping injection
python syswhispers.py -a x64 -c msvc -m jumper_randomized -f NtCreateSection,NtMapViewOfSection,NtUnmapViewOfSection,NtClose,NtCreateThreadEx -o SysWhispers -v

# Generate required files for APC injection
python syswhispers.py -a x64 -c msvc -m jumper_randomized -f NtAllocateVirtualMemory,NtProtectVirtualMemory,NtWriteVirtualMemory,NtQueueApcThread -o SysWhispers -v
```

**Note:** Three files are generated: `SysWhispers.h`, `SysWhispers.c` and `SysWhispers-asm.x64.asm`.

REDACTED