---
layout: page
title: codeblob functions and snippets
permalink: /newpage/technology/codeblob-functions-and-snippets
order: 12
---

This page has wordy crocs; use at your own risk.

Table of Contents
1. [git](#git)
  * [git aliases](#git-aliases)
  * [git get filenames](#git-get-filenames)
1. [ssh](#ssh)
  * [SSH auto complete function](#ssh-auto-complete-function)
1. [ZSH customize](#zsh-customize)
  * [zsh / bash export settings](#zsh-bash-exports)

```bash

```

---

## ZSH customize
### zsh / bash export settings
```bash
export LC_COLLATE=C

export -pf | grep -5 LC_

set | grep -3 -i LC_

```
---

## git
### git aliases
```bash
# git aliases
alias lg1="git log --graph --oneline --decorate --all"
alias lg2="git log --graph --abbrev-commit --decorate --date=relative --all"
```


### git get filenames
Show the name of the files committed in this commit.
```bash
git show --pretty="" --name-only efb6c
```

---

### ssh
#### SSH auto complete function
Reads the known hosts file and attempts to autocomlete hostnames to which have been used in the past.
Requires the .ssh/known_hosts file to be stored in non-binary format (there's a flag for that)
```bash
_complete_ssh_hosts ()
{
        COMPREPLY=()
        cur="${COMP_WORDS[COMP_CWORD]}"
        comp_ssh_hosts=`cat ~/.ssh/known_hosts | \
                        cut -f 1 -d ' ' | \
                        sed -e s/,.*//g | \
                        grep -v ^# | \
                        uniq | \
                        grep -v "\[" ;
                cat ~/.ssh/config | \
                        grep "^Host " | \
                        awk '{print $2}'
                cat ~/.ssh/config | \
                        grep "^Host " | \
                        awk '{print $3}'
                `
        COMPREPLY=( $(compgen -W "${comp_ssh_hosts}" -- $cur))
        return 0
}
complete -F _complete_ssh_hosts ssh
```
