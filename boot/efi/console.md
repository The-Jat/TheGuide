
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

## console_check_boot_keys()
```

uint32
console_check_boot_keys(void)
{
	efi_input_key key;

	for (int i = 0; i < 3; i++) {
		// give the user a chance to press a key
		kBootServices->Stall(100000);

		efi_status status = kSystemTable->ConIn->ReadKeyStroke(
			kSystemTable->ConIn, &key);

		if (status != EFI_SUCCESS)
			continue;

		if (key.UnicodeChar == 0 && key.ScanCode == SCAN_ESC)
			return BOOT_OPTION_DEBUG_OUTPUT;
		if (key.UnicodeChar == ' ')
			return BOOT_OPTION_MENU;
	}
	return 0;
}

```
