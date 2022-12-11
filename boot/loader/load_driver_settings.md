## load driver settings

## add_stage2_driver_settings(stage2_args* args)

```
status_t
add_stage2_driver_settings(stage2_args* args)
{
	// TODO: split more intelligently
	for (const char** arg = args->arguments;
			arg != NULL && args->arguments_count-- && arg[0] != NULL; arg++) {
		//dprintf("adding args: '%s'\n", arg[0]);
		add_safe_mode_settings((char*)arg[0]);
	}
	return B_OK;
}
```
