
### location = headers/private/kernel/platform/efi/system-table.h
```

typedef struct efi_system_table {
    efi_table_header Hdr;
    char16_t* FirmwareVendor;
    uint32_t FirmwareRevision;
    efi_handle ConsoleInHandle;
    efi_simple_text_input_protocol* ConIn;
    efi_handle ConsoleOutHandle;
    efi_simple_text_output_protocol* ConOut;
    efi_handle StandardErrorHandle;
    efi_simple_text_output_protocol* StdErr;
    efi_runtime_services *RuntimeServices;
    efi_boot_services* BootServices;
    size_t NumberOfTableEntries;
    efi_configuration_table *ConfigurationTable;
} efi_system_table;

```
