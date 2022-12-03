### Syscalls
#### What is system call ?
A System call is a mechanism that provides the interface between a process
and the operating system.\
\
It is a programmtic method in which a computer program requests a service
from the kernel of the OS.\
\
System call offers the services of the operating system to the user
programs via the API(Application Programming Interface).\

System call are the only entry points for the kernel system.

>How System Calls Work

The Applications run in an area of memory known as user space. A system call connects to the operating system's kernel, which executes in kernel space. When an application creates a system call, it must first obtain permission from the kernel. It achieves this using an interrupt request, which pauses the current process and transfers control to the kernel.
\
If the request is permitted, the kernel performs the requested action, like creating or deleting a file. As input, the application receives the kernel's output. The application resumes the procedure after the input is received. When the operation is finished, the kernel returns the results to the application and then moves data from kernel space to user space in memory.
\
A simple system call may take few nanoseconds to provide the result, like retrieving the system date and time. A more complicated system call, such as connecting to a network device, may take a few seconds. Most operating systems launch a distinct kernel thread for each system call to avoid bottlenecks. Modern operating systems are multi-threaded, which means they can handle various system calls at the same time.
\
![Alt Architecture](/Syscalls/syscalls_workflow.webp)


#### Haiku OS.
In haiku os the syscalls API is based on translation.
\
that is :- user_syscallName -> _kern_syscallName.

[_kern_open](/Syscalls/kern/_kern_open.md) syscall.
