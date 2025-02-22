---
layout: default
title: Vim
parent: Tools
nav_order: 1
---

# Modes

- Normal \[esc\]
- Insert \[i\]
- Command \[:\]
- Visual

# Setting

- `vim --version`: find the `vimrc` file
- `set number`: adopt line number
- `set relativenumber`: adopt relative line number

# Grammar

- H: left
- J: down
- K: up
- L: right

- i: insert ahead
- a: append
- o: add a line ahead
- O: append a line

- G: jump to the last line
- gg: jump back to the first line
- \[number\]j: move up \[number\] lines
- \[number\]j: move down \[number\] lines

- yy: copy this line
- dd: delete this line
- p: paste

- .: repeat the last operation
- u: revocate the last operation
- ctrl+r: recover the previous operation

- dw: delete the word
- cw: change the word
- yw: copy the word

- w: jump to the first letter of the next word
- e: jump to the last letter of the next word
- b: jump to the last letter of the previous word

-/ : search for a word

- %s/\[old\]/\[new\]/g: replace \[old\] with \[new\] globaly

- ci\{: delete content in \{\}

- ctrl+v: switch to visual mode for words
- shift+v: switch to visual mode for lines
