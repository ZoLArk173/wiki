# Service

## Basic 模板

## Add `status` Function to a Service

Assign `status` to `extra_commands`

```shell
extra_commands="status"
status_cmd="${name}_status"
```

## Website

- [Allow rc.d script to provide "status" method without having a procname/PID file](https://reviews.freebsd.org/D31614#:~:text=extra_commands%3D%22status%22%0Astatus_cmd%3D%22%24%7Bname%7D_status%22)

- [How to Make a Basic rc.d Script in FreeBSD (sosheskaz.github.io)](https://sosheskaz.github.io/tutorial/2017/03/28/FreeBSD-rcd-Setup.html)

- 
