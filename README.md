circle
============

circle is a simple iptables/ip6tables and ebtables control script that
clears previous rules and sets up a firewall from user defined rule files.

ebtables support is currently experimental.

```
Usage: circle [options] <command>

    -6        Enable IP6TABLES
    -e        Enable ebtables
    -i        Log illegal lines in source files
    -l INT    Log level (2-6), default 4
    -V        Be verbose
    -s        Be silent
    -I STR    Set INPUT chain policy
    -F STR    Set FORWARD chain policy
    -O STR    Set OUTPUT chain policy
    -P        Use true policy instead of pseudo-policy
    -h        Show this help message and exit
    -v        Show version and exit

Commands:
    compile   compile and print rules
    start     clear and load firewall rules
    stop      clear rules
    restart   alias for 'start'
```


Rules
-----

Rule files (files ending with .rules) are read from etc/firewall.d/ and each
line evaluated before added as a firewall rule. Only lines starting with ${ are
used, and any lines containing any of the patterns ```.*;.*,``` ```\$(.*)```,
```([&]|[|])```, ```[\`]``` are discarded (and produce a log message and/or
warning depending on options used).

A rule that would normally start with iptables should be defined with ${ipt4}
instead. ip6tables and ebtables respectively should use ${ipt6} and ${ebt}.


### Exclusions

Chains that are handled by another application or for any other reason should
not be flushed and destroyed by circle can be excluded in
```ipt4-chains.exclude``` and/or ```ipt6-chains.exclude``` under
```/etc/firewall.d/```. These files takes the name of one chain per line.


OPTIONS
-------

```
   -6     Enable ip6tables in rule files.

   -e     Enable ebtables in rule files. Experimental.

   -i     Log illegal lines encountered in rule files.

   -l LEVEL
          Set log level to LEVEL. Allowed values are 2-6, default is 4. Levels
          are inclusive with all lower levels. The numbers correspond to:
              6 debug
              5 notice
              4 info
              3 warning
              2 error

   -V     Verbose, show all available output.

   -s     Silent, output nothing but a few error messages.

   -I POLICY
          Set a pseudo-policy for the INPUT chain. The actual policy of the
          built-in chains remain ACCEPT, but after loading all rules the chain
          is appended with ´-A  INPUT  -j  POLICY´. Default is DROP.

   -F POLICY
          Set a pseudo-policy for the FORWARD chain. The actual policy of the
          built-in chains remain ACCEPT, but after loading all rules the chain
          is appended with ´-A  FORWARD  -j POLICY´. Default is DROP.

   -O POLICY
          Set a pseudo-policy for the OUTPUT chain. The actual policy of the
          built-in chains remain ACCEPT, but after loading all rules the chain
          is appended with ´-A OUTPUT -j POLICY´. Default is DROP.

   -P     Set actual policy instead of pseudo-policy. Modifies options -I, -F,
          -O.

   -h     Show help and exit.

   -v     Show version and exit.
```


Policies
--------
iptables/ip6tables policies are set to ALLOW by default. After all rules have
been loaded, the default chains (INPUT, OUTPUT, FORWARD) are appended with
catch all rules as pseudo-policies defaulting to DROP. While acting similar to
policies, this avoids accidental lockout by flushing the rules. The
pseudo-policies can be changed through options -I, -F, and -O. Using the option
-P will use real policies instead and modifies the previously mentioned options.
