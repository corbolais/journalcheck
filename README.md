journalcheck
============

2015-2019 - Alexander Koch

### A simple replacement for logcheck for usage with journald

Journalcheck aims at being a simple replacement for
[_logcheck_](http://logcheck.org) when using journald for system logs. It calls
`journalctl` to obtain all messages that have been recorded since its last
invocation, pipes the output through `egrep` with a given set of filters, and
passes the remaining messages to stdout. Journalcheck therefore works with
volatile system logs as well.

## Dependencies
 * systemd (`journalctl`)
 * coreutils (`split`)
 * grep (`egrep`)

## Usage
Journalcheck is best run as regular user (no need for root privileges!)
via cron:
```
MAILTO=user@localhost

# m  h  dom mon dow   command
*/30 *  *   *   *     journalcheck
```

With a local MTA/MDA set up correctly, you will receive all log entries not
matching the white-list by mail. In addition to the ones shipped with
journalcheck, it looks in _~/.journalcheck.d_ for user-defined filters.

## Configuration
Journalcheck is configurable through the following environment variables
(default values in brackets):

 * `JC_FILTERS_GLOBAL` (*/usr/lib/journalcheck*): Directory for system-wide filters
 * `JC_FILTERS_USER` (*~/.journalcheck.d*): Directory for user-defined filters
 * `JC_STATE_FILE` (*~/.journalcheck.state*): Last run timestamp file
 * `JC_NUM_THREADS` (no. of logical CPUs): Number of worker threads to spawn
 * `JC_LOGLEVEL` (0..5): Priority (loglevel) filter

## Help Wanted
As I only have a limited set of machines and applications running
to derive filters from, I rely heavily on contributions in order to provide a
universal filter set. Pull requests are welcome!

## License
Journalcheck is released under the terms of the MIT License, see
LICENSE file.
