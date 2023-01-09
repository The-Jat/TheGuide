
## loader
> location = src/system/boot/loader/loader.cpp


## open_maybe_packaged

```
int
open_maybe_packaged(BootVolume& volume, const char* path, int openMode)
{
	if (strncmp(path, kSystemDirectoryPrefix, strlen(kSystemDirectoryPrefix))
			== 0) {
		path += strlen(kSystemDirectoryPrefix);
		return open_from(volume.SystemDirectory(), path, openMode);
	}

        return open_from(volume.RootDirectory(), path, openMode);
}
```

* [open_from](/boot/loader/vfs.md#open_from)


## find_kernel

> arg1 = BootVolume& volume

> arg2 = const char** name = NULL

```
static int
find_kernel(BootVolume& volume, const char** name = NULL)
{
	for (int32 i = 0; sKernelPaths[i][0] != NULL; i++) {
		int fd = open_maybe_packaged(volume, sKernelPaths[i][0], O_RDONLY);
		if (fd >= 0) {
			if (name)
				*name = sKernelPaths[i][1];

			return fd;
		}
	}

	return B_ENTRY_NOT_FOUND;
}
```

* int fd = [open_maybe_packaged](#open_maybe_packaged)(volume, sKernelPaths[i][0], O_RDONLY);


## load_kernel

> arg1 = stage2_args* args

> BootVolume& volume

```
status_t
load_kernel(stage2_args* args, BootVolume& volume)
{
	const char *name;
	int fd = find_kernel(volume, &name);
	if (fd < B_OK)
		return fd;

	dprintf("load kernel %s...\n", name);

	elf_init();
	preloaded_image *image;
	status_t status = elf_load_image(fd, &image);

	close(fd);

	if (status < B_OK) {
		dprintf("loading kernel failed: %" B_PRIx32 "!\n", status);
		return status;
	}

	gKernelArgs.kernel_image = image;

	status = elf_relocate_image(gKernelArgs.kernel_image);
	if (status < B_OK) {
		dprintf("relocating kernel failed: %" B_PRIx32 "!\n", status);
		return status;
	}

	gKernelArgs.kernel_image->name = kernel_args_strdup(name);

	return B_OK;
}

```

* int fd = [find_kernel](#find_kernel)(volume, &name);
* [elf_init();](/boot/loader/elf.md#elf_init)
