---
title: Terminal Custormizing
category: Et Cetera
order: 1
---
터미널을 사용시 가독성이 떨어진다 생각되어 터미널 폰트색을 변경 해보았다.
As I frequently use the terminal, I felt that the readability was pretty poor, so I tried changing the font color of the terminal.

### **[Entry]**
~~~
vim .bash_profile
~~~
### **[Insert Mode]**
~~~
i
~~~
### **[Enter below Codes once get in INSERT mode ]**
~~~
export CLICOLOR=1
export LSCOLORS=ExFxCxDxBxegedabagacad
export TERM="xterm-color"
PS1='\[\e[0;33m\]\u\[\e[0m\]@\[\e[0;32m\]\h\[\e[0m\]:\[\e[0;34m\]\w\[\e[0m\]\$'
~~~
### **[Write and Quite]**
~~~
:wq!
~~~

