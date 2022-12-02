

headers/private/kernel/fs/fd.h\
```c++
struct fd_ops {
	status_t	(*fd_read)(struct file_descriptor *, off_t pos, void *buffer,
						size_t *length);
	status_t	(*fd_write)(struct file_descriptor *, off_t pos,
						const void *buffer, size_t *length);
	off_t		(*fd_seek)(struct file_descriptor *, off_t pos, int seekType);
	status_t	(*fd_ioctl)(struct file_descriptor *, ulong op, void *buffer,
						size_t length);
	status_t	(*fd_set_flags)(struct file_descriptor *, int flags);
	status_t	(*fd_select)(struct file_descriptor *, uint8 event,
						struct selectsync *sync);
	status_t	(*fd_deselect)(struct file_descriptor *, uint8 event,
						struct selectsync *sync);
	status_t	(*fd_read_dir)(struct io_context* ioContext,
						struct file_descriptor *, struct dirent *buffer,
						size_t bufferSize, uint32 *_count);
	status_t	(*fd_rewind_dir)(struct file_descriptor *);
	status_t	(*fd_read_stat)(struct file_descriptor *, struct stat *);
	status_t	(*fd_write_stat)(struct file_descriptor *, const struct stat *,
		int statMask);
	status_t	(*fd_close)(struct file_descriptor *);
	void		(*fd_free)(struct file_descriptor *);
};
```
