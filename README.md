check_collectd
==============
A nagios plugin to check/query values provided by the local
[collectd](https://collectd.org/) daemon. It's implemented as a drop-in
replacement for `collectd-nagios` understanding/passing all of its command
line arguments.

Why?
----
If you already use [collectd](https://collectd.org/) and
[nagios](https://www.nagios.org/) you'll most probably use the
[collectd-nagios](https://collectd.org/documentation/manpages/collectd-nagios.1.shtml)
utility to query values from your local `collectd` daemon. In this case you (most
probably) also are annoyed by the completely useless service output line
provided by this utility. Most people can't see the context in something like
`CRITICAL: 1 critical, 0 warning, 0 ok`. This is where `check_collectd` will
come in handy.

What?
-----
`check_collectd` does exactly what `collectd-nagios` does but allows you to
specify [Perl](https://www.perl.org/) `sprintf` format strings for its output.
While `-f` specifies the format string for the service status __OK__, the `-F`
switch specifies the format string used for service status __Warning__ and
__Critical__. The __Unknown__ service status is outputted as returned from
`collectd-nagios`.

How?
----
If provided, `check_collectd` will call `printf` with the appropriate format
string providing the service status string as first argument. The following
arguments depend on the specified `collectd-nagios` _consolidation function_.
If the consolidation function is `none` the following arguments are the values
(in most cases floats/doubles) found in the performance data field. If one of
the other consolidation functions is used (either `average`, `sum` or
`percentage`), the respective value (also a float/double in most cases) is
passed as the second argument following the performance data values as in the
case of `none`. Feel free to use or not use all or any values in your format
string.

Examples?
---------
To see some example nagios commands refer to `examples.md`.

Prerequisites?
--------------
`collectd-nagios` must be in the user's `PATH` which is the default if
`collectd` was installed via package managers. Queries using `collectd-nagios`
must be working (e.g. you configured the
[unixsock](https://collectd.org/documentation/manpages/collectd-unixsock.5.shtml)
plugin correctly). Although, `check_collectd` was tested using _collectd-5.5.0_
and _perl-5.20.3_, it most probably also works with older versions of both tools.
