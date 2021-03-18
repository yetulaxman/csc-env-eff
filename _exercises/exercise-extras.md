---
title: Extra excercises
author: CSC Training
---


```bash
psstack () { while [ -n "$1" ]; do echo $1; set -- $(ps -o ppid= $1); done | xargs ps -f -p | column -t; }
```

```bash
pps () { test -n "$1" && ps -o user=,pid=,ppid=,comm= $1 && pps $(ps -o ppid= $1); }
```

```bash
csc-queues () { [ "$1" = '-h' ] && echo "csc-queues [-h|USER ...]" || sacctmgr show -p assoc where user=${1:-$USER} | grep -v default_no_jobs | cut -d '|' -f 2,3,4,8,12,15 | column -t -s '|' && (( $# >= 2)) && shift && csc-queues "$@"; }
```

<https://raw.githubusercontent.com/jlento/miljoonalaatikko/master/bashfuncs/dates.sh>
