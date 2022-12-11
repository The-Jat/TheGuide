## heap

## heap_init()

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

* [platform_init_heap(args, &base, &top)]()
