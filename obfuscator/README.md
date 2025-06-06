## Usage:
```bash 
~> python3 filename_obfuscation.py -h
usage: filename_obfuscation.py [-h] [--fakename FAKENAME] [--invisible INVISIBLE] [--visible-spaces VISIBLE_SPACES] original_file

Disguise an .exe file to look like a long .mp4,png etc filename with hidden Unicode characters.

positional arguments:
  original_file         The original .exe file to rename

options:
  -h, --help            show this help message and exit
  --fakename FAKENAME   Visible part of the filename (default: hello_word.mp4)
  --invisible INVISIBLE
                        Number of invisible Unicode characters (default: 20)
  --visible-spaces VISIBLE_SPACES
                        Number of visible spaces before .exe (default: 12)

Example: python3 filename_obfuscate.py shell.exe --fake-name hello_word.mp4 --invisible 20 --visible-spaces 10
```
## Do you need visible spaces in real-world filename obfuscation?
- Short answer:
    - No, visible spaces aren’t strictly necessary. The invisible Unicode characters alone (like zero-width spaces \u200B) are enough to break up the filename visually and hide the real extension.

    - Why might someone add visible spaces?
    - To make the filename look longer or suspiciously padded, confusing users further.

Some file explorers or systems may collapse or ignore zero-width spaces, so visible spaces make the “gap” more obvious.
It might mimic how some malicious files try to look like normal files but with odd spacing.

- Why you might skip visible spaces:
    - Zero-width spaces are invisible but still break up the filename and fool naive viewers.
- Visible spaces can sometimes make the filename look weird or easier to spot as suspicious.
- Some systems trim trailing spaces in filenames (especially Windows), which may break your trick.
- Less chance of the filename being flagged as “odd” if it’s cleaner.

## 🛡️ Counter measure:
To see those hidden characters on Windows:

#### Windows
```cmd
dir /x
```
#### PowerShell:
```powershell
Get-ChildItem | ForEach-Object { $_.Name, ($_.Name.ToCharArray() | ForEach-Object { [int][char]$_ }) }
```
#### Linux/Unix
```bash
ls -b 
```
### ✅ Feature 

| Feature                                         | Included? | Description |
|------------------------------------------------|-----------|-------------|
| Rename any file                                | ✅        | You pass `shell.exe` (or any `.exe`) as input |
| Set a fake base name like `hello_word.mp4`     | ✅        | Via `--fake-name hello_word.mp4` |
| Insert N invisible Unicode characters (`U+200B`) | ✅        | Controlled via `--invisible 20` (or any number) |
| Avoid visible spaces (optional)                | ✅        | Use `--visible-spaces 0` or omit the flag (default = 0) |
| Output ends in `.exe`                          | ✅        | Always renamed to `.exe` so it executes on Windows |
| UTF-8 / Unicode-safe                           | ✅        | All characters are valid on Windows NTFS and Linux ext4 |
