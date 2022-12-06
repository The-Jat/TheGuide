>### Booting

----
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

