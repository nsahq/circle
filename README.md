ipt-firewall
============

Simple firewall control structure for IPTABLES and IP6TABLES.

EBTABLES support is currently experimental.

```
Usage: firewall [options] <command>

    -6        Enable IP6TABLES
    -e        Enable EBTABLES
    -i        Log illegal lines in source files
    -l INT    Log level (2-6), default 4
    -V        Be verbose
    -s        Be silent
    -I STR    Set INPUT chain policy
    -F STR    Set FORWARD chain policy
    -O STR    Set OUTPUT chain policy
    -h        Print this help message and exit
    -v        Show version and exit

Commands:
    start     clear and load firewall rules
    stop      clear rules
    restart   alias for 'start'
```

Rules
-----

Files with rules go in ```/etc/firewall.d/```, only files ending with
```.rules``` are processed.

All lines containing the following patterns are excluded:

* ```.*;.*```
* ```\$\(.*\)```
* ```([&]|[|])```
* ```[\`]```

Only lines starting with ```${``` are processed.

An IPv4 rule could be defined as:
```${IPT4} -A INPUT -p tcp --dport 80 -j ACCEPT```

### Supported Commands

* **```${IPT4}```**: ```/sbin/iptables```
* **```${IPT6}```**: ```/sbin/ip6tables```
* **```${EBT}```**: ```/sbin/ebtables```

### Exclusions

Any user-defined chains that should not be flushed and destroyed at start/stop
can be added to ```/etc/firewall.d/ipt4-chains.exclude``` and
```/etc/firewall.d/ipt6-chains.exclude``` respectively. One chain name should be
defined per line.

Policies
--------
The IPTABLES policies are set to ALLOW in the main script. At the end of the
main script after all rules have finished loading, the default chains (INPUT,
OUTPUT, FORWARD) are appended with DROP. While acting similar to policies, this
avoids accidental lockout by flushing the rules. These pseudo-policies can be
changed through options -I, -F, and -O.
