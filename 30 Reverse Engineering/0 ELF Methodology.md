# ELF Methodology

```powershell
# Strings
strings filename

# Strace
strace filename

# Ltrace
ltrace filename

# Static analysis with ghidra
ghidra

# Dynamic analysis with gdb
gdb filename
```

### GDB

Workflow within `gdb` generally involves the following commands:

```powershell
# Open the file with gdb
gdb encrypt

# Set a breakpoint at main
b main

# Move to the next instruction
ni

# Continue to another breakpoint
c

# Exit/quit
q
```
