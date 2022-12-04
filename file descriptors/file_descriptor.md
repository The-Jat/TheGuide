### What is file descriptor ?.
A file descriptor is an unsigned integer used by a process to identify an open file.
\
It is non negative integer.
\
It points to the entry in the file decriptor table.

\
Every process has a file descriptor table in their process control block(PCB) which tells
\
which files have been opened/used by current process.
\
Each file`s index, or file descriptor (fd), is an integer, are stored in the file descriptor table as integers, starting with 0.
However, 0, 1, and 2 are pre-allocated to stdin, stdout, and stderr, respectively.
\
### File Descriptor
when you open a file, the operating system creates an entry to represent that file
and store the information about that opened file.\ 
\So if there are 100 files opened
in your OS then there will be 100 entries in OS (somewhere in kernel).\
These entries are represented by integers like (...100, 101, 102....).\
This entry number is the file descriptor. So it is just an integer number
that uniquely represents an opened file for the process. If your process
opens 10 files then your Process table will have 10 entries for file descriptors.
\
* A file descriptor is a number that uniquely identifies an open file in a computer's operating system. It describes a data resource, and how that resource may be accessed. When a program asks to open a file — or another data resource, like a network socket — the kernel: Grants access.
\
> ## Everything is a file-

If you have been working or reading about Linux for some time, you must have come across the phrase, “Everything is a file” in Linux. What does this really mean though?
\
Imagine you are a process and you want to send some data over the network(it doesn’t matter where for now). The laptop/PC/server where you are running probably has some network adapters, sending a stream of bytes to these network adapters would send your file over the network. However, the network interface card may be manufactured by some particular manufacturer or adhere to some protocol and it may have defined its own standard way of taking input. You, as a process, probably shouldn’t care about that. If you had to, you’d probably have to do a lot of work to support Intel network interface cards, AMD network interface cards, etc. What if you want to store data? How many different SSD makes would you have to support then? What if the system has an HDD instead of an SSD? Imagine writing all of this code in your programs that you made, a function to handle storing data in SSDs, one function to store data in Samsung HDDs, one in some other manufacturer’s HDD, etc.
\
All this seems like a lot of work, and you as a process already have a lot of stuff to do. So, Linux uses the concept of files for interoperability. In short, when you need to write data, you write data to a network socket or to a hard disk, or anywhere else as if it was a file. Your operating system will take this stream of bytes and make it available to whoever it was meant to go to. The process in charge of receiving this stream of bytes will read them as if they were a file. So files become like a middleman and since it is a single standard, every process just needs to learn to read and write to files. Even when you need to read data from your keyboard or mouse, it is actually getting sent through a file. So, when you press a key on your keyboard, Linux stores that text in some file. Other processes can read this file, and can thus find out what you are typing. This ensures that if you switched to a different keyboard, as long as the OS supports the keyboard, it would keep sending data to that same file, and your program never even finds out it's a different keyboard, it just keeps on reading the same file.
\
So treating everything as a file ensures interoperability between different processes. Every process can read from files and write to files, and internally your operating system would map the stream of bytes to the correct process. So if I am a process and I want to print a document, all I have to do is write the document as a file, and Linux would ensure that the printer’s drivers(also running as processes) would read from that file.
\
[file_descriptor](/file%20descriptors/fd_h/fd_h.md) structure.
