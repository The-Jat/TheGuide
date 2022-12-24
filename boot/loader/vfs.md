## vfs

## vfs_init(stage2_args *args)


```
status_t
vfs_init(stage2_args *args)
{
	gRoot = new(nothrow) RootFileSystem();
	if (gRoot == NULL)
		return B_NO_MEMORY;

	return B_OK;
}
```

## get_boot_file_system

```

/*! Gets the boot device, scans all of its partitions, gets the
	boot partition, and mounts its file system.
	\param args The stage 2 arguments.
	\param _bootVolume On success set to the boot volume.
	\return \c B_OK on success, another error code otherwise.
*/
status_t
get_boot_file_system(stage2_args* args, BootVolume& _bootVolume)
{
	status_t error = platform_add_boot_device(args, &gBootDevices);
	if (error != B_OK)
		return error;

	NodeIterator iterator = gBootDevices.GetIterator();
	while (iterator.HasNext()) {
		Node *device = iterator.Next();

		error = add_partitions_for(device, false, true);
		if (error != B_OK)
			continue;

		NodeList bootPartitions;
		error = platform_get_boot_partitions(args, device, &gPartitions, &bootPartitions);
		if (error != B_OK)
			continue;

		NodeIterator partitionIterator = bootPartitions.GetIterator();
		while (partitionIterator.HasNext()) {
			Partition *partition = (Partition*)partitionIterator.Next();

			Directory *fileSystem;
			error = partition->Mount(&fileSystem, true);
			if (error != B_OK) {
				// this partition doesn't contain any known file system; we
				// don't need it anymore
				gPartitions.Remove(partition);
				delete partition;
				continue;
			}

			// init the BootVolume
			error = _bootVolume.SetTo(fileSystem);
			if (error != B_OK)
				continue;

			sBootDevice = device;
			return B_OK;
		}
	}

	return B_ERROR;
}

```


## BootVolume()

```
BootVolume::BootVolume()
	:
	fRootDirectory(NULL),
	fSystemDirectory(NULL),
	fPackageVolumeInfo(NULL),
	fPackageVolumeState(NULL)
{
}
```
