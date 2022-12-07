
### serial

> src/system/boot/platform/efi/serial.cpp

```

extern "C" void
serial_init(void)
{
	efi_status status = kSystemTable->BootServices->LocateProtocol(
		&sSerialIOProtocolGUID, NULL, (void**)&sSerial);

	if (status != EFI_SUCCESS || sSerial == NULL) {
		sSerial = NULL;
		return;
	}

	// Setup serial, 0, 0 = Default Receive FIFO queue and default timeout
	status = sSerial->SetAttributes(sSerial, kSerialBaudRate, 0, 0, NoParity, 8,
		OneStopBit);

	if (status != EFI_SUCCESS) {
		sSerial = NULL;
		return;
	}
}

```
