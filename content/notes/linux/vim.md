+++
title = 'Vim'
date = 2024-02-11

+++

- `:scriptnames` - shows what scripts vim loads.
- Check what slows down the editor most at startup by running the following and look at the `start.log` it creates: `vim --startuptime start.log file_name`
- To see how quickly Vim starts without your existing configuration: `vim --clean --startuptime clean.log file_name`
- To check the runtimepath `:set runtimepath`
- Here is a selection of subfolder that vim looks for when processing each directory in its runtimepath:
- **plugin**: vim script files that are loaded automatically when editing any kind of file.
- **autoload** scripts in autoload contain functions that are loaded only when requested by other scripts.
- **ftdetect** scripts to detect filetypes.
- **ftplugin** scripts that are executed when editing files with known type
- **compiler** Definitions of how to run various compilers or linters, and of how to parse their output. Must be called with `:compiler`
- **pack** Container for Vim 8 native packages.
- For a comprehensive overview of settings you can choose in .vimrc, run `:options`
- `vim -o2` : open 2 windows split horizontally
- `vim -O2` : open 2 windows split vertically
- while running vim `:sp filename` for a horizontal split, `:vsp filename` or `:vs filename` for a vertical split
- Ctrl + w (h, j, k, l) to move between open panes.
- `:qa` : close all tabs.
- `:wqa` : save and quit
- `:%s/\<word_to_be_replaced\>/word_to_by_replaced_by/gc` for interactively replacing word
- `ma` - mark the first line as mark a, move to the bottom of the text to be sorted
- `!'asort` - sorts
- V - Enter visual mode, move to the bottom of the text to be sorted and then !sort
- `:set list` - When the display is set into "list mode" all characters print. Tabs show up as "^I" and the end of line shows up as $.
- `:set wrapmargin=70` - you can tell Vim to automatically break lines when the run longer than 70 characters
- Vim can be used to locate procedures within a set of C or C++ program files.
- First generate a table of contest file called a "tags" file.
- `ctags *.c`
- Once this file is generated, you tell Vim that you want edit a procedure, and it will find the file containing that procedure and position you there
- example `vim -t write_file`
- in `write_file` procedure it calls `setup_data` and you need to look at that procedure. To jump to that function, position the cursor at the beginning of the word `setup_data` and press Ctrl+]
- `ci<char>` to delete within the `<char>` and put you in insertion mode. For example, `ci(` deletes everything within `(...)` on the same line
- `S` deletes the current line and puts in insert mode
- `.` repeats the previous action
- `I` inserts at the beginning of the line
- `gg=G` to indent the full file
- If you want to open the file `hello.txt` and immediately execute a Vim command, you can pass to the vim command the `+{cmd}` option. `vim +%s/pancake/bagel/g hello.txt` or `vim -c %s/pancake/bagel/g hello.txt`
- You can redirect the output of the `ls` command to be edited in Vim with `ls -l | vim -`
- `v:` Visual mode - selecting blocks of texts
- `V:` Visual Line mode - select block of texts by line
- `Ctrl+v` : Visual block mode - selecting block of text
- `:e {name of file}` to edit a file
- `:ls` -> list of different buffers that are open
- `:help {topic}` - to look for help for topic
- `:x` is equivalent to `:wq`
- `^` -> moves to the first non-white space character
- `H` -> goes to the top of the Screen
- `M` -> goes to the middle of the screen
- `L` -> goes to the bottom of the screen
- `Ctrl+d` -> goes down by a page
- `Ctrl+u` -> goes up by a page
- `%` -> move to the corresponding item
- `f{character}`, `t{character}`, `F{character}`, `T{character}` -> find/to forward/backward {character} on the current line
- `de` -> delete till the end of the word
- `d^` -> delete until the first non-whitespace character
- `cw` -> delete the word and puts into INSERT mode
- `~` -> capitalizes the letter under the cursor
- `sp/vsp` to split windows

### Buffers, Windows, and tabs

- `vim file1.js file2.js` creates two buffers which we can see using `:buffers`, `:ls`, or `:files`
- `:bnext` or `:bprevious`
- `:buffer` + filename
- `:buffer` + `n` where n is the number of buffer
- `:bdelete n`
- `:tabnew file1.txt`
- `3gt` or `3gT` to move to 3rd tab
- `:tablast` or `:tabfirst`

### Searching files

- `:edit file.txt`
- `:edit *.yml<Tab>`
- `:edit **/*.md<Tab>`
- `:edit .` - opens built in file explorer in VIM
- `netrw` commands
  - `%` create a new file
  - `d` create a new directory
  - `R` create a file or directory
  - `D` delete a file or directory
-

### Macros

- `q{character}` to start recording a macro in register `{character}`
- `q` to stop recording
- `@{character}` replays that macros
- `{number}@{character}` executes a macro {number} times
- Macros can be recursive
  - first clear the macro with `q{character}q`
  - record the macro, with `@{character}` to invoke the macro recursively (will be a no-op until recording is complete)

### Links

- [Vim Tips Wiki](http://vim.wikia.com/wiki/Vim_Tips_Wiki)
- [Vim Advent Calendar](https://vimways.org/2018/)
- [Vim Golf](http://www.vimgolf.com/)
- [Learn Vim](https://github.com/iggredible/Learn-Vim/tree/master)