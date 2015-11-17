# flax

> FlaX, the Flag Expander!

## What does it do?
Flags, in this context, are those options we add to commands, as when we write "mkdir -p folder". The "-p" thing is a flag.

So FlaX helps you do three different things depending on which option you choose. It can: 
* [using the l option] expand to **long** flags a command's short flags 
* [using the s option] shrinks to **short** flags a command's long flags
* [using the d option] give **help** (a **description**) of each of the command's flags

Each of those can be useful for different reasons.
* Expanding is useful to [give your scripts more legibility](https://thechangelog.com/use-long-flags-when-scripting/).
* Shrinking is useful for command line's convenience.
* Help is useful because sometimes before executing a command with flags you are not familiar with, you may want to know what it's suppose to be doing.

## Usage 
`flax [long|l|L | short|s|S | desc|d|D] [COMMAND with flags]`

## Examples

### To get long flags
```bash
flax long wget -r --relative -H -Q 6 -D de,ca,com  # Invented, makes no sense
flax l rsync -tomhanks --fuzzy -T somedir          # rsync has so many flags you can have Tom Hanks in it...
flax L curl -tomcruise www.myurl.com               # ...and so does curl
flax l $(xsel -ob) | xsel -ib                      # Converts a command in your clipboard to long flag.
```

### To get short flags
`flax S ls -X --almost-all --no-group -l --human-readable`
outputs: 
`ls -XAGlh`

`flax s curl --proxytunnel --include --netrc --insecure --fail --list-only --output --speed-time --data`
outputs: 
`curl -pinkfloyd`

### To get descriptions
```bash
flax d ls -XAGlh --group-directories-first
flax D rsync -acronyms                       # A meta-acronym?
```
Also works with builtin functions...
`flax d echo -en`

...and with commands that use XF86-style long flags (-myoption):
`flax desc lynx -dump -vikeys`

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
"

## Dependencies
Only `sed` and `perl`.

