
>Location = src/system/kernel/fs/vfs.cpp
```c++

static int
file_create(int fd, char* path, int openMode, int perms, bool kernel)
{
	FUNCTION(("file_create: path '%s', omode %x, perms %d, kernel %d\n", path,
		openMode, perms, kernel));

	// get directory to put the new file in
	char name[B_FILE_NAME_LENGTH];
	struct vnode* directory;
	status_t status = fd_and_path_to_dir_vnode(fd, path, &directory, name,
		kernel);
	if (status < 0)
		return status;

	status = create_vnode(directory, name, openMode, perms, kernel);

	put_vnode(directory);
	return status;
}

```

```c++

/*!	\brief Retrieves the directory vnode and the leaf name of an entry referred
		   to by a FD + path pair.

	\a path must be given in either case. \a fd might be omitted, in which
	case \a path is either an absolute path or one relative to the current
	directory. If both a supplied and \a path is relative it is reckoned off
	of the directory referred to by \a fd. If \a path is absolute \a fd is
	ignored.

	The caller has the responsibility to call put_vnode() on the returned
	directory vnode.

	\param fd The FD. May be < 0.
	\param path The absolute or relative path. Must not be \c NULL. The buffer
	       is modified by this function. It must have at least room for a
	       string one character longer than the path it contains.
	\param _vnode A pointer to a variable the directory vnode shall be written
		   into.
	\param filename A buffer of size B_FILE_NAME_LENGTH or larger into which
		   the leaf name of the specified entry will be written.
	\param kernel \c true, if invoked from inside the kernel, \c false if
		   invoked from userland.
	\return \c B_OK, if everything went fine, another error code otherwise.
*/
static status_t
fd_and_path_to_dir_vnode(int fd, char* path, struct vnode** _vnode,
	char* filename, bool kernel)
{
	if (!path)
		return B_BAD_VALUE;
	if (*path == '\0')
		return B_ENTRY_NOT_FOUND;
	if (fd < 0)
		return path_to_dir_vnode(path, _vnode, filename, kernel);

	status_t status = get_dir_path_and_leaf(path, filename);
	if (status != B_OK)
		return status;

	return fd_and_path_to_vnode(fd, path, true, _vnode, NULL, kernel);
}

```

```c++
/*!
	Calls fs_open() on the given vnode and returns a new
	file descriptor for it
*/
static int
create_vnode(struct vnode* directory, const char* name, int openMode,
	int perms, bool kernel)
{
	bool traverse = ((openMode & (O_NOTRAVERSE | O_NOFOLLOW)) == 0);
	status_t status = B_ERROR;
	struct vnode* vnode;
	void* cookie;
	ino_t newID;

	// This is somewhat tricky: If the entry already exists, the FS responsible
	// for the directory might not necessarily also be the one responsible for
	// the node the entry refers to (e.g. in case of mount points or FIFOs). So
	// we can actually never call the create() hook without O_EXCL. Instead we
	// try to look the entry up first. If it already exists, we just open the
	// node (unless O_EXCL), otherwise we call create() with O_EXCL. This
	// introduces a race condition, since someone else might have created the
	// entry in the meantime. We hope the respective FS returns the correct
	// error code and retry (up to 3 times) again.

	for (int i = 0; i < 3 && status != B_OK; i++) {
		// look the node up
		status = lookup_dir_entry(directory, name, &vnode);
		if (status == B_OK) {
			VNodePutter putter(vnode);

			if ((openMode & O_EXCL) != 0)
				return B_FILE_EXISTS;

			// If the node is a symlink, we have to follow it, unless
			// O_NOTRAVERSE is set.
			if (S_ISLNK(vnode->Type()) && traverse) {
				putter.Put();
				char clonedName[B_FILE_NAME_LENGTH + 1];
				if (strlcpy(clonedName, name, B_FILE_NAME_LENGTH)
						>= B_FILE_NAME_LENGTH) {
					return B_NAME_TOO_LONG;
				}

				inc_vnode_ref_count(directory);
				status = vnode_path_to_vnode(directory, clonedName, true, 0,
					kernel, &vnode, NULL);
				if (status != B_OK)
					return status;

				putter.SetTo(vnode);
			}

			if ((openMode & O_NOFOLLOW) != 0 && S_ISLNK(vnode->Type()))
				return B_LINK_LIMIT;

			int fd = open_vnode(vnode, openMode & ~O_CREAT, kernel);
			// on success keep the vnode reference for the FD
			if (fd >= 0)
				putter.Detach();

			return fd;
		}

		// it doesn't exist yet -- try to create it

		if (!HAS_FS_CALL(directory, create))
			return B_READ_ONLY_DEVICE;

		status = FS_CALL(directory, create, name, openMode | O_EXCL, perms,
			&cookie, &newID);
		if (status != B_OK
			&& ((openMode & O_EXCL) != 0 || status != B_FILE_EXISTS)) {
			return status;
		}
	}

	if (status != B_OK)
		return status;

	// the node has been created successfully

	rw_lock_read_lock(&sVnodeLock);
	vnode = lookup_vnode(directory->device, newID);
	rw_lock_read_unlock(&sVnodeLock);

	if (vnode == NULL) {
		panic("vfs: fs_create() returned success but there is no vnode, "
			"mount ID %" B_PRIdDEV "!\n", directory->device);
		return B_BAD_VALUE;
	}

	int fd = get_new_fd(FDTYPE_FILE, NULL, vnode, cookie, openMode, kernel);
	if (fd >= 0)
		return fd;

	status = fd;

	// something went wrong, clean up

	FS_CALL(vnode, close, cookie);
	FS_CALL(vnode, free_cookie, cookie);
	put_vnode(vnode);

	FS_CALL(directory, unlink, name);

	return status;
}

```
