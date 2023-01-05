## menu.md

## static-variables
```
// only set while in user_menu()
static Menu* sMainMenu = NULL;
static Menu* sBlocklistRootMenu = NULL;
static BootVolume* sBootVolume = NULL;
static PathBlocklist* sPathBlocklist;
```

## user_menu

```
status_t
user_menu(BootVolume& _bootVolume, PathBlocklist& _pathBlocklist)
{

	Menu* menu = new(std::nothrow) Menu(MAIN_MENU);

	sMainMenu = menu;
	sBootVolume = &_bootVolume;
	sPathBlocklist = &_pathBlocklist;

	Menu* safeModeMenu = NULL;
	Menu* debugMenu = NULL;
	MenuItem* item;

	TRACE(("user_menu: enter\n"));

	// Add boot volume
	menu->AddItem(item = new(std::nothrow) MenuItem("Select boot volume",
		add_boot_volume_menu()));

	// Add safe mode
	menu->AddItem(item = new(std::nothrow) MenuItem("Select safe mode options",
		safeModeMenu = add_safe_mode_menu()));

	// add debug menu
	menu->AddItem(item = new(std::nothrow) MenuItem("Select debug options",
		debugMenu = add_debug_menu()));

	// Add platform dependent menus
	platform_add_menus(menu);

	menu->AddSeparatorItem();

	menu->AddItem(item = new(std::nothrow) MenuItem("Reboot"));
	item->SetTarget(user_menu_reboot);
	item->SetShortcut('r');

	menu->AddItem(item = new(std::nothrow) MenuItem("Continue booting"));
	if (!_bootVolume.IsValid()) {
		item->SetLabel("Cannot continue booting (Boot volume is not valid)");
		item->SetEnabled(false);
		menu->ItemAt(0)->Select(true);
	} else
		item->SetShortcut('b');

	menu->Run();

	apply_safe_mode_options(safeModeMenu);
	apply_safe_mode_options(debugMenu);
	apply_safe_mode_path_blocklist();
	add_safe_mode_settings(sSafeModeOptionsBuffer.String());
	delete menu;


	TRACE(("user_menu: leave\n"));

	sMainMenu = NULL;
	sBlocklistRootMenu = NULL;
	sBootVolume = NULL;
	sPathBlocklist = NULL;
	return B_OK;
}
```

### working
+ user_menu(BootVolume& _bootVolume, PathBlocklist& _pathBlocklist)
	+ [Menu](#Menu-Constructor)* menu = new (std::nothrow) Menu(MAIN_MENU);	// create an object of Menu class.
	+ [sMainMenu](#static-variables) = menu; 	// set the sMainMenu ptr of Menu to the menu ptr.
	+ [sBootVolume](#static-variables) = &_bootVolume;	// set the sBootVolume ptr of BootVolume to the received _bootVolume address.
	+ [sPathBlocklist](#static-variables) = &_pathBlocklist;	// set the sPathBlockList ptr of PathBlockList to the received _pathBlockList.

	+ [Menu](#Menu-Constructor)* safeModeMenu = NULL;	// create a menu ptr and set to NULL, it is not pointing to anything.
	+ [Menu](#Menu-Constructor)* debugMenu = NULL;	// create a menu ptr and set to NULL.
	+ [MenuItem](#MenuItem-Constructor)* item;	// create an ptr to MenuItem.
	+ TRACE(("user_menu: enter\n"));	// for the logging purpose.
	+ // Add boot volume
	+ menu->AddItem(item = new(std::nothrow) MenuItem("Select boot volume",
		add_boot_volume_menu())); // [AddItem](#Menu-AddItem), [MenuItem Constructor](#MenuItem-Constructor)
	+ // Add safe mode
	+ menu->AddItem(item = new(std::nothrow) MenuItem("Select safe mode options",
		safeModeMenu = add_safe_mode_menu()));

	+ // add debug menu
	+ menu->AddItem(item = new(std::nothrow) MenuItem("Select debug options",
		debugMenu = add_debug_menu()));

	+ // Add platform dependent menus
	+ [platform_add_menus(menu);](/boot/efi/menu.md#platform_add_menus)

	+ menu->AddSeparatorItem();

	+ menu->AddItem(item = new(std::nothrow) MenuItem("Reboot"));
	+ item->SetTarget(user_menu_reboot);
	+ item->SetShortcut('r');

	+ menu->AddItem(item = new(std::nothrow) MenuItem("Continue booting"));
	+ if (!_bootVolume.IsValid()) {
		+ item->SetLabel("Cannot continue booting (Boot volume is not valid)");
		+ item->SetEnabled(false);
		+ menu->ItemAt(0)->Select(true);
	+ } else
		+ item->SetShortcut('b');

	+ [menu->Run();](#Menu-Run)

	+ apply_safe_mode_options(safeModeMenu);
	+ apply_safe_mode_options(debugMenu);
	+ apply_safe_mode_path_blocklist();
	+ add_safe_mode_settings(sSafeModeOptionsBuffer.String());
	+ delete menu;

	+ TRACE(("user_menu: leave\n"));

	+ sMainMenu = NULL;
	+ sBlocklistRootMenu = NULL;
	+ sBootVolume = NULL;
	+ sPathBlocklist = NULL;
	+ return B_OK;



## Menu

## menu_type

> Defined in headers/private/kernel/boot/menu.h

```
enum menu_type {
	MAIN_MENU = 1,
	SAFE_MODE_MENU,
	STANDARD_MENU,
	CHOICE_MENU,
};
```

## Menu-Constructor

```
Menu::Menu(menu_type type, const char* title)
	:
	fTitle(title),
	fChoiceText(NULL),
	fCount(0),
	fIsHidden(true),
	fType(type),
	fSuperItem(NULL),
	fShortcuts(NULL)
{
}
```

## Menu-AddItem

```
void
Menu::AddItem(MenuItem* item)
{
	item->fMenu = this;
	fItems.Add(item);
	fCount++;
}
```

## Menu-Run

```
void
Menu::Run()
{
	platform_run_menu(this);
}
```
+ [platform_run_menu(this)](/boot/efi/menu.md#platform_run_menu)


## MenuItem

## MenuItem-Constructor

```
MenuItem::MenuItem(const char *label, Menu *subMenu)
	:
	fLabel(strdup(label)),
	fTarget(NULL),
	fIsMarked(false),
	fIsSelected(false),
	fIsEnabled(true),
	fType(MENU_ITEM_STANDARD),
	fMenu(NULL),
	fSubMenu(NULL),
	fData(NULL),
	fHelpText(NULL),
	fShortcut(0)
{
	SetSubmenu(subMenu);
}
```

## MenuItem::SetSubMenu(Menu)

```
void
MenuItem::SetSubmenu(Menu* subMenu)
{
	fSubMenu = subMenu;

	if (fSubMenu != NULL)
		fSubMenu->fSuperItem = this;
}

```
