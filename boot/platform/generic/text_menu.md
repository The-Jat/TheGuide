## text_menu

## platform_generic_run_text_menu

> arg1 = Menu *menu

```
void
platform_generic_run_text_menu(Menu *menu)
{
//	platform_switch_to_text_mode();

	run_menu(menu);

//	platform_switch_to_logo();
}
```

* [run_menu(menu)](#run_menu)


## make_item_visible

> arg1 = Menu *menu

> arg2 = int32 selected

```
static bool
make_item_visible(Menu *menu, int32 selected)
{
	if (sMenuOffset > selected
		|| sMenuOffset + menu_height() <= selected) {
		if (sMenuOffset > selected)
			sMenuOffset = selected;
		else
			sMenuOffset = selected + 1 - menu_height();

		draw_menu(menu);
		return true;
	}

	return false;
}

```

## draw_menu

> arg1 = Menu *menu

```
static void
draw_menu(Menu *menu)
{
	console_set_color(kTextColor, kBackgroundColor);
	console_clear_screen();

	print_centered(1, "Welcome to the");
	print_centered(2, "Haiku Boot Loader");

	console_set_color(kCopyrightColor, kBackgroundColor);
	print_right(console_height() - 1, get_haiku_revision());

	console_set_color(kCopyrightColor, kBackgroundColor);
	print_centered(4, "Copyright 2004-2022 Haiku, Inc.");

	if (menu->Title()) {
		console_set_cursor(kOffsetX, kFirstLine - 2);
		console_set_color(kTitleColor, kTitleBackgroundColor);

		printf(" %s", menu->Title());
		print_spacing(console_width() - 1
			- strlen(menu->Title()) - 2 * kOffsetX);
	}

	MenuItemIterator iterator = menu->ItemIterator();
	MenuItem *item;
	int32 i = 0;

	while ((item = iterator.Next()) != NULL) {
		if (item->Type() == MENU_ITEM_SEPARATOR) {
			putchar('\n');
			i++;
			continue;
		}

		print_item_at(i++, item, false);
	}

	int32 height = menu_height();
	if (menu->CountItems() >= height) {
		int32 x = console_width() - kOffsetX;
		console_set_cursor(x, kFirstLine);
		console_set_color(kArrowColor, kBackgroundColor);
		putchar(30/*24*/);
		height--;

		int32 start = sMenuOffset * height / menu->CountItems();
		int32 end = (sMenuOffset + height) * height / menu->CountItems();

		for (i = 1; i < height; i++) {
			console_set_cursor(x, kFirstLine + i);
			if (i >= start && i <= end)
				console_set_color(WHITE, kSliderColor);
			else
				console_set_color(WHITE, kSliderBackgroundColor);

			putchar(' ');
		}

		console_set_cursor(x, kFirstLine + i);
		console_set_color(kArrowColor, kBackgroundColor);
		putchar(31/*25*/);
	}
}
```

## run_menu

> arg1 = Menu* menu

```
static void
run_menu(Menu* menu)
{
	sMenuOffset = 0;
	menu->Entered();
	menu->Show();

	draw_menu(menu);

	// Get selected entry, or select the last one, if there is none
	int32 selected;
	MenuItem *item = menu->FindSelected(&selected);
	if (item == NULL) {
		selected = 0;
		item = menu->ItemAt(selected);
		if (item != NULL)
			item->Select(true);
	}

	make_item_visible(menu, selected);

	while (true) {
		int key = console_wait_for_key();

		item = menu->ItemAt(selected);

		if (TEXT_CONSOLE_IS_CURSOR_KEY(key) || key == 'j' || key == 'J'
			|| key == 'k' || key == 'K') {
			if (item == NULL)
				continue;

			int32 oldSelected = selected;

			switch (key) {
				case TEXT_CONSOLE_KEY_UP:
				case 'k':
				case 'K':
					selected = select_previous_valid_item(menu, selected - 1);
					break;
				case TEXT_CONSOLE_KEY_DOWN:
				case 'j':
				case 'J':
					selected = select_next_valid_item(menu, selected + 1);
					break;
				case TEXT_CONSOLE_KEY_PAGE_UP:
				case TEXT_CONSOLE_KEY_LEFT:
					selected = select_previous_valid_item(menu,
						selected - menu_height() + 1);
					break;
				case TEXT_CONSOLE_KEY_PAGE_DOWN:
				case TEXT_CONSOLE_KEY_RIGHT:
					selected = select_next_valid_item(menu,
						selected + menu_height() - 1);
					break;
				case TEXT_CONSOLE_KEY_HOME:
					selected = first_selectable_item(menu);
					break;
				case TEXT_CONSOLE_KEY_END:
					selected = last_selectable_item(menu);
					break;
			}

			// check if selected has changed
			if (selected != oldSelected) {
				MenuItem *item = menu->ItemAt(selected);
				if (item != NULL)
					item->Select(true);

				make_item_visible(menu, selected);
				// make sure that the new selected entry is visible
				if (sMenuOffset > selected
					|| sMenuOffset + menu_height() <= selected) {
					if (sMenuOffset > selected)
						sMenuOffset = selected;
					else
						sMenuOffset = selected + 1 - menu_height();

					draw_menu(menu);
				}
			}
		} else if (key == TEXT_CONSOLE_KEY_RETURN
			|| key == TEXT_CONSOLE_KEY_SPACE) {
			if (item != NULL && invoke_item(menu, item, selected, key))
				break;
		} else if (key == '\t') {
			if (item == NULL)
				continue;

			int32 oldSelected = selected;

			// Use tab to cycle between items (on some platforms, arrow keys
			// are not available)
			selected = select_next_valid_item(menu, selected + 1);

			if (selected == oldSelected)
				selected = first_selectable_item(menu);

			// check if selected has changed
			if (selected != oldSelected) {
				MenuItem *item = menu->ItemAt(selected);
				if (item != NULL)
					item->Select(true);

				make_item_visible(menu, selected);
				// make sure that the new selected entry is visible
				if (sMenuOffset > selected
					|| sMenuOffset + menu_height() <= selected) {
					if (sMenuOffset > selected)
						sMenuOffset = selected;
					else
						sMenuOffset = selected + 1 - menu_height();

					draw_menu(menu);
				}
			}
		} else if (key == TEXT_CONSOLE_KEY_ESCAPE
			&& menu->Type() != MAIN_MENU) {
			// escape key was hit
			break;
		} else {
			// Shortcut processing
			shortcut_hook function = menu->FindShortcut(key);
			if (function != NULL)
				function(key);
			else {
				item = menu->FindItemByShortcut(key);
				if (item != NULL && invoke_item(menu, item, selected,
						TEXT_CONSOLE_KEY_RETURN)) {
					break;
				}
			}
		}
	}

	menu->Hide();
	menu->Exited();
}
```
* [draw_menu(menu)](#draw_menu)
* [make_item_visible(menu, selected)](#make_item_visible)
* [console_wait_for_key()](/boot/efi/console.md#console_wait_for_key)
