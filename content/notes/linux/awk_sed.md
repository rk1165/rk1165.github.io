+++
title = 'AWK and SED'
date = 2024-02-11

+++

### awk

- Curly braces are used to group blocks of code together, similar to C.
- `awk '{ print }' /etc/passwd` When a print command appears by itself, the full contents of the current line are printed.
- `awk '{ print $0 }' /etc/passwd` the `$0` variable represents the entire current line.
- `awk '{ print "" }' /etc/passwd` Whenever you pass the "" string to the print command, it prints a blank line.
- `awk '{ print "hiya" }' /etc/passwd` hiya will be printed for each line of /etc/passwd
- `awk ‑F":" '{ print $1 }' /etc/passwd` print a list of all user accounts on system
- `awk ‑F":" '{ print $1 $3 }' /etc/passwd` prints the first and third fields of the /etc/passwd file.
- `awk ‑F":" '{ print $1,$3 }' /etc/passwd` prints the space between both fields
- `awk ‑F":" '{ print "username: " $1 "\t\tuid:" $3 }' /etc/passwd` will insert some text labels
- awk can be told to source the script file by passing it the `-f` option. `awk -f myscript.awk myfile.in`
- Normally, awk executes each block of your script's code once for each input line.
- For situations where we may need to execute initialization code _before_ awk begins processing the text, awk allows us to define a `BEGIN` block.
- It's an excellent place to initialize the `FS` variable, print a heading, or initialize other global variables
- Another special block, called the `END` block is executed after all lines in the input file have been processed.
- We can place any kind of boolean expression before a code block to control when a particular block is executed.
- `$1 == "fred" { print $3 }` will output the third field of all lines that have a first field equal to `fred`. If the first field of the current line is not equal to `fred`, awk will continue processing the file and will not execute the `print` statement for the current line.
- awk provides the `~` and `!~` operators, which mean "matches" and "does not match".
- They're used by specifying a variable on the left side of the operator, and a regular expression on the right side.
- `$1 ~ /root/ { print $3 }` will print only the third field on the line if the first field on the same line contains the character sequence root
- `! /matchme/ { print $1 $3 $4 }` can be transformed to this

```awk
{
  if ( $0 !~ /matchme/ ) {
    print $1 $3 $4
  }
}
```

- Both the above scripts will output only those lines that _don't_ contain a `matchme` character sequence
- `( $1 == "foo" ) && ( $2 == "bar" ) { print }` print only those lines where field one equals foo and field two equals bar
- Awk has its own complement of special variables. Example `FS`. The `FS` value is not limited to a single character; it can also be set to a regular expression, specifying a character pattern of any length.
- `FS="\t+"` : processes fields separated by one or more tabs.
- `NF` variable - also called the "number of fields" variable. Awk sets this variable to the number of fields in the current record.
- `NF == 3 { print "this particular record has three fields: " $0 }`

```awk
{
  if ( NF > 2 ) {
    print $1 " " $2 ":" $3
  }
}
```

- The record number `NR` is another handy variable. It will always contain the number of the current record.
- `(NR < 10 ) || (NR > 100) { print "We are on record number 1‑9 or 101+" }`

```awk
{
  # skip header
  if ( NR > 10 ) {
    print "ok, now for the real information!"
  }
}
```

- [Understanding Awk](https://earthly.dev/blog/awk-examples/)
- `awk 'BEGIN{ FS=":"; OFS="\t" } { print $1, $6}' /etc/passwd`
- `ls -l | awk 'BEGIN { sum=0} {sum += $5} END {print sum}'` total size of files
- `echo "Mary had a little lamb, little lamb, little lamb" | awk '{print substr($0, 1,4)}'` print first 4 chars
- `echo "This is my text" | awk '{ print length($0)}'` length of a line`
- `echo "This is my text" | awk '{ if (length($0) > 10) print "String too long"; else print length($0)}'` if-else
- `head currencies.csv | awk 'BEGIN {FS=","} {print toupper($2)}'`
- `awk 'BEGIN{IGNORECASE=1; FS=","} /fiji/ { print NR, "-", $0}' currencies.csv`

### sed

- `sed '' quotes` : prints the quotes file as it is
- `sed -n 's/idea/pidgeons/' quotes` replaces the word idea with pidgeons but suppresses o/p
- `sed -n 's/idea/pidgeons/p' quotes` replaces the word and prints the lines which had the replacement
- `sed -n 's/idea/pidgeons/; p' quotes` : sed executes both the commands on each line, replaces the string and prints
- `sed -n -e 's/idea/pidgeons/' -e 'p' quotes`
- `sed -i 's/idea/pidgeons/' quotes` lets sed change the file also
- `sed -n '3,6p' quotes` prints between 3rd and 6th line
- `sed -n '/Tesla/{g;p}; h' quotes` sends every line to **hold buffer** and then when any line matches Tesla, fetches the previous line from hold buffer and prints it.
- `sed -e 's/book: //I' classic_reads` : replace book with blank, case insensitive
- `sed -e '1,3 s/book: //I' classic_reads` : replace lines 1,3
- `sed -n -e '1,3 s/book: //I' classic_reads`
- `sed -n -e '1,3 s/book: //Ip' classic_reads`
- `sed -e 's/book: //I' -e 's/author: //I' classic_reads`
- `sed -e 's/Book: //' -e 's/Author: //' -e 's/-/by/' -e '1 i Books you must read' -e '1 i -----------------------------' classic_reads`
- `sed -e '10,30d' classic_reads` : delete lines 10 to 30
- `sed -e '1,20d' classic_reads`
- `sed -n '10,15p' classic_reads`
- `ls -l /etc | sed -n '/^d/p'`
