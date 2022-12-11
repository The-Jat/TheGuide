## dtb


## dtb_init()

```
void
dtb_init()
{
	efi_configuration_table *table = kSystemTable->ConfigurationTable;
	size_t entries = kSystemTable->NumberOfTableEntries;

	INFO("Probing for device trees from UEFI...\n");

	// Try to find an FDT
	for (uint32 i = 0; i < entries; i++) {
		if (!table[i].VendorGuid.equals(DEVICE_TREE_GUID))
			continue;

		void* dtbPtr = (void*)(table[i].VendorTable);

		int res = fdt_check_header(dtbPtr);
		if (res != 0) {
			ERROR("Invalid FDT from UEFI table %d: %s\n", i, fdt_strerror(res));
			continue;
		}

		sDtbTable = dtbPtr;
		sDtbSize = fdt_totalsize(dtbPtr);
	
		INFO("Valid FDT from UEFI table %d, size: %" B_PRIu32 "\n", i, sDtbSize);

#ifdef TRACE_DUMP_FDT
		dump_fdt(sDtbTable);
#endif

		dtb_handle_chosen_node(sDtbTable);

		int node = -1;
		int depth = -1;
		while ((node = fdt_next_node(sDtbTable, node, &depth)) >= 0 && depth >= 0) {
			dtb_handle_fdt(sDtbTable, node);
		}
		break;
	}
}

```
