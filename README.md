# flax

> FlaX, the Flag Expander!

## What does it do?
Flags, in this context, are those options we add to commands, as when we write `mkdir -p folder`. The `-p` thing is a flag.

So FlaX helps you do three different things depending on which option you choose. It can: 
* [l option] expand to **long** flags a command's short flags 
* [s option] shrinks to **short** flags a command's long flags
* [d option] give **help** (a **description**) of each of the command's flags

Each of those can be useful for different reasons.
* Expanding is useful to [give your scripts more legibility](https://thechangelog.com/use-long-flags-when-scripting/).
* Shrinking is useful for command line's convenience.
* Help is useful because sometimes before executing a command with flags you are not familiar with, you may want to know what it's suppose to be doing.

## Usage 
`flax [long|l|L | short|s|S | desc|d|D] [COMMAND with flags]`

## Examples

### To get long flags

Try those to see the output:

```bash
# Invented, makes no sense
flax long wget -r --relative -H -Q 6 -D de,ca,com 

# rsync has so many flags you can have Tom Hanks in it...
flax l rsync -tomhanks --fuzzy -T somedir

# ...and so does curl
flax L curl -tomcruise www.myurl.com

# Converts a command in your clipboard to long flag.
flax l $(xsel -ob) | xsel -ib
```

### To get short flags

`flax S ls -X --almost-all --no-group -l --human-readable`

outputs: 

`ls -XAGlh`

and

`flax s curl --proxytunnel --include --netrc --insecure --fail --list-only --output --speed-time --data`

outputs: 

`curl -pinkfloyd`

### To get descriptions

`flax d ls -XAGh --group-directories-first`

```
ls (1)               - list directory contents
                             extension -X, size -S, time -t, version -v  
  -X                         sort alphabetically by entry extension  
  -A, --almost-all           do not list implied . and ..  
  -G, --no-group             in a long listing, don't print group names  
  -h, --human-readable       with -l, print sizes in human readable format  
      --group-directories-first 
```
or try this meta-acronym example...

`flax d rsync -acronyms`

```
rsync (1)            - a fast, versatile, remote (and local) file-copying tool
 -a, --archive               archive mode; equals -rlptgoD (no -H,-A,-X)  
 -c, --checksum              skip based on checksum, not mod-time & size  
 -r, --recursive             recurse into directories  
 -o, --owner                 preserve owner (super-user only)  
 -n, --dry-run               perform a trial run with no changes made  
 -y, --fuzzy                 find similar file for basis if no dest file  
 -m, --prune-empty-dirs      prune empty directory chains from the file-list  
 -s, --protect-args          no space-splitting; only wildcard special-chars
 ```

Also works with builtin functions...

`flax d echo -en`

```
echo (1)             - display a line of text
      -e        enable interpretation of the following backslash escapes  
      -n        do not append a newline
```

...and with commands that use XF86-style long flags (`-myoption`):

`flax d lynx -ftp -vikeys`

```
lynx (1)             - a general purpose distributed information browser for the World Wide Web
  -ftp              disable ftp access (off)  
  -vikeys           enable vi-like key movement (off)
```


## WARNING
This program has not been tested for all corner cases. Exercise caution when using the output commands, because even if the original input was correct, the output *may* not be. No warranties, please be smart. But if files disappear, your cat is microwaved or sundry related tragedies occur, I welcome you to curse me, tell me how to fix it, or at least send me a bug report.

## Limitations (not exhaustive)
* Ignores inexistent flags. Uses 1st match. Uses 1st given expansion.
* Builtin functions (cd, echo, etc.) don't use long flags. Therefore, only the -d option is available.
* Specific reported issues:
  * `cp -p`:  no long flags, but one appears in the info at the right that isn't it. (Try to fix.)
  * `sed -iThis`:  argument glued to flag. Not treated: rather rare and too much trouble to handle.
  * `wget -nv -nc`:  some single dash with 2+ letters. Not treated: rather rare and too much trouble to handle.
  * `lynx -dump`:  only single dash with 2+ letters. Only treated for help/description option.
  * `netstat -tuxw`:  long flags for those are in a separate line harder to parse. Not treated.

## Dependencies
Only `sed` and `perl`.

