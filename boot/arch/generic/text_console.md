
## text_console


// This is set by the boot platform during console_init().
ConsoleNode* gConsoleNode;


## console_hide_cursor

```
void
console_hide_cursor(void)
{
	gConsoleNode->SetCursorVisible(false);
}

```
