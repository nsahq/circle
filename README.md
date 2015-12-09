ipt-firewall
============

Simple IPTABLES firewall control structure.

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