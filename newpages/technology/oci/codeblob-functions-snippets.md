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
  * [SSH config examples](#ssh-config-examples)
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

#### SSH config examples

To get a complete list of supported ciphers, MACs, key exchange algorithms and authentication algorithms, 
see the manual for ssh_config:
```bash
man ssh_config
```

In order to simplify the config file, you can assign a given configuration to many hosts at a time with the * symbol as shown below.
The man page reports regex is supported on the `Host` line, however, complex PCRE regex does not seem to be supported in all cases (c.2023).
```bash
Host *
    Protocol 2
    HostKeyAlgorithms ssh-rsa
    Ciphers aes256-ctr, aes256-cbc
    MACs hmac-sha2-512, hmac-sha2-256
    KexAlgorithms diffie-hellman-group-exchange-sha256
    IdentityFile ~/.ssh/your_special_id_rsa
```
This will cause all your SSH connections to any server to use those parameters unless they have already 
been specified. To make this clear: when the config file is read, only the first definition of a parameter for a 
certain server will be used. So in the example below:

```bash
Host singleserver
    Hostname example.com
    User myusername
    Port 10022
    Ciphers aes128-cbc
    MACs hmac-sha1
    KexAlgorithms diffie-hellman-group1-sha1

Host *.example.com
    Protocol 2
    HostKeyAlgorithms ssh-rsa
    Ciphers aes256-ctr
    MACs hmac-sha2-512
    KexAlgorithms diffie-hellman-group-exchange-sha256
    IdentityFile ~/.ssh/your_special_id_rsa


#ref: https : //diego .assencio .com/?index=771ffcf2efe6efe3e934d81cb6b8bbfb
```
When you connect to "singleserver", the parameters set under the singleserver section will take precedence as they are defined 
earlier in the config file. In other words, the MAC used will be hmac-sha1 (HMAC with SHA-1 as hash function) 
instead of hmac-sha2-512. Similarly, the block cipher used will be aes128-cbc (AES with CBC as mode of operation and 128-bit long keys) 
instead of aes256-ctr, and so on.

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
