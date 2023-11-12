# victron-venus-documentation

# Managing services

```bash
svstat /service/logscript
svstat /service/*
svc -d /service/logscript
svc -u /service/logscript
```

## The svc program

svc controls services monitored by [supervise](https://cr.yp.to/daemontools/svc.htmlsupervise.html).

```
     svc opts services

```

_opts_ is a series of getopt-style options. _services_ consists of any number of arguments, each argument naming a directory used by supervise.

svc applies all the options to each service in turn. Here are the options:

-   \-u: Up. If the service is not running, start it. If the service stops, restart it.
-   \-d: Down. If the service is running, send it a TERM signal and then a CONT signal. After it stops, do not restart it.
-   \-o: Once. If the service is not running, start it. Do not restart it if it stops.
-   \-p: Pause. Send the service a STOP signal.
-   \-c: Continue. Send the service a CONT signal.
-   \-h: Hangup. Send the service a HUP signal.
-   \-a: Alarm. Send the service an ALRM signal.
-   \-i: Interrupt. Send the service an INT signal.
-   \-t: Terminate. Send the service a TERM signal.
-   \-k: Kill. Send the service a KILL signal.
-   \-x: Exit. supervise will exit as soon as the service is down. If you use this option on a stable system, you're doing something wrong; supervise is designed to run forever.

## The svstat program

svstat prints the status of services monitored by [supervise](https://cr.yp.to/daemontools/svstat.htmlsupervise.html).

```
     svstat services

```

_services_ consists of any number of arguments, each argument naming a directory. svstat prints one human-readable line for each directory, saying whether supervise is successfully running in that directory, and reporting the status information maintained by supervise.

## The supervise program
supervise starts and monitors a service.
Interface
     supervise s
supervise switches to the directory named s and starts ./run. It restarts ./run if ./run exits. It pauses for a second after starting ./run, so that it does not loop too quickly if ./run exits immediately.

If the file s/down exists, supervise does not start ./run immediately. You can use svc to start ./run and to give other commands to supervise.

supervise maintains status information in a binary format inside the directory s/supervise, which must be writable to supervise. The status information can be read by svstat.

supervise may exit immediately after startup if it cannot find the files it needs in s or if another copy of supervise is already running in s. Once supervise is successfully running, it will not exit unless it is killed or specifically asked to exit. You can use svok to check whether supervise is successfully running. You can use svscan to reliably start a collection of supervise processes.

# Links

- https://github.com/victronenergy/venus/wiki/commandline---development
- https://github.com/victronenergy/venus/wiki/commandline---operational
- https://cr.yp.to/daemontools/svc.html

