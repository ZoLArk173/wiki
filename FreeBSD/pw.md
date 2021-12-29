# `pw`

The `pw` command can create, remove, modify and display system users and groups.

## Add an User

``` shell
pw user add -n zolark173 -c 'User' -d /home/zolark173 -G wheel -m -s /usr/local/bin/bash
```

- `-n zolark173`: login name
- `-c 'User'`: user comment
- `-d /home/zolark173`: home directory
- `-G wheel`: add user to `wheel` group
- `-m`: create and setup home directory
- `-s /usr/local/bin/bash`: login shell

# Webpage

- [How to add users on FreeBSD using adduser or pw - nixCraft (cyberciti.biz)](https://www.cyberciti.biz/faq/how-to-add-users-on-freebsd-using-adduser-or-pw/)