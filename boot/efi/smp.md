

## Smp
> location = src/system/boot/platform/efi/smp.cpp

## smp_init()

```

void
smp_init(void)
{
#if NO_SMP
	gKernelArgs.num_cpus = 1;
	return;
#endif

	arch_smp_init();
}

```

* [arch_smp_init()](/boot/arch/x86/arch_smp.md#arch_smp_init)
