
```c++

struct fs_mount {
	fs_mount()
		:
		volume(NULL),
		device_name(NULL)
	{
		mutex_init(&lock, "mount lock");
	}

	~fs_mount()
	{
		mutex_destroy(&lock);
		free(device_name);

		while (volume) {
			fs_volume* superVolume = volume->super_volume;

			if (volume->file_system != NULL)
				put_module(volume->file_system->info.name);

			free(volume->file_system_name);
			free(volume);
			volume = superVolume;
		}
	}

	struct fs_mount* next;
	dev_t			id;
	fs_volume*		volume;
	char*			device_name;
	mutex			lock;	// guards the vnodes list
	struct vnode*	root_vnode;
	struct vnode*	covers_vnode;	// immutable
	KPartition*		partition;
	VnodeList		vnodes;
	EntryCache		entry_cache;
	bool			unmounting;
	bool			owns_file_device;
};

```
