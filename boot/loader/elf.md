##

## elf_init()

```
void
elf_init()
{
	void* settings = load_driver_settings("kernel");
	if (settings == NULL)
		return;

	sLoadElfSymbols = get_driver_boolean_parameter(settings, "load_symbols",
		false, false);

	unload_driver_settings(settings);
}
```

* void* settings = [load_driver_settings](/boot/loader/load_driver_settings.md#load_driver_settings)("kernel");
