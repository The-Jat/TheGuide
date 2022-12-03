
>Location = src/system/kernel/fs/vfs.cpp
```c++

static int
file_open(int fd, char* path, int openMode, bool kernel)
{
	bool traverse = (openMode & (O_NOTRAVERSE | O_NOFOLLOW)) == 0;

	FUNCTION(("file_open: fd: %d, entry path = '%s', omode %d, kernel %d\n",
		fd, path, openMode, kernel));

	// get the vnode matching the vnode + path combination
	struct vnode* vnode;
	ino_t parentID;
	status_t status = fd_and_path_to_vnode(fd, path, traverse, &vnode,
		&parentID, kernel);
	if (status != B_OK)
		return status;

	if ((openMode & O_NOFOLLOW) != 0 && S_ISLNK(vnode->Type())) {
		put_vnode(vnode);
		return B_LINK_LIMIT;
	}

	// open the vnode
	int newFD = open_vnode(vnode, openMode, kernel);
	if (newFD >= 0) {
		// The vnode reference has been transferred to the FD
		cache_node_opened(vnode, FDTYPE_FILE, vnode->cache,
			vnode->device, parentID, vnode->id, NULL);
	} else
		put_vnode(vnode);

	return newFD;
}

```
