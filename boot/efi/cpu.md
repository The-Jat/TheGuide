
# cpu
> location = src/system/boot/platform/efi/cpu.cpp

## cpu_init

```

void
cpu_init()
{
	gKernelArgs.num_cpus = 1;
		// this will eventually be corrected later on

	boot_arch_cpu_init();
}

```
* boot_arch_cpu_init()
