## start


### platform_boot_options() {#platform_boot_options}

```
static uint32 sBootOptions;

extern "C" uint32
platform_boot_options()
{
   return sBootOptions;
}
```
+ It returns the sBootOptions which is a static variable,


