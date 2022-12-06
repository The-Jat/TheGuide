>### Booting

----
boot/loader/main.cpp
* Bootloader gives the control to the main() in the boot/loader/main.cpp.
* It receives the stage2_args structure.
```
extern "C" int
main(stage2_args *args)
{
	...
}
```

