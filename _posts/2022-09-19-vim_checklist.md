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
  | <ul><li>:w : write to the file</li><li>:q : quit the file if nothing written</li><li>:q! : force quit even with written text</li></ul> | quitting/writing to vim |  |
| :!<command> | Running shell commands from vim |  |
  | <ul><li>:set number : normal line numbers</li><li>:set relativenumber : line numbers are relative</li></ul> | Make vim have line numbers |  |
| Ex: 5<arrow key down> (goes 5 lines down) | Typing a number x before a vim command makes you do that command x many times |  |
  | <ul><li>h : move left</li><li>j : move down</li><li>k : move up</li><li>l : move right</li></ul> | Moving around (arrow keys also works) |  |
  | <ul><li> u : undo (undoes whatever you just did in the last insert mode only)</li><li>Ctrl + r : redo</li></ul> | undo/redo previous actions |  |
  | <ul><li>dd : delete entire line</li><li>Shift + d : delete the rest of the line from where cursor is</li></ul> | Deleting text |  |
| yy/Shift + y | Copy entire line |  |
  | <ul><li>cc : Change the line (delete but also puts you in insert mode after)</li><li>Shift + c : Changes the rest of the line (changes from where cursor is to the end of the line)</li></ul> | Changing in vim (like delete but leaves you in insert mode after) |  |
| r | Replace character cursor is on |  |
  | <ul><li>p : paste text</li><li>Shift + p : paste text on line above cursor position</li></ul> | Pasting text |  |
  | <ul><li>w : move forward a word (this consideres a word separated by hyphens or other searators as well besides spaces)</li><li>Shift + w : move forward a word (only delimiter is a space)</li><li>b : move backward a word (same conditions as moving forward)</li><li>Shift + b : move backward a word (same conditions as moving forward)</li><li>e : jump to the end of the current word</li><li>0 : go to the beginning of the line (without entering insert mode)</li><li>$ : go to the end of line (without entering insert mode)</li></ul> | Moving across text |  |
  | <ul><li>dw : deletes a word in front of cursor position</li><li><number x>dw : deletes x many words in front</li><li>d0 : delete everything until the start of line</li><li>d$ : delete everything until end of line</li></ul> | Combining vim commands examples |  |
| These all can be ci<something>, di<something, yi<something> <ul><li>diw : deletes the word</li><li>ciw : changes the word</li><li>yiw : copy the word you are inside of</li><li>di" : deletes everything inside two quotation marks</li><li>ci" : changes everything inside two quotation marks</li><li>yi" : copy everything inside quotes</li></ul> | Options when inside a word (cursor is like in the l part of hello) |  |


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
