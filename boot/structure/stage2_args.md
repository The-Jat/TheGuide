### stage2_args structure.

> location = headers/private/kernel/boot/stage2_args.h
```
typedef struct stage2_args {
	size_t heap_size;
	const char **arguments;
	int32 arguments_count;
	struct platform_stage2_args	platform;
} stage2_args ;
```
