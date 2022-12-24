## heap

The memory is allocated during the execution of instructions written by programmers. Note that the name heap has nothing to do with the heap data structure. It is called a heap because it is a pile of memory space available to programmers to allocate and de-allocate. Every time when we made an object it always creates in Heap-space and the referencing information to these objects is always stored in Stack-memory. Heap memory allocation isn’t as safe as Stack memory allocation because the data stored in this space is accessible or visible to all threads. If a programmer does not handle this memory well, a memory leak can happen in the program.

“Heap” memory, also known as “dynamic” memory, is an alternative to local stack memory. Local memory is quite automatic. Local variables are allocated automatically when a function is called, and they are deallocated automatically when the function exits. Heap memory is different in every way. The programmer explicitly requests that space be allocated, using the new operator in Java or C++. This memory “block” will be of a particular size, ususally determined automatically from the size of the object being created. That memory block (your object) continues to be allocated until something happens that makes it go away. In some languages (noteably, C and C++), an object in heap memory only goes away when the programmer explicitly requests that it be deallocated. So the programmer has much greater control of memory, but with greater responsibility since the memory must now be actively managed. Dropping all references to a memory location without deallocating it is a signficant source of errors in C/C++, and this is so common that it has a name: memory leak. (In fact, many commercial programs implemented in C++ have memory leaks, that will eventually make them crash after being used for a long time.) Java removes this source of errors by handling memory deallocation automatically, using garbage collection. The downside is that garbage collection is a slow process that happens at unpredictable times.

## platform_init_heap

It allocates some memory to be used for dynamic allocation (malloc, new).

This function returns the base and top of the memory to be used for allocation.

Heap just the random memory.

platform_init_heap returns the base and top of the stack by reference.

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
