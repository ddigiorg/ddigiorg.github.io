---
layout: page
title: Vim
date: 2021-08-22
tags: computer
---

## General

- `:h[elp] <keyword>`: open help for keyword
- `:sav[eas] <file>`: save file as
- `:clo[se]`: close current pane
- `:ter[minal]`: open a terminal window
- `K`: open man page or word under the cursor

>**Tip**: Run `vimtutor` in a terminal for a basic Vim tutorial.

## Cursor Movement

- `h`: move cursor left
- `j`: move cursor down
- `k`: move cursor up
- `l`: move cursor right
- `H`: move to top of screen
- `M`: move to middle of screen
- `L`: move to bottom of screen
- `w`: jump forwards to the start of a word
- `W`: jump forwards to the start of a word (words can contain punctuation)
- `e`: jump forwards to the end of a word
- `E`: jump forwards to the end of a word (words can contain punctuation)
- `b`: jump backwards to the start of a word
- `B`: jump backwards to the start of a word (words can contain punctuation)
- `ge`: jump backwards to the end of a word
- `gE` jump backwards to the end of a word (words can contain punctuation)
- `%` move to the matching character (default supported pairs: '()', '{}', '[]' - use `:h matchpairs` in vim for more info)
- `0`: jump to the start of a line
- `^`: jump to the first non-blank character of the line
- `$`: jump to the end of a link
- `g_`: jump to the last non-blank character of the line
- `gg`: go to the first line of the document
- `G`: go to the last line of the document
- `gd`: move to local declaration (useful for finding first instance of code variables)
- `gD`: move to global declaration (useful for finding first instance of code variables)
- `fx`: jump to next occurrence of character x
- `tx`: jump to before next occurrence of character x
- `Fx`: jump to previous occurrence of character x
- `Tx`: jump to after previous occurrence of character x
- `;`: repeat previous f, t, F, or T movement
- `,`: repeat previous f, t, F, or T movement, backwards
- `}`: jump to next paragraph (or function/block when editing code)
- `{`: jump to previous paragraph (or function/block when editing code)
- `zz`: center cursor on screen
- `Ctrl` + `e`: move screen down one line (without moving cursor)
- `Ctrl` + `y`: move screen up one line (without moving cursor)
- `Ctrl` + `b`: move back one full screen
- `Ctrl` + `f`: move forward one full screen
- `Ctrl` + `d`: move forward 1/2 a screen
- `Ctrl` + `u`: move back 1/2 a screen

>**Tip**: Prefix a cursor movement command with a number to repeat it. For example, `4j` moves down 4 lines.

## Inserting/Appending Text

- `i`: insert before the cursor
- `I`: insert at the beginning of the line
- `a`: append after the cursor
- `A`: append at the end of the line
- `o` open a new line beloe the current line
- `O`: open a new line above the current line
- `ea`: append at the end of the word
- `Ctrl` + `h`: delete the character before the cursor during insert mode
- `Ctrl` + `w`: delete the word before the cursor during insert mode
- `Ctrl` + `j`: begin new line during insert mode
- `Ctrl` + `t`: indent (move right) line one shiftwidth during insert mode
- `Ctrl` + `d`: de-indent (move leftO line one shiftwidth during insert mode
- `Ctrl` + `n`: insert (auto-complete) next match before the cursor during insert mode
- `Ctrl` + `p`: insert (auto-complete) previous match before the curser during insert mode
- `Ctrl` + `rx`: insert contents of register x
- `Ctrl` + `ox`: temporarily enter normal mode to issue one normal-mode command x
- `Esc`: exit insert mode

## Editing

- `r`: replace a single character
- `R`: replace more than one character until `ESC` is pressed
- `J`: join line below to the current one with one space in between
- `gJ`: join line below to the current one without space in between
- `gwip`: reflow paragrah
- `g~`: switch case up tp motion
- `gu`: change to lowercase up to motion
- `gU`: change to uppercase up to motion
- `cc`: replace entire line
- `C` or `c$`: replace to the end of the line
- `ciw`: replace entire word
- `cw` or `ce`: replace to the end of the word
- `s`: delete character and substitute text
- `S`: delete line and substitute text (same as `cc`)
- `xp`: transpose two letters (delete and paste)
- `u`: undo
- `U`: restore (undo) last change
- `Ctrl` + `r`: redo
- `.`: repeat last command

## Marking Text

- `v`: start visual mode (then mark likes and do a command)
- `V`: start linewise visual mode
- `o`: move to other end of marked area
- `Ctrl` + `v`: start visual block mode
- `O`: move to other corner of block
- `aw`: mark a word
- `ab`: mark a block between (), inclusive
- `aB`: mark a block between {}, inclusive
- `at`: mark a block between <> tags, inclusive
- `ib`: mark a block between (), exclusive
- `iB`: mark a block between {}, exclusive
- `it`: mark a block between <> tags, exclusive
- `ESC`: exit visual mode

>**Tip**: instead of `b` or `B` one can also use `(` or `{` respectively.

## Visual Commands

- `>`: shift text right
- `<`: shift text left
- `y`: yank (copy) marked text
- `d`: delete marked text
- `~`: switch case
- `u`: change marked text to lowercase
- `U`: change marked text to uppercase

## Registers

- `reg[isters]`: show registers content
- `"xy`: yank into register x
- `"xp`: paste contents of register x
- `"+y`: yank into the system clipboard register
- `"+p`: paste from the system clipboard register

>**Tip**: Registers are being stored in `~/.viminfo`, and will be loaded again on next restart of vim.

>**Tip**: Special registers:
>  `O` : last yank
>  `"`: unnamed register, last delete or yank
>  `%`: current file name
>  `#`: alternate file name
>  `*`: clipboard contents (X11 primary)
>  `+`: clipboard contents (X11 clipboard)
>  `/`: last search pattern
>  `:`: last command-line
>  `.`: last inserted text
>  `-`: last small (less than a line) delete
>  `=`: expression register
>  `_`: black hole register

## Marks and Positions

- `:marks`: list of marks
- `ma`: set current position for mark A
- `'a`: jump to position of mark A
- `y'a`: tank text to position of mark A
- ``0`: go to the position where Vim was previously exited
- ``"`: go to the position when last editing this file
- ``.`: go tp the position of the last change in this file
- ````: go to the position before the last jump
- `:ju[mps]`: list of jumps
- `Ctrl` + `i`: go to newer position in jump list
- `Ctrl` + `o`: go to older position in jump list
- `:changes`: list of changes
- `g,`: go to newer position in change list
- `g;`: fo to older position in change list
- `Ctrl` + `]`: jump to the tag under cursor

>**Tip**: To jump to a mark you can either use a backtick (```) or an apostrophe (`'`). Using an apostrophe jumps to the beginning (first non-blank) of the line holding the mark.

## Macros

- `qa`: record macro a
- `q` stop recording macro
- `@a`: run macro a
- `@@`: rerun last macro

## Cut and Paste



## Links
