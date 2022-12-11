
## text_console



ConsoleNode* gConsoleNode;	// This is set by the boot platform during console_init().


## console_hide_cursor

```
void
console_hide_cursor(void)
{
	gConsoleNode->SetCursorVisible(false);
}

```
