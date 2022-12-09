>### Booting

----

##### In case of EFI (Extensible Firmware Interface)
> src/system/boot/platform/efi/start.cpp

* control comes to efi_main function.

efi_main(efi_handle image, [efi_system_table](/boot/efi_table.md) *systemTable)

It does the following things : -
* [console_init()](/boot/efi/console.md) =
* [serial_init()](/boot/efi/serial.md) =
* [serial_enable()](/boot/efi/serial.md#serial_enable) =
* [console_check_boot_keys()](/boot/efi/console.md#console_check_boot_keys) =
* [cpu_init()](/boot/efi/cpu.md#cpu_init) = 
* [acpi_init()](/boot/efi/acpi.md#acpi_init) =
* #ifdef _BOOT_FDT_SUPPORT
	dtb_init();
* [timer_init()](/boot/efi/timer.md#timer_init) =
* [smp_init()](/boot/efi/smp.md#smp_init) =
* main(&args);

```
efi_main(efi_handle image, efi_system_table *systemTable)
{
	stage2_args args;

	memset(&args, 0, sizeof(stage2_args));

	kImage = image;
	kSystemTable = systemTable;
	kBootServices = systemTable->BootServices;
	kRuntimeServices = systemTable->RuntimeServices;

	call_ctors();

	console_init();
	serial_init();
	serial_enable();

	sBootOptions = console_check_boot_keys();

	// disable apm in case we ever load a 32-bit kernel...
	gKernelArgs.platform_args.apm.version = 0;

	cpu_init();
	acpi_init();
#ifdef _BOOT_FDT_SUPPORT
	dtb_init();
#endif
	timer_init();
	smp_init();

	main(&args);

	return EFI_SUCCESS;
}

```


boot/loader/main.cpp
* Bootloader gives the control to the main() in the boot/loader/main.cpp.
* It receives the [stage2_args](/boot/structure/stage2_args.md) structure.
```
extern "C" int
main(stage2_args *args)
{
	...
}
```
* heap_init(args)

