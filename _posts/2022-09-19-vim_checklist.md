---
layout: post
title: Vim checklist
---

This is a easy access vim checklist that has examples along with the commands and description.

## Normal Mode

When entering into vim, you enter normal mode. Command mode is where most of the commands in vim will be used.

| Command | Description | Example |
|---|---|---|
| Esc | Entering normal mode (if not already in) |  |
| :w : write to the file<br />:q : quit the file if nothing written<br />:q! : force quit even with written text<br /> | quitting/writing to vim |  |
| :!<command> | Running shell commands from vim |  |
| :set number : normal line numbers<>:set relativenumber : line numbers are relative</li></ul> | Make vim have line numbers |  |
| Ex: 5<arrow key down> (goes 5 lines down) | Typing a number x before a vim command makes you do that command x many times |  |
| h : move left<br />j : move down<br />k : move up<br />l : move right</li></ul> | Moving around (arrow keys also works) |  |
| u : undo (undoes whatever you just did in the last insert mode only)<br />Ctrl + r : redo</li></ul> | undo/redo previous actions |  |
| dd : delete entire line<br />Shift + d : delete the rest of the line from where cursor is</li></ul> | Deleting text |  |
| yy/Shift + y | Copy entire line |  |
| cc : Change the line (delete but also puts you in insert mode after)<br />Shift + c : Changes the rest of the line (changes from where cursor is to the end of the line)</li></ul> | Changing in vim (like delete but leaves you in insert mode after) |  |
| r | Replace character cursor is on |  |
| p : paste text<br />Shift + p : paste text on line above cursor position</li></ul> | Pasting text |  |
| w : move forward a word (this consideres a word separated by hyphens or other searators as well besides spaces)<br />Shift + w : move forward a word (only delimiter is a space)<br />b : move backward a word (same conditions as moving forward)<br />Shift + b : move backward a word (same conditions as moving forward)<br />e : jump to the end of the current word<br />0 : go to the beginning of the line (without entering insert mode)<br />$ : go to the end of line (without entering insert mode)</li></ul> | Moving across text |  |
| dw : deletes a word in front of cursor position<br /><number x>dw : deletes x many words in front<br />d0 : delete everything until the start of line<br />d$ : delete everything until end of line</li></ul> | Combining vim commands examples |  |
| These all can be ci<something>, di<something, yi<something> diw : deletes the word<br />ciw : changes the word<br />yiw : copy the word you are inside of<br />di" : deletes everything inside two quotation marks<br />ci" : changes everything inside two quotation marks<br />yi" : copy everything inside quotes</li></ul> | Options when inside a word (cursor is like in the l part of hello) |  |

## Insert Mode

Insert mode will be used to do all the typing/programming. 

| Command | Description | Example |
|---|---|---|

## Visual Mode

Visual mode will be used to do different things to characters/lines of text.

| Command | Description | Example |
|---|---|---|

## Other Useful Vim tricks

These are some other random useful tricks that you might want to know when using vim.

| Command | Description | Example |
|---|---|---|
