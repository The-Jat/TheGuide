## arch_smp

> location = src/system/boot/platform/efi/arch/x86/arch_smp.cpp

## arch_smp_init()

```

void
arch_smp_init(void)
{
	cpuid_info info;
	if (get_current_cpuid(&info, 1, 0) != B_OK)
		return;

	if ((info.eax_1.features & IA32_FEATURE_APIC) == 0) {
		// Local APICs aren't present; As they form the basis for all inter CPU
		// communication and therefore SMP, we don't need to go any further.
		TRACE("no local APIC present, not attempting SMP init\n");
		return;
	}

	// first try to find ACPI tables to get MP configuration as it handles
	// physical as well as logical MP configurations as in multiple cpus,
	// multiple cores or hyper threading.
	if (acpi_do_smp_config() == B_OK) {
		TRACE("smp init success\n");
		return;
	}

	// Everything failed or we are not running an SMP system, reset anything
	// that might have been set through an incomplete configuration attempt.
	gKernelArgs.arch_args.apic_phys = 0;
	gKernelArgs.arch_args.ioapic_phys = 0;
	gKernelArgs.num_cpus = 1;
}

```
