# Regular Expressions
Regular Expressions (Regex) are patterns used to search, match, and process text.
They are commonly used with tools such as:
- `grep`
- `sed`
- `awk`
- `find`
This document covers the basic regular expression pattern practiced with `grep`.
---
## 1. Basic Concept
A regular expression describes a pattern rather than necessarily matching one exact string.
### For example:
```bash
grep "cat " dictionary
```
### searches for the exact sequence:
```Plain text
cat
```
But regular expressions can also describe more flexible patterns.
## 2. Beginning of a Line: ^
The `^` character represents the beginning of a line.
### Example:
```bash
grep "^ran" dictionary
```
### This matches lines that start with:
```Plain text
ran
```
### For example:
```Plain text
ran
random
range
```
The important point is that ran must occur at the beginning of the line.
## 3. End of Line: $
The `$` character represents the end of a line.
### Example:
```bash
grep "ran$" dictionary
```
### This matches lines that end with :
```Plain text
ran
```
### For example:
```Plain text
ran
program ran
```
### But a line such as:
```Plain text
random
```
does not match because it does not end with `ran`.
## 4. Any Single Character: .
The `.` character matches any single character.
### Example:
```bash
grep "a.b" file1
```
### This pattern means:
```Plain text
a
any single character
b
```
### It can therefore match:
```Plain text
aab
acb
a1b
```
The character between a and b can be different.
## 5. Character Classes: [ ]
Square brackets define a character class.
### Example:
```bash
grep "c[aeiou]t" dictionary
```
### This means:
```Plain text
c
one character from a, e, i, o, u
t
```
### Possible matches include:
```Plain text
cat
cet
cit
cot
cut
```
## 6. Character Ranges
A range can be specified inside square brackets.
### For example:
```bash
[a-z]
```
means one lowercase English letter from a through z.
### Example:
```bash
grep "^c[a-z]" dictionary
```
This searches for linees that:
 1. Start with c
 2. Have a lowercase letter as the second character
## 7. Multiple Ranges
Multiple range can be combined.
### Example:
```bash
grep "^c[a-zA-Z]" dictionary
```
This searches for lines that:
 1. Start with c
 2. Have an English alphabetic character as the second character
### Here:
```Plain text
[a-z]
```
means lowercase letters.
### And:
```Plain text
[A-Z]
```
means uppercase letters.
## 8. Matching an Exact Number of Characters
### The following command:
```grep
grep "^...$" dictionary
```
matches lines containing exactly three characters.
### Explanation:
```Plain text
^
```
Beginning of line.
```Plain text
.
```
First character.
```Plain text
.
```
Second character.
```Plain text
.
```
Third character.
```Plain text
$
```
End of line.
### Therefore:
```Plain text
^...$
```
### means:
 The line must contain exactly three characters.
 ## 9. The * Character
 In a basic regular expression, `*` means:
 Zero or more occurrences of the preceding regular expression.
 ### For example:
 ```bash
grep "ab*" file1
```
### The pattern contains:
```Plain text
a
```
### followed by zero or more occurrences of:
```Plain text
b
```
### Therefore it can match:
```Plain text
ab
abb
abbb
```
The important poit is that * applies to the expression immediately before it.
## 10. Combining Regex Elements
Regular expression elements can be combined.
### For example:
```bash
grep "^c[a-d]t" dictionary
```
### Therefore the pattern can match:
```Plain text
cat
cbt
cct
cdt
```
when these string accur at the begining of a line.
## 11. Practical Examples
### Find lines starting with ran
```bash
grep "^ran" dictionary
```
### Find lines ending with ran
```
grep "ran$" dictionary
```
### Find a three-character line
```bash
grep "^...$" dictionary
```
### Find a pattern containing a vowel
```bash
grep "c[aeiou]t" dictionary
```
### Find a character in range
```bash
grep "^c[a-d]t" dictionary
```
## 12. Common Regex Characters
|Pattern|Meaning|
|:---:|:---:|
|^|Beginning of line|
|$|End of line|
|.|Any single character|
|*|Zero or more occurrences of the preceding expression|
|[abc]|One character from a, b, or c|
|[a-z]|One lowercase letter|
|[A-Z]|One uppercase letter|
|[a-zA-Z]|One English alphabetic character|
## 13. Regex vs Wildcards
Regular expressions and shell wildcards are not the same thing.
### For example:
```bash
ls *.txt
```
uses a shell wildcard.
### The `*` means:
Any sequence of characters in a filename.
### But in:
```bash
grep "ab*" file1
```
the `*` is part of a regular expression and means:
 Zero or more occurrences of the preceding b.
The same character can therefore have different meaning depending on the tool and context.
## 14. Basic Regular Expression vs Extended Regular Expression
`grep` normally uses Basic Regular Expressions (BRE).

For more advanced regular expression features, `grep -E` can be used.
### For example:
```bash
grep -E "cat|dog" file1
```
The `|` operator means OR in Extended Regular Expressions.
This document focuses on the basic regular expressions that were practiced during Linux Essentials.

### More advanced regular expression features can be learned later when working with tools such as:
* grep -E
* sed
* awk
## 15. Practical Administration
Regular expressions become useful when searching configuration files, logs, and command output.
### For example:
```bash
grep "^root:" /etc/passwd
```
### This searches for a line beginning with:
```Plain text
root:
```
### Another example:
```bash
grep "error$" logfile
```
### This searches for lines ending with:
```Plain text
error
```
Regex becomes particularly powerful when combined with pipes and other text-processing commands.
## 16. Important Notes
- `^` matches the beginning of a line.
- `$` matches the end of a line.
- `.` matches one character.
- `*` applies to the preceding expression.
- `[ ]` defines a character class.
- `[a-z]` represents a range of lowercase English letters.
- `[A-Z]` represents a range of uppercase English letters.
- Regex syntax depends on the tool being used.
- Shell wildcards and regular expressions are different concepts.

### Regular expressions are an important foundation for Linux command-line text processing and troubleshooting.
