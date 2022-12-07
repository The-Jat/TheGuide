
#### console

> location = src/system/boot/platform/efi/console.cpp

```

console_init(void)
{
#if 1
	gConsoleNode = &sConsole;

	update_screen_size();
	console_hide_cursor();
	console_clear_screen();
#else
	// FIXME: This does not work because we cannot initialize video before VFS, as it
	// needs to read the driver settings before setting a mode; and also because the
	// heap does not yet exist.
	platform_init_video();
	platform_switch_to_logo();
	gConsoleNode = video_text_console_init(gKernelArgs.frame_buffer.physical_buffer.start);
#endif

	// enable stdio functionality
	stdin = (FILE *)gConsoleNode;
	stdout = stderr = (FILE *)gConsoleNode;

	return B_OK;
}

```
