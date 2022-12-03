

>Location = src/system/kernel/fs/vfs.cpp

```c++

/*!	\brief Opens a node specified by a FD + path pair.

	At least one of \a fd and \a path must be specified.
	If only \a fd is given, the function opens the node identified by this
	FD. If only a path is given, this path is opened. If both are given and
	the path is absolute, \a fd is ignored; a relative path is reckoned off
	of the directory (!) identified by \a fd.

	\param fd The FD. May be < 0.
	\param path The absolute or relative path. May be \c NULL.
	\param openMode The open mode.
	\return A FD referring to the newly opened node, or an error code,
			if an error occurs.
*/
int
_kern_open(int fd, const char* path, int openMode, int perms)
{
	KPath pathBuffer(path, KPath::LAZY_ALLOC);
	if (pathBuffer.InitCheck() != B_OK)
		return B_NO_MEMORY;

	if ((openMode & O_CREAT) != 0)
		return file_create(fd, pathBuffer.LockBuffer(), openMode, perms, true);

	return file_open(fd, pathBuffer.LockBuffer(), openMode, true);
}


```

[_file_create](/kernel/fs/file_create.md) syscall.
[_file_open](/kernel/fs/file_open.md) call.

