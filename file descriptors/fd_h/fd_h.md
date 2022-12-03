

headers/private/kernel/fs/fd.h\
```c++
struct file_descriptor {
	int32	type;               /* descriptor type */
	int32	ref_count;
	int32	open_count;
	struct fd_ops *ops;
	union {
		struct vnode *vnode;
		struct fs_mount *mount;
		struct net_socket *socket;
	} u;
	void	*cookie;
	int32	open_mode;
	off_t	pos;
};
```
\
references:-
\
[fd_ops](/file%20descriptors/fd_h/fd_ops.md) structure.
\
[vnode](/fs/fs_h.md) structure.
\
[fs_mount](/fs/fs_mount.md) structure.
\
[net_socket](/socket/net_socket.md) structure.

