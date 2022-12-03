
>Location = src/system/kernel/fs/vfs.cpp
'''c++

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

'''
