# Regular Expressions with grep and sed

## 1. Searching Text with grep
`grep` searches input lines for a pattern and prints matching lines.
```bash
grep 'oo' file1.txt
grep -n 'oo' file1.txt
grep -i 'goo' file1.txt
```

- `-n` : displays line numbers.

- `-i` : ignores case differences.

- By default, grep uses Basic Regular Expressions (BRE).

> Use quotes around patterns to prevent the shell from interpreting special characters.

## 2. Anchors: Beginning and End of a Line
```bash
grep '^o' file1.txt
grep '^g' file1.txt
grep -i '^g' file1.txt
grep 's$' file1.txt
```

- `^` : matches the beginning of a line.

- `$` : matches the end of a line.

Examples :

Pattern|Meaning
|---|---|
`^o`|Line begins with lowercase o
`^g`|Line begins with lowercase g
`s$`|Line ends with s
`^Google$`|Line contains exactly Google

The anchors match positions, not literal `^` or `$` characters in these contexts.

## 3. The Dot Metacharacter
```bash
grep '.o' file1.txt
```
In a regular expression, `.` matches any single character except a newline in ordinary grep line-based matching.

Therefore, `.o` matches a character followed by o, such as `oo`, `go`, or `Zo`.

To search for a literal dot, escape it :
```bash
grep '\.' file1.txt
```

## 4. Character Classes and Ranges
```bash
grep '[og]' file1.txt
grep '^[a-g]' file1.txt
grep '^[A-G]' file1.txt
grep -i '^[a-g]' file1.txt
```

- `[og]` : matches one character, either o or g.

- `[a-g]` : matches one character in the range a through g.

- `[^a-g]` : matches one character not in that range, when used as a bracket expression.

For example :
```bash
grep '^[^a-g]' file1.txt
```

Matches lines whose first character is not in the specified range.

Important: Inside `[og]`, the characters are alternatives. 
The | character inside a bracket expression is normally treated literally, not as an alternation operator.

## 5. Alternation with Extended Regular Expressions
Use `grep -E` for Extended Regular Expressions (ERE) :
```bash
grep -E '^(o|g)' file1.txt
grep -Ei '^(o|g)' file1.txt
grep -E '^(g|G)' file1.txt
```

- `-E` : enables ERE syntax.

- `(o|g)` : matches either o or g.

- `|` : means alternation in ERE.

- `-i` : makes matching case-insensitive.

The following is an equivalent way to match a line beginning with o or g :
```bash
grep '^[og]' file1.txt
```
For simple single-character alternatives, a character class is often shorter and easier to read.

> `egrep` is the historical name for `grep -E`; prefer grep -E in new notes and scripts.

## 6. Fixed-String Search
```bash
grep -F 's$' file1.txt
grep -F '^' file1.txt
```
`-F` treats the pattern as literal text, not as a regular expression.

Therefore :

- `grep 's$' file1.txt` matches lines ending in s.

- `grep -F 's$' file1.txt` searches for the literal characters s$.

- `grep -F '^' file1.txt` searches for a literal caret.

> `fgrep` is the historical equivalent of `grep -F`; prefer grep -F.

## 7. Searching Command Output
```bash
ls | grep 'fi'
```
The output from `ls` is passed to `grep`, which prints lines containing fi.

For scripts and reliable file processing, avoid parsing ls output when filenames may contain spaces or unusual characters. 
Prefer tools such as `find` when you need to search filenames.

## 8. Substitution with sed
`sed` is a stream editor that can transform text as it processes input.
```bash
sed 's/oo/00/' file1.txt
```
The substitution syntax is :
```bash
s/pattern/replacement/flags
```
By default, the first match on each line is replaced.

```bash
sed 's/oo/00/g' file1.txt
```
The `g` flag replaces all matches on each line.

Write the modified output to a new file:
```bash
sed 's/oo/00/g' file1.txt > newfile1.txt
```
This leaves `file1.txt` unchanged.

## 9. Using Extended Regular Expressions in sed
```bash
sed -E 's/^(g|G)/U/' file1.txt
```
This replaces a leading `g` or `G` with `U`.

Alternatively, GNU sed supports case-insensitive matching with the `I` flag:
```bash
sed 's/^g/U/I' file1.txt
```
The `I` flag is a GNU extension and may not be supported by every sed implementation.

## 10. Practical Examples
Assuming `file1.txt` contains the sample words from the exercise:
```bash
# Find lines containing "oo"
grep 'oo' file1.txt

# Find matches regardless of case
grep -i 'google' file1.txt

# Find lines beginning with "o" or "g"
grep -E '^(o|g)' file1.txt

# Find lines ending in "s"
grep 's$' file1.txt

# Replace the first "oo" on each line
sed 's/oo/00/' file1.txt

# Replace every "oo" on each line
sed 's/oo/00/g' file1.txt
```

## Quick Reference

Tool or syntax|Purpose
|---|---|
grep|Search using BRE
grep -E|Search using ERE
grep -F|Search for literal text
grep -i|Ignore case
grep -n|Show line numbers
^|Beginning of line
$|End of line
.|Any single character except newline
[abc]|One character from the specified set
[a-z]|One character in a range
`(a\|b)`|Alternation in ERE
sed 's/a/b/'|Replace first match per line
sed 's/a/b/g'|Replace all matches per line

> Key distinction: `grep` primarily searches for matching lines, while `sed` can transform text as it processes the input.
