# toke
[toke](https://github.com/9beach/toke) is a [aria2c](https://aria2.github.io) command-line client written in [Python](https://www.python.org).

`toke` is largely influenced by [diana](https://github.com/baskerville/diana).
Although largely rewritten, you can easily find the soul of `diana` in my code. 

## Installation

For _Microsoft Windows_ users, first [install Python](https://www.python.org/downloads/), 
and then just copy [toke](https://github.com/9beach/toke/blob/main/toke) to your favorite directory.

For _OSX_ and _Linux_ users, just copy `toke` to your favorite directory
in `$PATH`, and `chmod 755 toke`. 

At last, make your `.toke` file in your home directory. A typical `.toke` is 
like this.

```
❯ cat ~/.toke
host: 192.123.134.244
secret: 9898
port:6800
```

You know what is this. For `toke` to connect to your `aria2c`, you need to run `aria2c` in daemon mode like this, `aria2c --enable-rpc --rpc-listen-all`.

## Usages

For _Microsoft Windows_ users, you need to type `python c:/path-to/toke` in Command Prompt to run `toke`. But in all the usages below, I just type `toke` for _OSX_ and _Linux_ users.

```
❯ toke
Usage: toke ACTION [OPT] [...]

Actions
    add [OPT ...] ITEM [...]    Download the given items (local or remote URLs
                                to torrents, etc.).
    l/list                      Print list of active downloads.
    lc/list-compact             Print compact list of active downloads.
    ll/list-long                Print long list of active downloads.
    lj/list-json                Print JSON response of active downloads.
    p/paused                    Print list of paused downloads.
    pc/paused-compact           Print compact list of paused downloads.
    pl/paused-long              Print long list of paused downloads.
    pj/paused-json              Print JSON response of paused downloads.
    s/stopped                   Print list of stopped downloads.
    sc/stopped-compact          Print compact list of stopped downloads.
    sl/stopped-long             Print long list of stopped downloads.
    sj/stopped-json             Print JSON response of stopped downloads.
    e/errors                    Print the list of errors.
    f/files GID [...]           Print files of the given GIDs.
    st/stats                    Print download bandwidth statistics.
    so/show-options             Print global options of aria2.
    clear/clean                 Stop seeding completed downloads.
    co/change-options OPT [...] Change global options of aria2 dynamically.
    forcerm GID [...]           Forcibly remove downloads of the given GIDs.
    pause GID [...]             Pause the downloads of the given GIDs.
    pg/purge                    Clear the list of stopped downloads and errors.
    re/resume GID [...]         Resume the downloads of the given GIDs.
    rm/remove/stop GID [...]    Remove the downloads of the given GIDs.
    shutdown                    Shut down aria2.
    sleep                       Pause all the active downloads.
    wake                        Resume all the paused downloads.

Examples
    toke list
    toke lc
    toke add /path-to/file.torrent "magnet:?xt=urn:btih:FFC7E738EAA4CD4EC..."
    toke add :dir=/mnt/mov :select-file=1,3-5 /path-to/file.torrent
    toke add :out=dad.png "https://my-webserver/pic/IMG_0422.png"
    toke pause 9ba47a7aa365473f cdacca57aef44ec6
    toke co :max-tries=6 :max-concurrent-downloads=50
    toke files f81834e2c0d8d02f 615c023cf658b2a4

For more information on OPT in `change-options` and `add` actions, please see
<http://aria2.github.io/manual/en/html/aria2c.html#aria2.changeGlobalOption>
and <http://aria2.github.io/manual/en/html/aria2c.html#aria2.addTorrent>.
```

```
❯ toke add sintel.torrent
added: b8aaa40985558c64
```

```
❯ toke add *torrent
added: 0ad16a169a3f4278
added: e2d09371458ab4ab
added: 41654d88bcb56854
```

```
❯ toke list
 12.6G	 90.5%	   n/a	  0KB/s	My.Mysterious.Movies.1978.1080...-HELLO[myworld]
263.6M	 40.7%	   40s	3.9MB/s	Big Buck Bunny
210.6M	 57.9%	   27s	3.3MB/s	Cosmos Laundromat
123.3M	 99.8%	    5s	 40KB/s	Sintel
```

```
❯ toke stats
download: 25MB/s, upload: 0KB/s, active: 9, stopped: 8, waiting: 0
```

After finishing downloading 3 files above.

```
❯ toke stopped
123.3M	100.0%	   n/a	  0KB/s	Sintel
210.6M	100.0%	   n/a	  0KB/s	Cosmos Laundromat
263.6M	100.0%	   n/a	  0KB/s	Big Buck Bunny
```

You can easily find what the usages below are.
```
toke list
toke lc
toke add /path-to/file.torrent "magnet:?xt=urn:btih:FFC7E738EAA4CD4EC..."
toke add :dir=/mnt/mov :select-file=1,3-5 /path-to/file.torrent
toke add :out=dad.png "https://my-webserver/pic/IMG_0422.png"
toke pause 9ba47a7aa365473f cdacca57aef44ec6
toke co :max-tries=6 :max-concurrent-downloads=50
toke files f81834e2c0d8d02f 615c023cf658b2a4
```

I'll comment several usages only.

You can change saved file name with `:out=name` when you `toke add`.

```
toke add :out=dad.png "https://my-webserver/pic/IMG_0422.png"
```

You can change temporary downloading directory with `:dir=new_folder_name` when you `toke add`. And also
you can select specific files to download in this way `:select-file=1,3-5`.

```
toke add :dir=/mnt/mov :select-file=1,3-5 /path-to/file.torrent
```

You can change `max-concurrent-downloads` and `max-tries` options of
`aria2c` server dynamically with `toke`. But when you restart `aria2c` server, it recovers the original options in the `aria2.conf`.

```
toke change-options :max-tries=6 :max-concurrent-downloads=50
```

You can list all the global options of `aria2c` with the command `toke show-options`.

For more information on OPT (variable with colon prefix, i.e., the global options of `aria2c`) in `change-options` and `add` actions, please see
<http://aria2.github.io/manual/en/html/aria2c.html#aria2.changeGlobalOption>
and <http://aria2.github.io/manual/en/html/aria2c.html#aria2.addTorrent>.
