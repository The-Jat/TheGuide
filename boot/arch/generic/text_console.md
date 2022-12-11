
## text_console



ConsoleNode* gConsoleNode;	// This is set by the boot platform during console_init().


## console_clear_screen()

```
void
console_clear_screen()
{
	gConsoleNode->ClearScreen();
}
```

## console_hide_cursor()

```
void
console_hide_cursor(void)
{
	gConsoleNode->SetCursorVisible(false);
}

```
