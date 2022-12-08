
## arch timer
> location = src/system/boot/platform/efi/arch/x86_64/arch_timer.cpp

## arch_timer_init()
 ```

void
arch_timer_init(void)
{
	calculate_cpu_conversion_factor(2);
	hpet_init();
}

```
