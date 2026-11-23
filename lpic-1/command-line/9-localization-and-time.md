# Localization and Time Configuration

Linux supports different languages, character encodings, regional formats, and time zones.

### Two important concepts :
- `Internationalization` (i18n): Designing software to support different languages and regions.

- `Localization` (l10n): Configuring software for a specific language or region.

## 1. Locale
A `locale` defines language and regional settings, such as date format, time format, numbers, and character encoding.

### Useful Commands
```bash
locale          # Display current locale settings
locale -a       # List available locales
env             # Display environment variables
```
### Important Locale Variables

Variable|Purpose
|---|---|
`LANG`|Default locale
`LC_ALL`|Overrides all other locale settings
`LC_CTYPE`|Character classification and encoding
`LC_TIME`|Date and time formats
`LC_NUMERIC`|Number formats
`LC_MESSAGES`|Language of system messages
`LC_MONETARY`|Currency format

### Changing Locale Temporarily
```bash
export LC_TIME=en_US.UTF-8
```
This changes date and time formatting for the current shell and its child processes.

Set another locale :
```bash
export LC_ALL=en_GB.UTF-8
```
Remove the override :
```bash
unset LC_ALL
```
### Priority order :
- `LC_ALL` (highest priority)

- Individual `LC_*` variables

- `LANG` (default)

Avoid setting `LC_ALL` permanently unless necessary, because it overrides all individual locale settings.

### Set Locale for One Command
```bash
LC_TIME=en_US.UTF-8 date
```
This changes the locale only for that command.

## 2. Character Encoding
Character encoding defines how text characters are represented as bytes.

Common encodings :
- `ASCII` : Supports a limited set of characters.

- `UTF-8` : Supports Unicode characters and is widely used on Linux.

- `UTF-16` : Another Unicode encoding.

### iconv
`iconv` converts text from one character encoding to another.

```bash
iconv --list
```
List supported encodings.

Syntax :
```bash
iconv -f SOURCE_ENCODING -t TARGET_ENCODING input_file
```
Example :
```bash
iconv -f UTF-8 -t ASCII file.txt
```
Convert and save the output :
```bash
iconv -f UTF-8 -t UTF-16 input.txt -o output.txt
```
Useful options :
```bash
iconv -l
iconv -f UTF-8 -t ASCII//TRANSLIT input.txt
```
`//TRANSLIT` attempts to represent characters that are not available in the target encoding using similar characters. Conversion may still lose information.

## 3. Time Zones
A time zone defines the local time used in a region.

Time zone data is commonly stored in :
```bash
/usr/share/zoneinfo/
```
Explore available time zones :
```bash
ls /usr/share/zoneinfo/
```
### Useful Commands
```bash
timedatectl              # Show date, time and time zone
timedatectl list-timezones
timedatectl status
tzselect                 # Interactive time zone selection
```
Set a time zone :
```bash
sudo timedatectl set-timezone Europe/London
```
Check the result :
```bash
timedatectl
date
```

### Important Configuration Files
File|Purpose
|---|---|
`/etc/localtime`|Local time zone configuration, usually linked to zoneinfo
`/etc/timezone`|Time zone name on some distributions, especially Debian-based systems

> Older tools such as `tzconfig` are deprecated on many modern distributions. Prefer `timedatectl`.

## 4. Date and Time Commands
```bash
date             # Display current date and time
date -u          # Display UTC time
date +%F         # YYYY-MM-DD
date +%T         # HH:MM:SS
date '+%A %B'    # Weekday and month name
```
Custom date formatting :
```bash
date '+%Y-%m-%d %H:%M:%S'
```
### Useful format symbols :

Symbol|Meaning
|---|---|
%Y|Four-digit year
%m|Month
%d|Day
%H|Hour (24-hour format)
%M|Minute
%S|Second
%A|Full weekday name
%B|Full month name
%F|Date in YYYY-MM-DD format
%T|Time in HH:MM:SS format

### Setting System Time
On modern systems, use `timedatectl` :
```bash
timedatectl status
sudo timedatectl set-time '2026-11-23 14:30:00'
```
> Changing system time requires administrator privileges and may affect running services and scheduled tasks.

## 5. NTP and Time Synchronization
`NTP` (Network Time Protocol) keeps system clocks synchronized with time servers.

### Check synchronization status :
```bash
timedatectl status
timedatectl timesync-status
```
Enable or disable systemd time synchronization when supported :
```bash
sudo timedatectl set-ntp true
sudo timedatectl set-ntp false
```
> Some systems use services such as `chronyd` for time synchronization.

### Important Notes

- `UTC` is a common reference time used by servers and infrastructure systems.

- The system `time zone` controls how local time is displayed.

- Changing the `time zone` does not change the actual system clock.

- `LC_ALL` overrides `LANG` and all individual `LC_*` variables.

- Use `UTF-8` for modern text environments whenever possible.

- `Locale` availability depends on installed and generated locales.

- `locale-gen` and `localectl` can help configure locales, depending on the Linux distribution.
