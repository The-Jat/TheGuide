## load driver settings

> location = src/system/boot/loader/load_driver_settings.cpp

## add_stage2_driver_settings(stage2_args* args)

> arg1 = [stage2_args](/boot/structure/stage2_args.md)* args

```
status_t
add_stage2_driver_settings(stage2_args* args)
{
	// TODO: split more intelligently
	for (const char** arg = args->arguments;
			arg != NULL && args->arguments_count-- && arg[0] != NULL; arg++) {
		//dprintf("adding args: '%s'\n", arg[0]);
		add_safe_mode_settings((char*)arg[0]);
	}
	return B_OK;
}
```
It does not do anything in x86.


## load_driver_settings

> arg1 = stage2_args*

> arg2 = Directory* volume

```

status_t
load_driver_settings(stage2_args* /*args*/, Directory* volume)
{
	int fd = open_from(volume, "home/config/settings/kernel/drivers", O_RDONLY);
	if (fd < B_OK)
		return fd;

	Directory* settings = (Directory*)get_node_from(fd);
	if (settings == NULL)
		return B_ENTRY_NOT_FOUND;

	void* cookie;
	if (settings->Open(&cookie, O_RDONLY) == B_OK) {
		char name[B_FILE_NAME_LENGTH];
		while (settings->GetNextEntry(cookie, name, sizeof(name)) == B_OK) {
			if (!strcmp(name, ".") || !strcmp(name, ".."))
				continue;

			status_t status = load_driver_settings_file(settings, name);
			if (status != B_OK)
				dprintf("Could not load \"%s\" error %" B_PRIx32 "\n", name, status);
		}

		settings->Close(cookie);
	}

	return B_OK;
}

```

* [open_from](/boot/loader/vfs.md#open_from)
* Directory* settings = (Directory*)[get_node_from](vfs.md#get_node_from)(fd);

