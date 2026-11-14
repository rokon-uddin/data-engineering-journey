# Linux Commands — Reference

Week 02 · the Linux needed for working with data. Source: class 2 Linux cheatsheet.

## Navigation & files

```bash
pwd                 # print the current folder
ls                  # list what is in this folder
ls -lh              # long listing: size (human-readable), date, permissions
ls -lhS             # same, sorted by size — largest first
cd folder           # go into that folder
cd ..               # go up one level
cd ~                # go to the home folder
cd -                # go back to the previous folder
mkdir -p a/b        # create a folder, including missing parent folders
cp a b              # copy a file (use cp -r for folders)
mv a b              # move or rename
rm a                # delete — permanent, there is no undo
```

## Looking inside a file

```bash
head -5 f           # first 5 lines
tail -5 f           # last 5 lines
tail -f f           # follow live — prints new lines as they arrive
wc -l f             # count the lines
less f              # page through the file (q = quit, / = search)
cat f               # print the whole file — avoid on large files
```

> Always start a large file with `head`. `cat` will scroll 100k lines past you, and `Ctrl/cmd + C` is the only way out.

## Searching (grep)

```bash
grep "ERROR" f          # lines that contain ERROR
grep -c "ERROR" f       # count of matching lines only
grep -i "error" f       # case-insensitive match
grep -v "INFO" f        # lines that do NOT contain INFO
grep -n "ERROR" f       # show line numbers with the matches
grep "^2026-10-17" f    # lines that START with this text (^ = start of line)
grep -r "TODO" .        # search every file under the current folder
```

## Cutting, sorting, counting

```bash
cut -c1-10          # characters 1–10 of each line
cut -d',' -f3       # split on comma, take field 3
cut -d'|' -f2       # split on pipe, take field 2
sort                # sort lines
sort -rn            # sort numerically, largest to smallest
uniq -c             # count repeated lines — input must be sorted first
tr -d ' '           # delete all spaces
```

## Pipes & redirection

```bash
# |  sends the output of the left command into the right command
# Count ERROR lines per day (first 10 chars = date), most frequent first
grep "| ERROR |" app.log | cut -c1-10 | sort | uniq -c | sort -rn

# >   write to a new file (overwrites existing content)
# >>  append to the end of a file
grep "| ERROR |" app.log > errors.log
echo "one more line" >> errors.log
```

> Unix philosophy: small tools that each do one job well, joined with pipes to do big jobs.

## Permissions

```bash
ls -l script.sh         # shows permissions, e.g. -rwxr-xr-x
chmod +x script.sh      # grant execute permission
./script.sh             # run the script
```

`r` read · `w` write · `x` execute. Three groups, in order: owner, group, everyone else.

## Environment variables

```bash
echo $HOME              # path of the home folder
echo $PATH              # folders the shell searches for commands
export API_KEY=abc      # set a variable for this terminal session only
env | grep API          # list the variables that are set, filtered by API
```