# Windows Internals

## COM

COM is based on two foundational principles. First, clients communicate with objects (sometimes called COM server objects) through interfaces—well-defined contracts with a set of logically related methods grouped under the virtual table dispatch mechanism, which is also a common way for C++ compilers to implement virtual functions dispatch. This results in binary compatibility and removal of compiler name mangling issues. Consequently, it is possible to call these methods from many languages (and compilers), such as C, C++, Visual Basic, .NET languages, Delphi and others. The second principle is that component implementation is loaded dynamically rather than being statically linked to the client.

## .NET Framework

The .NET Framework consists of two major components:

- **The Common Language Runtime (CLR)** - This is the run-time engine for .NET and includes a Just In Time (JIT) compiler that translates Common Intermediate Language (CIL) instructions to the underlying hardware CPU machine language, a garbage collector, type verification, code access security, and more. It’s implemented as a COM in-process server (DLL) and uses various facilities provided by the Windows API.
- **The .NET Framework Class Library (FCL)** - This is a large collection of types that implement functionality typically needed by client and server applications, such as user interface services, networking, database access, and much more.

![](Attachments/Pasted%20image%2020241221214635.png)

## Services, Functions, and Routines

Several terms in the Windows user and programming documentation have different meanings in different contexts. For example, the word service can refer to a callable routine in the OS, a device driver, or a server process. The following list describes what certain terms mean in this book:

- **Windows API functions** - These are documented, callable subroutines in the Windows API. Examples include CreateProcess, CreateFile, and GetMessage.
- **Native system services (or system calls)** - These are the undocumented, underlying services in the OS that are callable from user mode. For example, NtCreateUserProcess is the internal system service the Windows CreateProcess function calls to create a new process.
- **Kernel support functions (or routines)** - These are the subroutines inside the Windows OS that can be called only from kernel mode (defined later in this chapter). For example, ExAllocatePoolWithTag is the routine that device drivers call to allocate memory from the Windows system heaps (called pools).
- **Windows services** - These are processes started by the Windows service control manager. For example, the Task Scheduler service runs in a user-mode process that supports the schtasks command (which is similar to the UNIX commands at and cron). (Note that although the registry defines Windows device drivers as “services,” they are not referred to as such in this book.)
- **Dynamic link libraries (DLLs)** - These are callable subroutines linked together as a binary file that can be dynamically loaded by applications that use the subroutines. Examples include Msvcrt.dll (the C run-time library) and Kernel32.dll (one of the Windows API subsystem libraries). Windows user-mode components and applications use DLLs extensively. The advantage DLLs provide over static libraries is that applications can share DLLs, and Windows ensures that there is only one in-memory copy of a DLL’s code among the applications that are referencing it. Note that library .NET assemblies are compiled as DLLs but without any unmanaged exported subroutines. Instead, the CLR parses compiled metadata to access the corresponding types and members.

## Processes

Although programs and processes appear similar on the surface, they are fundamentally different. A program is a static sequence of instructions, whereas a process is a container for a set of resources used when executing the instance of the program. At the highest level of abstraction, a Windows process comprises the following:

- **A private virtual address space** - This is a set of virtual memory addresses that the process can use.
- **An executable program** - This defines initial code and data and is mapped into the process’s virtual address space.
- **A list of open handles** - These map to various system resources such as semaphores, synchronization objects, and files that are accessible to all threads in the process.
- **A security context** - This is an access token that identifies the user, security groups, privileges, attributes, claims, capabilities, User Account Control (UAC) virtualization state, session, and limited user account state associated with the process, as well as the AppContainer identifier and its related sandboxing information.
- **A process ID** - This is a unique identifier, which is internally part of an identifier called a client ID.
- **At least one thread of execution** - Although an “empty” process is possible, it is (mostly) not useful.

## Threads

A thread is an entity within a process that Windows schedules for execution. Without it, the process’s program can’t run. A thread includes the following essential components:

- The contents of a set of CPU registers representing the state of the processor
- Two stacks—one for the thread to use while executing in kernel mode and one for executing in user mode
- A private storage area called thread-local storage (TLS) for use by subsystems, run-time libraries, and DLLs
- A unique identifier called a thread ID (part of an internal structure called a client ID; process IDs and thread IDs are generated out of the same namespace, so they never overlap)

In addition, threads sometimes have their own security context, or token, which is often used by multithreaded server applications that impersonate the security context of the clients that they serve. The volatile registers, stacks, and private storage area are called the thread’s context. Because this information is different for each machine architecture that Windows runs on, this structure, by necessity, is architecture-specific. The Windows GetThreadContext function provides access to this architecture-specific information (called the CONTEXT block).

Although threads have their own execution context, every thread within a process shares the process’s virtual address space (in addition to the rest of the resources belonging to the process), meaning that all the threads in a process have full read-write access to the process virtual address space. Threads cannot accidentally reference the address space of another process, however, unless the other process makes available part of its private address space as a shared memory section (called a file mapping object in the Windows API) or unless one process has the right to open another process to use cross-process memory functions, such as ReadProcessMemory and WriteProcessMemory (which a process that’s running with the same user account, and not inside of an AppContainer or other type of sandbox, can get by default unless the target process has certain protections).

In addition to a private address space and one or more threads, each process has a security context and a list of open handles to kernel objects such as files, shared memory sections, or one of the synchronization objects such as mutexes, events, or semaphores, as illustrated in Figure 1-2.

![](Attachments/Pasted%20image%2020241221220959.png)

Each process’s security context is stored in an object called an access token. The process access token contains the security identification and credentials for the process. By default, threads don’t have their own access token, but they can obtain one, thus allowing individual threads to impersonate the security context of another process—including processes on a remote Windows system—without affecting other threads in the process.
