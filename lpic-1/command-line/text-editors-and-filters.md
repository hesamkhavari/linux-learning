# Text Editors and Filters

## 1. Creating and Viewing Text Files
Create empty files or update timestamps:
```bash
touch it1.txt it2.txt it3.txt it4.txt
```
Display multiple files :
```bash
cat it1.txt it2.txt it3.txt it4.txt
```
Display line numbers :
```bash
cat -n it1.txt
```
Concatenate files and save the output :
```bash
cat -n it1.txt it2.txt it3.txt it4.txt > it5.txt
```
Display a file page by page :
```bash
less it5.txt
```

- `cat` concatenates files and writes their contents to standard output.

- `>` redirects output to a file and overwrites existing contents.

- `less` is useful for viewing large files without printing everything at once.

- `cat -n` numbers all output lines, including blank lines.

## 2. Extracting Characters and Fields with cut
Extract characters 4 through 7 :
```bash
cut -c 4-7 it1.txt
```
Extract characters 1 and 2 :
```bash
cut -c 1,2 it1.txt
```
Extract the first field using a colon delimiter :
```bash
cut -d ':' -f 1 /etc/passwd
```
Extract the first and third fields :
```bash
cut -d ':' -f 1,3 /etc/passwd
```

- `-c` selects character positions.

- `-d` specifies a field delimiter.

- `-f` selects fields.

- The default field delimiter is a `tab` when -d is omitted.

## 3. Tabs and Text Formatting
### expand and unexpand
Convert tabs to spaces :
```bash
expand it3.txt
```
Convert spaces to tabs where possible :
```bash
unexpand it3.txt
```
Convert all eligible sequences of spaces to tabs :
```bash
unexpand -a it3.txt
```

> These commands are useful when working with text containing tabs and spaces. `unexpand` does not necessarily replace every space in every position.

### fmt
Reformat paragraphs :
```bash
fmt it3.txt
```
Format text to a target width of 7 columns :
```bash
fmt -w 7 it3.txt
```
Format text to a target width of 15 columns :
```bash
fmt -w 15 it3.txt
```

> `fmt` reformats paragraphs by wrapping and joining lines where appropriate. It is not a general-purpose tool for preserving exact column layouts.

## 4. Displaying the Beginning and End of Files
First 10 lines by default :
```bash
head it3.txt
```
First 5 lines :
```bash
head -n 5 it3.txt
```
Last 10 lines by default :
```bash
tail /var/log/syslog
```
Follow new lines added to a file :
```bash
tail -f /var/log/syslog
```
Follow a systemd journal in real time :
```bash
journalctl -f
```

- `head` displays the beginning of a file.

- `tail` displays the end of a file.

- `tail -f` follows appended data, which is useful for monitoring logs.

- `/var/log/syslog` is common on Debian/Ubuntu systems; other distributions may use different log files or rely primarily on the `systemd` journal.

## 5. Viewing Characters and Line Numbers
### od
Display file contents in octal representation :
```bash
od it1.txt
```
Display characters in a readable representation :
```bash
od -c it1.txt
```
> `od` displays file data in different representations.
> It is useful for inspecting tabs, newlines and other characters that are difficult to see in ordinary text output.

### nl
Number non-empty lines by default :
```bash
nl it1.txt
```
Number lines in another file :
```bash
nl it4.txt
```

> Unlike `cat -n`, `nl` supports configurable numbering rules and normally does not number blank lines by default.

## 6. Combining and Joining Text Files
### paste
Combine corresponding lines side by side :
```bash
paste it1.txt it2.txt
```
Use a comma as the delimiter :
```bash
paste -d ',' it1.txt it2.txt
```

> `paste` combines corresponding lines from multiple files, usually separating columns with tabs.

### join
```bash
join file1.txt file2.txt
```

> `join` combines lines from two files based on a common field.
> By default, both files must be sorted by the join field, and the default join field is the first field.

> The original it1.txt and it2.txt examples are not necessarily suitable for `join`;
> use dedicated test files with matching keys and compatible sorting.

## 7. Printing Text with pr
Format a file for paginated printing :
```bash
pr it1.txt
```
Format multiple files :
```bash
pr it1.txt it2.txt it3.txt
```

> `pr` formats text into pages with headers and pagination. It is primarily a formatting utility, not a physical printer command by itself.

## 8. Editing Text Streams with sed
Replace the first occurrence of "bisim" on each matching line :
```bash
sed 's/bisim/walkytalki/' it2.txt
```
Replace the first occurrence of "shansi" :
```bash
sed 's/shansi/chance/' it2.txt
```
Replace every occurrence on each line :
```bash
sed 's/shansi/chance/g' it2.txt
```
Print only lines matching a pattern :
```bash
sed -n '/shansi/p' it2.txt
```
Delete lines matching a pattern from the output :
```bash
sed '/shansi/d' it2.txt
```

- `sed` is a stream editor used to transform or filter text.

- `s/old/new/` substitutes the first matching occurrence on each line.

- Adding `g` makes substitution global within each line.

- By default, `sed` writes the transformed output to standard output; it does not modify the original file unless an in-place option such as `-i` is used.

For example, this command displays modified content without changing the source file:
```bash
sed 's/shansi/chance/g' it2.txt > it2-modified.txt
```
> See the `GNU sed manual` for substitution rules and additional operations.

## 9. Sorting Text with sort
Sort lines alphabetically :
```bash
sort it1.txt
```
Reverse the sort order :
```bash
sort -r it1.txt
```
Randomize line order (GNU sort) :
```bash
sort -R it1.txt
```
Sort numerically :
```bash
sort -n numbers.txt
```
Sort and remove duplicate adjacent lines :
```bash
sort -u it1.txt
```
Sort using a specific field and delimiter :
```bash
sort -t ':' -k 3,3n /etc/passwd
```

- `sort -r` reverses the selected sort order.

- `sort -R` randomizes the order in GNU sort; it is not equivalent to `sort -r`.

- `sort -n` compares numeric values.

- `sort -u` outputs unique sorted lines.

- `-t` sets the field delimiter and `-k` specifies the sort key.

## 10. Splitting Files with split
Split into files containing two lines each :
```bash
split -l 2 it1.txt
```
Split into chunks of two bytes each :
```bash
split -b 2 it1.txt
```
Split a large log into chunks of 1000 lines :
```bash
split -l 1000 /var/log/syslog log-part-
```
> By default, `split` generates output files with a prefix such as **xaa**, **xab**, and so on. You can specify your own prefix, as in the final example.

- `-l` specifies the number of lines per output file.

- `-b` specifies the maximum size of each chunk in bytes.

- Byte-based splitting can divide a line across output files.

## 11. Translating and Deleting Characters with tr
Convert uppercase to lowercase :
```bash
echo "HESAM" | tr 'A-Z' 'a-z'
```
Convert H to h :
```bash
echo "HESAM" | tr 'H' 'h'
```
Map characters position by position :
```bash
echo "HESAM" | tr 'HESAM' '12345'
```
Delete all uppercase S characters :
```bash
echo "HESSAM" | tr -d 'S'
```
Squeeze repeated occurrences of selected characters :
```bash
echo "HESSAAM" | tr -s 'SA'
```

- `tr` translates or deletes individual characters; it does not perform general word or substring replacement.

- `-d` deletes selected characters.

- `-s` squeezes repeated occurrences of selected characters into a single occurrence.

- `tr` normally reads from standard input and writes to standard output, so it is commonly used in pipelines.

## 12. Detecting Duplicate Lines with uniq
Remove adjacent duplicate lines :
```bash
uniq it1.txt
```
Count occurrences of adjacent identical lines :
```bash
uniq -c it1.txt
```
Display only repeated lines :
```bash
uniq -d it1.txt
```
Display only lines that are not repeated adjacently :
```bash
uniq -u it1.txt
```

>  Important: `uniq` detects consecutive identical lines, not duplicates located anywhere in a file.

To count duplicates regardless of their original positions :
```bash
sort it1.txt | uniq -c
```
To display lines that occur more than once, regardless of their original positions :
```bash
sort it1.txt | uniq -d
```

## 13. Counting Text with wc
Count lines, words and bytes :
```bash
wc it1.txt
```
Count words :
```bash
wc -w it1.txt
```
Count lines :
```bash
wc -l it1.txt
```
Count bytes :
```bash
wc -c it1.txt
```
Count characters according to the current locale :
```bash
wc -m it1.txt
```
Count information for multiple files :
```bash
wc it1.txt it2.txt
```

> `wc` reports line, word, and byte counts by default. The `-m` option counts characters rather than bytes, which can produce different results for multibyte text.

## 14. Building Pipelines
### Filters become especially useful when combined
Find the most frequent lines in a file :
```bash
sort it1.txt | uniq -c | sort -nr
```
Count accounts by login shell :
```bash
cut -d ':' -f 7 /etc/passwd | sort | uniq -c
```
Display the first five matching log lines :
```bash
grep -i 'error' /var/log/syslog | head -n 5
```

> A pipeline sends the standard output of one command to the standard input of the next. 
> This allows small utilities to work together without requiring intermediate files.

## Key Takeaways

- Use `cut` for extracting characters or delimited fields.

- Use `head`, `tail`, `less`, and `od` to inspect text and file contents.

- Use `sed` for stream-based substitutions and filtering.

- Use `sort` and `uniq` together when working with duplicate lines.

- Use `paste` for combining corresponding lines and `join` for matching records by a common field.

- Use `tr` for character-level translation, deletion and squeezing.

- Use `wc` to count lines, words, bytes or characters.

- Use `pipelines` to combine filters into practical text-processing workflows.

