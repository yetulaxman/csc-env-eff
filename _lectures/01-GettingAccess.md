---
title:	Using CSC Environment Efficiently
author:	CSC Training
date:	2020-10-01
lang:	en
---


# Getting access

- Access is controlled by user accounts and projects
- User accounts are created as a self service at [my.csc.fi](https://my.csc.fi)
- Access to services, like Puhti, Mahti and Allas is set by projects
- Projects are also created as self service by *Project Managers*, who
   - Need to fullfill [Project Manager requirements](link)
   - Can apply access to services for the project
   - Can invite users to projects
- [Detailed info on docs.csc.fi](https://docs.csc.fi/accounts/)

# Counters of columns, rows and records	

- awk fields are accessed through variables `$1`, `$2`, …, `$(NF-1)`, `$(NF)`.
    - `NF` (Number of Fields) is the number of fields on each line (# columns in row).
```bash
$ echo "0 1:2,3 4" | awk -F"[:, ]" '{print "entries:" NF " first:" $1 " last:" $NF}'
```
    - `$0` refers to the whole input row.
```bash
awk -F":" '{printf "user: %s\n    whole line: %s\n", $1, $0}' /etc/passwd
```
    - `printf` enables formatted printout - we will discuss in more details later.
    - `NR` (Number of Records) is the number of input records (lines):
```bash
$ awk 'END {print NR}' /etc/passwd
```
    - Much simpler still: `wc -l /etc/passwd`


