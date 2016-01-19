ipt-firewall
============

Simple IPTABLES firewall control structure.

```
Usage: firewall [options] <command>

    -6        enable IP6TABLES
    -e        enable EBTABLES
    -i        log illegal lines in source files
    -l INT    log level (2-6), default 4
    -s        do not print firewall output
    -h        print this help message

Commands:
    start     clear and load firewall rules
    stop      clear rules
    restart   alias for 'start'
```

Rules
-----

Files with rules go in ```/etc/firewall.d/```, only files ending with ```.rules``` are
processed.

All lines containing the following patterns are excluded:

* ```.*;.*```
* ```\$\(.*\)```
* ```([&]|[|])```
* ```[\`]```

Only lines starting with ```${``` are processed.

An IPv4 rule could be defined as:
```${IPT4} -A INPUT -p tcp --dport 80 -j ACCEPT```

### Supported Commands:

* **```${IPT4}```**: ```/sbin/iptables```
* **```${IPT6}```**: ```/sbin/ip6tables```
* **```${EBT}```**: ```/sbin/ebtables```

Policies
--------
The IPTABLES policies are set to ALLOW in the main script. At the end of the
main script after all rules have finished loading, the default chains (INPUT,
OUTPUT, FORWARD) are appended with DROP. While acting similar to policies, this
avoids accidental lockout by flushing the rules.
