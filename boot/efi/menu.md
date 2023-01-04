## menu

## platform_add_menus()

> arg1 = Menu *menu

```
void
platform_add_menus(Menu *menu)
{
	MenuItem *item;

	switch (menu->Type()) {
		case MAIN_MENU:
			item = new(std::nothrow)MenuItem("Select video mode", video_mode_menu());
			if (item != NULL) {
				menu->AddItem(item);
				item->SetTarget(video_mode_hook);
				item->SetShortcut('v');
			}

			break;

		case SAFE_MODE_MENU:
			item = new(std::nothrow) MenuItem("Use fail-safe graphics driver");
			if (item != NULL) {
				menu->AddItem(item);
				item->SetType(MENU_ITEM_MARKABLE);
				item->SetData(B_SAFEMODE_FAIL_SAFE_VIDEO_MODE);
				item->SetHelpText("The system will use VESA mode "
					"and won't try to use any video graphics drivers.");
			}

			smp_add_safemode_menus(menu);

			item = new(std::nothrow) MenuItem("Disable ACPI");
			if (item != NULL) {
				menu->AddItem(item);
				item->SetType(MENU_ITEM_MARKABLE);
				item->SetData(B_SAFEMODE_DISABLE_ACPI);
				item->SetHelpText("Disables Advanced Configuration and Power "
					"Interface hardware support, overriding the ACPI setting "
					"in the kernel settings file.");
			}
			break;

		default:
			break;
	}
}
```


## platform_run_menu

> arg1 = Menu *menu

```
void
platform_run_menu(Menu *menu)
{
	platform_generic_run_text_menu(menu);
}
```

* platform_generic_run_text_menu(menu)
