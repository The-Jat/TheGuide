>### Booting

----

##### In case of EFI (Extensible Firmware Interface)
> src/system/boot/platform/efi/start.cpp

* control comes to efi_main function.

efi_main(efi_handle image, [efi_system_table](/boot/efi_table.md) *systemTable)

```
efi_main(efi_handle image, efi_system_table *systemTable)

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

