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

# ConnMan

ConnMan is a command-line network manager designed for use with embedded devices and fast resolve times. It is modular through a plugin architecture, but has native DHCP and NTP support.[1]

## Enabling and disabling WiFi

To check if WiFi is enabled you can run `connmanctl technologies` and check for the line that says `Powered: True/False`. To power the WiFi on you can run `connmanctl enable wifi` or if you need to disable it you can run `connmanctl disable wifi`. Other ways to enable WiFi could include using the `Fn` keys on the laptop to turn it on or running `ip link set _interface_ up`.

## Connecting to an open access point

To scan the network `connmanctl` accepts simple names called _technologies_. To scan for nearby Wi-Fi networks:

```
$ connmanctl scan wifi

```

To list the available networks found after a scan run (example output):

```
$ connmanctl services
```

```
*AO MyNetwork               wifi_dc85de828967_68756773616d_managed_psk
    OtherNET                wifi_dc85de828967_38303944616e69656c73_managed_psk 
    AnotherOne              wifi_dc85de828967_3257495245363836_managed_wep
    FourthNetwork           wifi_dc85de828967_4d7572706879_managed_wep
    AnOpenNetwork           wifi_dc85de828967_4d6568657272696e_managed_none

```

To connect to an open network, use the second field beginning with `wifi_`:

```
$ connmanctl connect wifi_dc85de828967_4d6568657272696e_managed_none

```

## Technologies

Various hardware interfaces are referred to as _Technologies_ by ConnMan.

To list available _technologies_ run:

```
$ connmanctl technologies

```

To get just the types by their name one can use this one liner:

```
$ connmanctl technologies | awk '/Type/ { print $NF }'

```

**Note:** The field `Type = tech_name` provides the technology type used with `connmanctl` commands

To interact with them one must refer to the technology by type. _Technologies_ can be toggled on/off with:

```
$ connmanctl enable technology_type

```

and:

```
$ connmanctl disable technology_type

```

For example to toggle off wifi:

```
$ connmanctl disable wifi

```

**Warning:** connman grabs rfkill events. It is most likely impossible to use `rfkill` or `bluetoothctl` to (un)block devices, yet hardware keys may still work.[\[2\]](https://git.kernel.org/cgit/network/connman/connman.git/tree/doc/overview-api.txt#n406) Always use `connmanctl enable|disable`

### Tips and tricks

**Tip:** Network names can be tab-completed.

You should now be connected to the network. Check using `connmanctl state` or `ip addr`.

# Links

- https://github.com/victronenergy/venus/wiki/commandline---development
- https://github.com/victronenergy/venus/wiki/commandline---operational
- https://cr.yp.to/daemontools/svc.html
- https://wiki.archlinux.org/title/ConnMan

