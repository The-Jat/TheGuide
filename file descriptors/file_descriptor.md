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
[file_descriptor](file descriptors/fd_h.md)
