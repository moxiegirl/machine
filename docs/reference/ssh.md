<!--[metadata]>
+++
title = "ssh"
description = "Log into or run a command on a machine using SSH."
keywords = ["machine, ssh, subcommand"]
[menu.main]
parent="smn_machine_subcmds"
+++
<![end-metadata]-->

# ssh

Log into or run a command on a machine using SSH. To login, use the 
`docker-machine ssh machinename` command, for example:

```
$ docker-machine ssh dev
                        ##        .
                  ## ## ##       ==
               ## ## ## ##      ===
           /""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
           \______ o          __/
             \    \        __/
              \____\______/
 _                 _   ____     _            _
| |__   ___   ___ | |_|___ \ __| | ___   ___| | _____ _ __
| '_ \ / _ \ / _ \| __| __) / _` |/ _ \ / __| |/ / _ \ '__|
| |_) | (_) | (_) | |_ / __/ (_| | (_) | (__|   <  __/ |
|_.__/ \___/ \___/ \__|_____\__,_|\___/ \___|_|\_\___|_|
Boot2Docker version 1.4.0, build master : 69cf398 - Fri Dec 12 01:39:42 UTC 2014
docker@default:~$ 
```
The system displays a splash screen indicating the machine's driver. Then, it
displays the prompt inside the running machine. You can interact with the
command line as in any shell:

```
docker@boot2docker:~$ ls /
Users/   dev/     home/    lib/     mnt/     proc/    run/     sys/     usr/
bin/     etc/     init     linuxrc  opt/     root/    sbin/    tmp      var/
```

To leave an SSH session, type  `exit` at the prompt.

```
docker@default:~$  exit
```

Similar to the regular `ssh` program, you can run commands inside a machine
remotely by passing a command together with the `docker-machine ssh` command line:

```
$ docker-machine ssh dev free
             total         used         free       shared      buffers
Mem:       1023556       183136       840420            0        30920
-/+ buffers:             152216       871340
Swap:      1212036            0      1212036
```

When you run a command, you don't enter the shell. Instead, the command runs in
the shell and exits returning you to your local prompt. You can also pass flags
to the commands you run:

```
$ docker-machine ssh dev df -h
Filesystem                Size      Used Available Use% Mounted on
rootfs                  899.6M     85.9M    813.7M  10% /
tmpfs                   899.6M     85.9M    813.7M  10% /
tmpfs                   499.8M         0    499.8M   0% /dev/shm
/dev/sda1                18.2G     58.2M     17.2G   0% /mnt/sda1
cgroup                  499.8M         0    499.8M   0% /sys/fs/cgroup
/dev/sda1                18.2G     58.2M     17.2G   0%
/mnt/sda1/var/lib/docker/aufs
```

## The SSH implementation

The first time `docker-machine` is invoked, it first checks to see if you have
the `ssh` binary locally. If you do, Machine attempts to use the local `ssh` for
the SSH commands it needs to run. It does this  whether the commands are a part
of an operation such as creation or were requested by the user directly. 

If it does not find a `ssh` binary locally, Machine uses the native Go
implementation from [crypto/ssh](https://godoc.org/golang.org/x/crypto/ssh).
This is useful in situations where you may not have access to traditional UNIX
tools, for example, if you are using Docker Machine on Windows without an
`msysgit` installation.

Typically, you should allow Machine to handle this implementation detail for
you. However, if you want to ensure Machine uses the Go native version, you can.
To do this, set a global command line flag / environment variable like so:

```
$ docker-machine --native-ssh ssh dev
```

If you are using the Go native binary, you can also include additional  `ssh`
arguments to pass through to the `ssh` binary.  You can do this as long as the
arguments do not conflict with the default arguments for the command generated
by Docker Machine. For instance, the following command forwards port `8080`
from the `default` machine to `localhost` on your host computer:

```
$ docker-machine ssh default -L 8080:localhost:8080
```

There are variations between the standard `ssh` behavior and the Go native
version, please report any issues or inconsistencies you come across to [Docker
Support](https://support.docker.com).




