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
In haiku it is of structure type:-\
headers/private/kernel/fs/fd.h\
\
struct file_descriptor {\
	int32	type;               /* descriptor type */\
	int32	ref_count;\
	int32	open_count;\
	struct fd_ops *ops;\
	union {\
		struct vnode *vnode;\
		struct fs_mount *mount;\
		struct net_socket *socket;\
	} u;\
	void	*cookie;\
	int32	open_mode;\
	off_t	pos;\
};\
