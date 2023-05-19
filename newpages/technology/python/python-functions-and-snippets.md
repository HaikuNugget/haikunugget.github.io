---
layout: page
title: Python functions and snippets
permalink: /newpage/technology/python-functions-and-snippets
order: 32
---

This page has wordy crocs; use at your own risk.


```bash

```

#### generate some terraform from a comma separated list of CIDRs
example content for csl.txt
```bash
192.168.10.0/23,10.20.30.0/23,172.16.32.0/23,
1.0.0.0/24,2.0.0.0/24,3.0.0.0/24,
11.0.0.0/24,12.0.0.0/24,14.0.0.0/24
abcd:1234::/32
```

```bash
#!/usr/bin/env python3

file_comma_spearated_list = "./csl.txt"
my_ip_string = ""

csl_file_content = []
csl_join_file_content = []

with open(file_comma_spearated_list) as fh:
    for line in fh:
        csl_file_content = fh.readlines()

for i in csl_file_content:
    csl_join_file_content.extend(i.split(','))
cidr_list_not_empty = [i for i in csl_join_file_content if (i != "\n" or "" or None)]
cidr_list_subnets_only = [i.strip() for i in cidr_list_not_empty]

print('cidr_list_subnets_only')
print(cidr_list_subnets_only)

for tf_blob in cidr_list_subnets_only:
    # note the multi \\ is for rendering in jekyell
    #text = """
    #make = \{cidr} 
    #    \{\{pretty = terraform
    #      \{\{ pretty_terraform = goes_here
    #         pretty_ciders = \{cidr}
    #    
    #    }}
    #was
    #\{port\}"""

    print(text.format(cidr=tf_blob, port=22))

    print("""===================delete after using udpated "text" command =============================""")

```


#### Setup for recursive delete of a directory structure or file blob (buckets, S3, etc.)
```bash
>>> var1
'/mnt'
>>> alist
['/mnt/data/data1', '/mnt/data', '/data/partition', '/data']
>>> alist.append(var1)
>>> alist
['/mnt/data/data1', '/mnt/data', '/data/partition', '/data', '/mnt']
>>> alist.sort()
>>> alist
['/data', '/data/partition', '/mnt', '/mnt/data', '/mnt/data/data1']
>>> for i in range(0,len(alist)): print(alist.pop())
...
/mnt/data/data1
/mnt/data
/mnt
/data/partition
/data
```
