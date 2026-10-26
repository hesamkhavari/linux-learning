# vi Editor: Modes, Navigation and Editing

## 1. Opening a File
```bash
vi file1.txt
```
Opens `file1.txt` in the `vi` editor. If the file does not exist, it can be created when the buffer is saved.

## 2. The Main Modes
- **Normal mode** : Navigate, delete, copy, paste, and execute editing commands.

- **Insert mode** : Type and edit text.

- **Command-line mode** : Enter commands such as saving, quitting, and searching.

Press `Esc` to return to Normal mode from Insert mode.

## 3. Entering Insert Mode

Command|Action
|---|---|
`i`|Insert before the cursor
`a`|Append after the cursor
`o`|Open a new line below the current line and enter Insert mode
`Esc`|Return to Normal mode

Example:
```bash
i       Insert before the cursor
Esc     Return to Normal mode
a       Insert after the cursor
Esc     Return to Normal mode
o       Create a new line below
Esc     Return to Normal mode
```

## 4. Navigation
In Normal mode :

Key|Action
|---|---|
H|Move left
J|Move down
K|Move up
L|Move right

These keys are useful when `arrow keys` are unavailable or when working in a terminal.

## 5. Deleting and Changing Text

Command|Action
|---|---|
x|Delete the character under the cursor
dw|Delete from the cursor to the end of the current word
dd|Delete the current line
cw|Change text from the cursor through the current word
c|Begin a change operation; requires a motion or text object

A change operation deletes the selected text and enters Insert mode.

For example:
```bash
cw      Change the current word
        Type replacement text
Esc     Return to Normal mode
```

## 6. Copying and Pasting
In vi, copying is commonly called `yanking`.

Command|Action
|---|---|
yy|Yank the current `line`
yw|Yank a `word` from the cursor position
p|Put the yanked or deleted text `after` the cursor, or `below` the current line for linewise text
P|Put text `before` the cursor, or `above` the current line for linewise text

Examples :
```bash
yy      Yank the current line
p       Put the copied line below
dd      Delete the current line
p       Put the deleted line back
```
Deleted text can usually be put back because delete operations also store text in a register.

## 7. Searching Within a File

Command|Action
|---|---|
/pattern|Search forward
?pattern|Search backward
n|Repeat the last search in the same direction
N|Repeat the last search in the opposite direction

Examples :
```bash
/bad
```
Searches forward for bad.

```bash
?not
```
Searches backward for not.

After a search, press `n` or `N` to move between matches.

## 8. Saving and Quitting
Enter these commands in Normal mode by typing : and then the command.

Command|Action
|---|---|
:w|Write changes to the current file
:w file2.txt|Write the buffer to file2.txt
:q|Quit if there are no unsaved changes
:wq|Save and quit
ZZ|Save and quit if changes need saving
:q!|Quit without saving changes
:e!|Reload the current file, discarding unsaved changes

Caution: `:e!` discards unsaved edits. Use it only when you intend to lose those changes.

`:w!` attempts to force writing despite certain editor-level restrictions, but it does not bypass operating-system permissions.

## 9. Example Practice Session
```bash
vi file1.txt
```
1. Inside vi :

2. Press i and enter some text.

3. Press Esc.

4. Press o to create a line below and enter more text.

5. Press Esc.

6. Move to a line and press yy to copy it.

7. Press p to paste the line below.

8. Search using /bad and press n to find the next match.

9. Type :w and press Enter to save.

10. Type :q and press Enter to quit.

## Quick Reference

Task|Command
|-----|-----|
Insert before cursor|i
Append after cursor|a
Open line below|o
Delete character|x
Delete word|dw
Delete line|dd
Change word|cw
Copy line|yy
Paste|p
Search forward|/pattern
Search backward|?pattern
Save|:w
Save and quit|:wq
Quit without saving|:q!
Discard changes and reload|:e!

Key takeaway: Learn the distinction between Normal mode, Insert mode, and Command-line mode first. 
Most common vi editing tasks become easier once switching between these modes feels natural.
