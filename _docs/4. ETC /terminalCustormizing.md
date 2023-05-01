---
title: Terminal (MacOs)
category: Et Cetera
order: 1
---
### commonly used commands

Here are some commonly used terminal commands on MacOs.<br>
actually there are many other utilities and commands available as well..

|Command|Description|
|-------|-----------|
|ls| Lists the files and directories in the current directory|
|cd| Changes the current directory|
|pwd| Prints the current working directory path|
|mkdir| Creates a new directory|
|touch| Creates a new file|
|rm| Removes a file or directory|
|cp| Copies a file or directory|
|mv| Moves a file or directory or renames a file|
|cat| Displays the contents of a file|
|grep| Searches for a specific string in a file|
|ssh| Connects to a remote server via SSH|
|scp| Copies files between a local computer and a remote server|
|top| Monitors system resource usage|
|ps| Lists the currently running processes|
|history| Displays a list of previously used commands|
|man| to access the manual pages for each command|


### Terminal Font Customizing 

As I frequently use the terminal, I felt that the readability was pretty poor, so I tried changing the font color of the terminal.

**[Entry]**
~~~
vim .bash_profile
~~~
**[Insert Mode]**
~~~
i
~~~
**[Enter below Codes once get in INSERT mode ]**
~~~
export CLICOLOR=1
export LSCOLORS=ExFxCxDxBxegedabagacad
export TERM="xterm-color"
PS1='\[\e[0;33m\]\u\[\e[0m\]@\[\e[0;32m\]\h\[\e[0m\]:\[\e[0;34m\]\w\[\e[0m\]\$'
~~~
**[Write and Quite]**
~~~
:wq!
~~~

