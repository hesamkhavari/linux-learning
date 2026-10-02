# Vim Text Editor
## Check vi
```bash
type vi
readlink -f /usr/bin/vi
```
Start Vim:
```bash
vim
```
Open a file:
```bash
vim name
```

## Vim Modes
Vim is modal.
### Normal Mode
Used for navigation and commands.
Press:
```
Esc
```
to return to Normal mode.
### Insert Mode
Press:
```
i
```
to insert before the cursor.

## Saving and Exiting
```vim
:w
```
Save.

```vim
:q
```
Quit.

```vim
:wq
```
Save and quit.

```vim
ZZ
```
Save and quit.

## Editing
```
dd      Delete line
yy      Copy line
p       Paste after cursor
P       Paste before cursor
u       Undo
dw      Delete word
cw      Change word
```
## Navigation
```
H   Move left
L   Move right
```
## Search
```vim
:/pattern
```
Example:
```vim
:/car
```
## Execute Shell Command
```vim
:!ls
```
Runs `ls` from inside Vim.
## Open Another File
```bash
:e links.txt
```
