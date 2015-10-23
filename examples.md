Examples
========
To _install_ `check_collectd` copy the perl script somewhere near your `nagios`
plugins, e.g.
```bash
pushd /tmp
git clone https://github.com/lindenbaum/check_collectd.git
cp check_collectd/check_collectd /usr/lib64/nagios/plugins/
rm -r check_collectd
popd
```

Now assume you have the following command defined in your nagios configuration:
```
# This command checks the current entropy.
# Warning on values below 200, Critical on values below 100.
define command {
  command_name check_entropy
  command_line /usr/bin/collectd-nagios -s /var/run/collectd-unixsock -H '$HOSTNAME$' -n 'entropy/entropy' -g none -c 100: -w 200:
}
```
In a critical condition something like the below would show up as service output
in your nagios frontend (or in alerts):
```
CRITICAL: critical 1, warning 1, okay 0
```
...not very helpful indeed.

With `check_collectd` your configuration would look almost the same:
```
# This command checks the current entropy.
# Warning on values below 200, Critical on values below 100.
define command {
  command_name check_entropy
  command_line /usr/lib64/nagios/plugins/check_collectd -s /var/run/collectd-unixsock -H '$HOSTNAME$' -n 'entropy/entropy' -g none -c 100: -w 200: -f '%s: Entropy fine (%i)' -F '%s: Entropy too low (%i)'
}
```
But the critical condition service output in the frontend (and in alerts) would
this:
```
CRITICAL: Entropy too low (90)
```
...brave new world.
