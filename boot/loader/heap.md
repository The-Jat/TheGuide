## heap

## what is heap

+ Heaps are memory areas allocated to each program. Memory allocated to heaps can be dynamically allocated, unlike memory allocated to stacks.
+ Heaps memory is allocated during the execution of programmers' instructions. It is crucial to highlight that the name heap has nothing to do with the heap data structure. It is termed a heap because it is a collection of memory space that programmers can allocate and deallocate.

 The memory is allocated during the execution of instructions written by programmers. Note that the name heap has nothing to do with the heap data structure. It is called a heap because it is a pile of memory space available to programmers to allocate and de-allocate. Every time when we made an object it always creates in Heap-space and the referencing information to these objects is always stored in Stack-memory. Heap memory allocation isn’t as safe as Stack memory allocation because the data stored in this space is accessible or visible to all threads. If a programmer does not handle this memory well, a memory leak can happen in the program.
 
 Heap is a memory region allotted to every program. Unlike stack, heap memory can be dynamically allocated. This means that the program can 'request' and 'release' memory from the heap segment whenever it requires. Also, this memory is global, i.e. it can be accessed and modified from anywhere within a program and is not localized to the function where it is allocated. This is accomplished using 'pointers' to reference dynamically allocated memory which in turn leads to a small degradation in performance as compared to using local variables (on the stack).
 
 ```
 int func()
{
    // Nothing allocated yet excluding the
    // pointer itself, which is allocated
    // here on the stack.
    char* p;
  
    // Memory allocated on the stack.
    bool flag = true;
  
    if (flag) {
  
        // Create 1000 bytes on the stack
        char buffer[1000];
  
        // Create 1000 bytes on the heap
        p = new char[1000];
    }
  
    // buffer is deallocated here but pointer
    // p is not Here occurs a memory leak,
    // We have to call delete[] p;
}
 
 ```
 
 ## Advantages heap memory:

+ Heap doesn’t have any limit on memory size.
+ It allows you to access variables globally.
+ Garbage collection runs on the heap memory to free the memory used by the object.


## heap_init()

> arg1 = [stage2_args](/boot/structure/stage2_args.md)* args

```
status_t
heap_init(stage2_args* args)
{
	void* base;
	void* top;
	if (platform_init_heap(args, &base, &top) < B_OK)
		return B_ERROR;

	sHeapBase = base;
	sHeapEnd = top;
	sMaxHeapSize = (uint8*)top - (uint8*)base;

	// declare the whole heap as one chunk, and add it
	// to the free list
	FreeChunk* chunk = (FreeChunk*)base;
	chunk->SetTo(sMaxHeapSize);
	sFreeChunkTree.Insert(chunk);

	sAvailable = chunk->Size();
#ifdef DEBUG_MAX_HEAP_USAGE
	sMaxHeapUsage = sMaxHeapSize - sAvailable;
#endif

	if (sLargeAllocations.Init(64) != B_OK)
		return B_NO_MEMORY;

	return B_OK;
}


```

* void* base;
* void* top;
* if ([platform_init_heap(args, &base, &top)](/boot/efi/heap.md#platform_init_heap)) < B_OK)
	return B_ERROR;
* sHeapBase = base;	// sHeapBase is the static global variable which points to start of the heap
* sHeapEnd = top;	// sHeapEnd is the static global variable which points to top of the heap
* sMaxHeapSize = (uint8*)top - (uint8*)base;	// sMaxHeapSize static variable which is the size of heap

* // declare the whole heap as one chunk, and add it
* // to the free list
* FreeChunk* chunk = (FreeChunk*)base;
* chunk->SetTo(sMaxHeapSize);
* sFreeChunkTree.Insert(chunk);

* sAvailable = chunk->Size();
* #ifdef DEBUG_MAX_HEAP_USAGE
	* sMaxHeapUsage = sMaxHeapSize - sAvailable;
* #endif

* if (sLargeAllocations.Init(64) != B_OK)
	* return B_NO_MEMORY;

* return B_OK;
