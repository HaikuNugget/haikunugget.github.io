---
layout: page
title: ansible-snippets-with-expect
permalink: /newpage/technology/ansible-snippets-with-expect
order: 12
---

This page has wordy crocs; use at your own risk.

### How do I run this thing?
```bash
ansible-playbook  -i inventory.yaml pingsometing_and_expect.txt
```

### What will it run against?
```bash
127.0.0.1 hosts as defined under dbhosts in the inventory.yaml file
```

### inventory.yaml
```yaml

dbhosts:
  hosts:
    127.0.0.1
```

### What tool is the expect driving?

```console
expect is driving the bash tool "getinput.sh".

getinput.sh is an example tool taking a 
  single line of input and outputing the input given.
```

The source code is included in the `getinput.sh` example.

### getinput.sh 
example tooling to be driven by expect for use with the playbook
```bash
#!/bin/bash

#echo "Please enter your name: "
read -p "Please enter your name: " STUFF
echo $STUFF
```

### qbf.txt
The text file used for the example editing in sed.

```text
The quick brown fox jumped over the lazy yellow dog.
```

### pingsometing_and_expect.txt <br>
This file is the ansible playbook and does a few things:
  * pings a host command execution example
  * has examples for installing pip on a Debian Linux based machine
  * has examples for installing pexpect as used by ansible for the "builtin" command
  * has examples for collecting facts and disabling fact collection
  * has examples to output gathered facts
  * has example for using the ansible builtin expect (using pexpect) to drive the getinput.sh script
  * has example for applying an inline sed edit and appending a line after a known line to the file (disabled)
    * uses a "safe" example for the inline edit

```yaml
---
- name: "Run a shell command on a remote host"
  hosts: dbhosts
  gather_facts: no
  vars:
    installdir: '/Users/moby/workspacenf/20230426-ansible-expect'
    #executable_filename: "mysqlinstaller.exe"
    executable_filename: "getinput.sh"
    db_port: "3306"
    db_username: "dbadmin"
    db_password: "D@taB@se#123"

  tasks:

  - name: Ping a thing.
    shell:
      ping -c1 localhost 
    args:
      chdir: /tmp
      #warn: false

  - name: Check MacOS system information
    shell:
      "sw_vers"
    register: os_info

  - debug:
      msg: ""

  - name: Print all available facts
    ansible.builtin.debug:
      var: ansible_facts

  - name: Print the full output of "Check MacOS system information"
    ansible.builtin.debug:
      var: os_info

  #- name: Install pip
  #  apt:
  #    update_cache: yes
  #    name:
  #      - python3-pip

  - name: Install pexpect
    pip:
      name: pexpect

  - name: "expect example"
    ansible.builtin.expect:
      echo: yes
      chdir: "{{ installdir }}"
      command: "./{{ executable_filename }}"
      timeout: "300"
      responses:
        (.*)Please enter your name(.*): "YourNameHere"
        (.*)Please enter your age(>*): "21"
    register: command_output

  - debug:
      var: command_output.stdout_lines

  - name: "Sed it up"
    shell:
        cat qbf.txt| sed 's/\(The quick brown fox jumped over the lazy yellow dog.\)/&\nJohn/'
        #/usr/bin/sed -i 's/\(The quick brown fox jumped over the lazy yellow dog.\)/&\nJohn/' qbf.txt
    args:
      chdir: /tmp
    register: command_output2
  
  - debug:
      var: command_output2.stdout_lines
```

