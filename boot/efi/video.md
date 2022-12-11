## video

## platform_init_video

```
extern "C" status_t
platform_init_video(void)
{
	list_init(&sModeList);

	// we don't support VESA modes or EDID
	gKernelArgs.vesa_modes = NULL;
	gKernelArgs.vesa_modes_size = 0;
	gKernelArgs.edid_info = NULL;

	// make a guess at the best video mode to use, and save the mode ID
	// for switching to graphics mode
	efi_status status = kBootServices->LocateProtocol(&sGraphicsOutputGuid,
		NULL, (void **)&sGraphicsOutput);
	if (sGraphicsOutput == NULL || status != EFI_SUCCESS) {
		dprintf("GOP protocol not found\n");
		gKernelArgs.frame_buffer.enabled = false;
		sGraphicsOutput = NULL;
		return B_ERROR;
	}

	size_t bestArea = 0;
	size_t bestDepth = 0;

	TRACE(("looking for best graphics mode...\n"));

	for (size_t mode = 0; mode < sGraphicsOutput->Mode->MaxMode; ++mode) {
		efi_graphics_output_mode_information *info;
		size_t size, depth;
		sGraphicsOutput->QueryMode(sGraphicsOutput, mode, &size, &info);
		size_t area = info->HorizontalResolution * info->VerticalResolution;
		TRACE(("  mode: %lu\n", mode));
		TRACE(("  width: %u\n", info->HorizontalResolution));
		TRACE(("  height: %u\n", info->VerticalResolution));
		TRACE(("  area: %lu\n", area));
		if (info->PixelFormat == PixelRedGreenBlueReserved8BitPerColor) {
			depth = 32;
		} else if (info->PixelFormat == PixelBlueGreenRedReserved8BitPerColor) {
			// seen this in the wild, but acts like RGB, go figure...
			depth = 32;
		} else if (info->PixelFormat == PixelBitMask
			&& info->PixelInformation.RedMask == 0xFF0000
			&& info->PixelInformation.GreenMask == 0x00FF00
			&& info->PixelInformation.BlueMask == 0x0000FF
			&& info->PixelInformation.ReservedMask == 0) {
			depth = 24;
		} else {
			TRACE(("  pixel format: %x unsupported\n",
				info->PixelFormat));
			continue;
		}
		TRACE(("  depth: %lu\n", depth));

		video_mode *videoMode = (video_mode*)malloc(sizeof(struct video_mode));
		if (videoMode != NULL) {
			videoMode->mode = mode;
			videoMode->width = info->HorizontalResolution;
			videoMode->height = info->VerticalResolution;
			videoMode->bits_per_pixel = info->PixelFormat == PixelBitMask ? 24 : 32;
			videoMode->bytes_per_row = info->PixelsPerScanLine * depth / 8;
			add_video_mode(videoMode);
		}

		area *= depth;
		TRACE(("  area (w/depth): %lu\n", area));
		if (area >= bestArea) {
			TRACE(("selected new best mode: %lu\n", mode));
			bestArea = area;
			bestDepth = depth;
			sGraphicsMode = mode;
		}
	}

	if (bestArea == 0 || bestDepth == 0) {
		sGraphicsOutput = NULL;
		gKernelArgs.frame_buffer.enabled = false;
		return B_ERROR;
	}

	gKernelArgs.frame_buffer.enabled = true;
	sModeChosen = false;
	sSettingsLoaded = false;
	return B_OK;
}

```
