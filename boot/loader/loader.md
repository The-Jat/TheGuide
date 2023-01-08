
## loader
> location = src/system/boot/loader/loader.cpp

## load_kernel

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
