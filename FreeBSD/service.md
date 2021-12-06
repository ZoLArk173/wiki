# Service

## Basic 模板

``` shell
#!/bin/sh

. /etc/rc.subr

name="myservice" # How the service will be invoked from service
rcvar="${name}_enable" # The variable in rc.conf that will allow this service to run
load_rc_config $name # Loads the config file, if relevant.

# Set environment variable
export JRE_HOME="/usr/local"

pidfile="//myservice/myservice.pid"
start_cmd="/home/home/myservice ; echo $name is now running on PID $(cat $pidfile)"
stop_cmd="kill $(cat $pidfile); echo $name is not running. ; rm $pidfile)"
status_cmd="if [ -e $pidfile ]; then echo $ is running on PID $(cat $pidfile). ; return 1; fi; echo MyService is not running. ; return 0"

run_rc_command "$1"
```



## Add `status` Function to a Service

Assign `status` to `extra_commands`:

```shell
extra_commands="status"
status_cmd="${name}_status"
```

## Website

- [Allow rc.d script to provide "status" method without having a procname/PID file](https://reviews.freebsd.org/D31614#:~:text=extra_commands%3D%22status%22%0Astatus_cmd%3D%22%24%7Bname%7D_status%22)
- [How to Make a Basic rc.d Script in FreeBSD (sosheskaz.github.io)](https://sosheskaz.github.io/tutorial/2017/03/28/FreeBSD-rcd-Setup.html)
