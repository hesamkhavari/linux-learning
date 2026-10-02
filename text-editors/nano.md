# Nano Text Editor
## Open a File
```bash
nano filename
```
Open an empty editor:
```bash
nano
```
Find the executable:
```bash
which nano
type nano
```
### Useful Shortcuts
|Shortcut|	Function|
|:---|:---|
|Ctrl+X|	Exit|
|Ctrl+O|	Write/save file|
|Ctrl+W|	Search|
|Ctrl+K|	Cut line|
|Ctrl+U|	Paste previously cut text|
|Ctrl+G|	Help|
|Ctrl+R|	Read/insert another file|
|Ctrl+\|	Search and replace|
|Alt+A|	Start/stop text selection|
|Alt+^|	Copy selected text|
---
### Open at a Specific Line
```bash
nano +5 name.txt
```
Opens the file near line 5.
### Read-Only Mode
```bash
nano -v name.txt
```
