## UEFI

The Unified Extensible Firmware Interface (UEFI) is a publicly available specification that defines a software interface between an operating system and platform firmware. It runs when the computer is booted. It initializes the hardware and loads the operating system into the memory.

UEFI replaces the Basic Input Output System (BIOS) firmware interface originally present in all IBM PC-compatible personal computers, with most UEFI firmware implementations support for BIOS services. UEFI can support remote diagnostics and repair computers, even with no operating system installed.

UEFI has many new features and advantages that cannot be achieved through the traditional BIOS, and it is aimed to replace the BIOS in the future completely. UEFI stores all the information about initialization and startup in a .efi file, a file stored on a special partition called EFI System Partition (ESP).

The ESP partition will also contain the boot loader programs for the operating system installed on the computer. Because of this partition, UEFI can directly boot the operating system and save the BIOS self-test process, which is important for UEFI's faster booting.

> What does UEFI do?

UEFI defines a new method of communicating OS and platform firmware, providing a lightweight BIOS alternative that uses only the information needed to launch the OS boot process. UEFI provides enhanced computer security features and supports most existing BIOS systems with backward compatibility.

UEFI is programmable, enabling original equipment manufacturer developers to add applications, drivers, and UEFI to function as a lightweight OS. UEFI contains platform-related data tables, boot, and runtime service calls the OS loader uses. This information defines the required interfaces and structures that must be implemented for firmware and hardware devices to support UEFI.

BIOS is generally considered a vestige from earlier computing, whereas UEFI is regarded as the wave of the future. However, for ease of understanding, some information technology users refer to the processes collectively as UEFI BIOS, despite their substantial differences.

> What makes BIOS Outdated?

Present in all IBM PC-compatible personal computers, BIOS has been around since the late 1970s. Since then, it has incorporated some major improvements, such as adding a user interface and advanced power management functions, allowing BIOS to configure the PCs and easily create better power management plans. But still, it has not advanced as much as computer hardware and software technology since the 70s.

### Limitations of BIOS

BIOS have the following limitations, such as:
* BIOS can boot from drives of less than 2 TB. 3 TB or above drives are now standard, and a system with a BIOS can't boot from them.
* The BIOS must run in 16-bit processor mode and only has 1 MB of space to execute in. It has trouble initializing multiple hardware devices at once, which leads to a slower boot process when initializing all the hardware interfaces and devices on a modern PC.
* It cannot initialize multiple hardware devices at once, thus leading to a slow booting process.

---

### UEFI entry point
*  This routine is the entry point for the EFI Boot Application.

```
/*
Arguments:
    ImageHandle - Supplies a pointer to the image handle.
    SystemTable - Supplies a pointer to the EFI system table.
Return Value:
    EFI status code.
*/

EFI_STATUS
efi_main (EFI_HANDLE ImageHandle, EFI_SYSTEM_TABLE *SystemTable){

// do initilization.

}
```

### Clear the screen

This will clear the complete screen.

```
SystemTable->ConOut->ClearScreen(SystemTable->ConOut);

```

### Output to screen

This will print the string to the screen.

```
SystemTable->ConOut->ClearScreen(SystemTable->ConOut, (char16_t*)L"Welcome to Boot Manager\r\n");

```
