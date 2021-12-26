# sblock

A bash script that downloads hosts sources in parallel,
sanitizes them, and installs them in /etc/hosts.

It's practically very similar to [hblock](https://github.com/hectorm/hblock).

To migrate from hblock, just run `sudo mv -T /etc/hblock /etc/sblock`.

Dependencies: bash, GNU coreutils, [aria2](https://aria2.github.io).

## Install

If you're an ArchLinux user,
you can install it from the **AUR**:

```
$ paru -S sblock-git
```

Otherwise clone the repository and run `sudo make install`.
Similarly, to uninstall, run `sudo make uninstall`.

This will also install a cron entry in `/etc/cron.daily`, which will
cause sblock to run once a day
(unless you do not have any cron software like cronie or fcron
installed on your machine).

## Usage

Just put some hosts sources in `/etc/sblock/sources.list` and
run sblock:

```sh
$ sblock
````

### Internet connectivity check

If NetworkManager is running on your system, sblock will use it to
wait until internet connectivity is detected on your system.
Otherwise, connectivity check is skipped. You can force-skip this
check by passing the `-n` flag to sblock.

## Config files

Similar to hblock, the hosts file generated by sblock is
configured through 4 files:

```
/etc/sblock/sources.list
/etc/sblock/allow.list
/etc/sblock/deny.list
/etc/sblock/header
```

The only mandatory file is `sources.list`.
The other three are optional.

### sources.list

Just put a list of your sources in the `/etc/sblock/sources.list` file:

```sh
$ cat /etc/sblock/sources.list
https://raw.githubusercontent.com/4skinSkywalker/Anti-Porn-HOSTS-File/master/HOSTS.txt
https://raw.githubusercontent.com/StevenBlack/hosts/master/alternates/porn/hosts
https://raw.githubusercontent.com/soystemd/hosts/master/hosts
```

### allow.list

You can also put hosts that you don't want to block,
in `/etc/sblock/allow.list`, one per line:

```sh
$ cat /etc/sblock/allow.list
1337x.to
www.1337x.to
rarbg.to
www.rarbg.to
imgur.com
www.imgur.com
```

### deny.list

Add additional hosts that you want to block to `/etc/sblock/deny.list` .
If it exists, it's content will be added to the hosts file.

Entries in deny.list are applies *after* the allow.list file.
So if an entry exists in both allow.list and deny.list, it *will* be
added to the hosts file (and thus denied).

```sh
$ cat /etc/sblock/deny.list
ads.reddit.com
nsfw.reddit.com
```

### header

Contents of the file `/etc/slock/header` are added in a block at the
top of your hosts file, and it's home to `localhost` entries.

If the header file doesn't exist, sblock will add the following entries
as the header (if you don't want this, make `/etc/sblock/header` an empty file):

```
::1 localhost
127.0.0.1 localhost
127.0.1.1 $HOSTNAME.localdomain $HOSTNAME
```

In the above block, $HOSTNAME is your machine's hostname,
as provided by `uname -n`.
