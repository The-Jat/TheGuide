## ACPI

> location = src/system/boot/platform/efi/acpi.cpp

## acpi_init()

```

void
acpi_init()
{
	efi_guid acpi = ACPI_20_TABLE_GUID;
	efi_configuration_table *table = kSystemTable->ConfigurationTable;
	size_t entries = kSystemTable->NumberOfTableEntries;

	// Try to find the ACPI RSDP.
	for (uint32 i = 0; i < entries; i++) {
		if (!table[i].VendorGuid.equals(acpi))
			continue;

		acpi_rsdp *rsdp = (acpi_rsdp *)(table[i].VendorTable);
		if (strncmp((char *)rsdp, ACPI_RSDP_SIGNATURE, 8) == 0)
			TRACE(("acpi_init: found ACPI RSDP signature at %p\n", rsdp));

		if (rsdp != NULL && acpi_check_rsdt(rsdp) == B_OK) {
			gKernelArgs.arch_args.acpi_root = rsdp;
			arch_handle_acpi();
			break;
		}
	}
}

```
