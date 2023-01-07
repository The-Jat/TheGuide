## Copy Directory

> CopyDirectoryToPackage

```
# copy the directory
CopyDirectoryToPackage MaK
	: [ FDirName $(HAIKU_TOP) data neural ]
	: neural ;
```

It will create the Directory "MaK" in the system directory and copy the contents of the data/neural inside system/MaK/neural/all_the_contents_of_data_neural.
