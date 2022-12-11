## heap

## platform_init_heap

```
extern "C" status_t
platform_init_heap(struct stage2_args *args, void **_base, void **_top)
{
	if (kBootServices->AllocatePages(AllocateAnyPages, EfiLoaderData,
			STAGE_PAGES, &staging) != EFI_SUCCESS)
		return B_NO_MEMORY;

	*_base = (void*)staging;
	*_top = (void*)((int8*)staging + STAGE_PAGES * B_PAGE_SIZE);

	return B_OK;
}

```
