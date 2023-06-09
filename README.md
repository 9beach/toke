# toke
[toke](https://github.com/9beach/toke) is a [aria2c](https://aria2.github.io)
command-line client written in [Python](https://www.python.org).

`toke` is mainly influenced by [diana](https://github.com/baskerville/diana).
Although largely rewritten, you can easily feel the soul of `diana` in my code.

## Installation

For _Microsoft Windows_ users, first
[install Python](https://www.python.org/downloads/), and then just copy
[toke](https://raw.githubusercontent.com/9beach/toke/main/toke)
to your favorite directory. In UNIX-like OSes (MacOS, Linux, BSD), you can 
install `toke` in the following way:

```
sudo curl -L https://raw.githubusercontent.com/9beach/toke/main/toke -o /usr/local/bin/toke
sudo chmod a+rx /usr/local/bin/toke
```

At last, make your `.toke` file in your home directory. A typical `.toke` is
like this.

```
❯ cat ~/.toke
host: 192.123.134.244
secret: 9898
port:6800
```

You know what is this. If there is no `.toke` file, `toke` tries to read the
environment variables, `TOKE_HOST`, `TOKE_PORT`, and `TOKE_SECRET`.

For `toke` to connect to your `aria2c`, you need to run `aria2c` in daemon mode,
e.g, `aria2c --enable-rpc --rpc-listen-all`.

## Usages

For _Microsoft Windows_ users, you need to type `python c:/path-to/toke` in
Command Prompt to run `toke`. But in all the usages below, I just type `toke`
for _OSX_ and _Linux_ users.

```
❯ toke
Usage: toke ACTION [OPT] [...]

Actions
    add [OPT ...] ITEM [...]    Download the given items (local or remote URLs 
                                to torrents, etc.).
    list/l                      Print list of active downloads.
    list-compact/lc             Print compact list of active downloads.
    list-long/ll                Print long list of active downloads.
    list-json/lj                Print JSON response of active downloads.
    paused/p                    Print list of paused downloads.
    paused-compact/pc           Print compact list of paused downloads.
    paused-long/pl              Print long list of paused downloads.
    paused-json/pj              Print JSON response of paused downloads.
    stopped/s                   Print list of stopped downloads.
    stopped-compact/sc          Print compact list of stopped downloads.
    stopped-long/sl             Print long list of stopped downloads.
    stopped-json/sj             Print JSON response of stopped downloads.
    errors/e                    Print the list of errors.
    files/f GID [...]           Print files of the given GIDs.
    stats/st                    Print download bandwidth statistics.
    show-options/so             Print global options of aria2.
    change-options/co OPT [...] Change global options of aria2 dynamically.
    clean/clear                 Stop seeding completed downloads.
    forcerm GID [...]           Forcibly remove downloads of the given GIDs.
    pause GID [...]             Pause the downloads of the given GIDs.
    purge/pg                    Clear the list of stopped downloads and errors.
    resume/re GID [...]         Resume the downloads of the given GIDs.
    stop/remove/rm GID [...]    Remove the downloads of the given GIDs.
    sleep                       Pause all the active downloads.
    wake                        Resume all the paused downloads.
    shutdown                    Shut down aria2.

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
❯ toke list-compact
 12.6G	 90.5%	   n/a	  0KB/s	My.Mysterious.M...ELLO[myworld] 05b8b08a5d847095
263.6M	 40.7%	   40s	3.9MB/s	Big Buck Bunny                  4ec9cb9e0912eaac
210.6M	 57.9%	   27s	3.3MB/s	Cosmos Laundromat               05b8b08a5d847095
123.3M	 99.8%	    5s	 40KB/s	Sintel                          e2d09371458ab4ab
```

```
❯ toke files 05b8b08a5d847095 4ec9cb9e0912eaac
05b8b08a5d847095:
[X]  1  100.0%  /Torrents/incompletes/Cosmos Laundromat/Cosmos Laundromat.en.srt
[X]  2  100.0%  /Torrents/incompletes/Cosmos Laundromat/Cosmos Laundromat.es.srt
[X]  3  100.0%  /Torrents/incompletes/Cosmos Laundromat/Cosmos Laundromat.fr.srt
[X]  4  100.0%  /Torrents/incompletes/Cosmos Laundromat/Cosmos Laundromat.it.srt
[X]  5  100.0%  /Torrents/incompletes/Cosmos Laundromat/Cosmos Laundromat.mp4
[X]  6  100.0%  /Torrents/incompletes/Cosmos Laundromat/poster.jpg
4ec9cb9e0912eaac:
[X]  1  100.0%  /Torrents/incompletes/Big Buck Bunny/Big Buck Bunny.en.srt
[X]  2  100.0%  /Torrents/incompletes/Big Buck Bunny/Big Buck Bunny.mp4
[X]  3  100.0%  /Torrents/incompletes/Big Buck Bunny/poster.jpg
```

```
❯ toke stats
download: 25MB/s, upload: 0KB/s, active: 4, stopped: 0, waiting: 0
```

Finishing downloads, you can check the downloaded files.

```
❯ toke stopped
123.3M	100.0%	   n/a	  0KB/s	Sintel
210.6M	100.0%	   n/a	  0KB/s	Cosmos Laundromat
263.6M	100.0%	   n/a	  0KB/s	Big Buck Bunny
```

You can easily get to know what the usages below are.

```
toke list
toke lc
toke add /path-to/file.torrent "magnet:?xt=urn:btih:FFC7E738EAA4CD4EC..."
toke add :dir=/mnt/mov :select-file=1,3-5 /path-to/file.torrent
toke add :out=dad.png "https://my-webserver/pic/IMG_0422.png"
toke pause 9ba47a7aa365473f cdacca57aef44ec6
toke resume 9ba47a7aa365473f cdacca57aef44ec6
toke co :max-tries=6 :max-concurrent-downloads=50
toke files f81834e2c0d8d02f 615c023cf658b2a4
```

You can change downloading file name with `:out=new-name` when you `toke add`.

```
toke add :out=dad.png "https://my-webserver/pic/IMG_0422.png"
```

You can change temporary downloading directory with `:dir=new-destination`
when you `toke add`. And also you can specify the files to download like this,
`:select-file=1,3-5`.

```
toke add :dir=/mnt/mov :select-file=1,3-5 /path-to/file.torrent
```

You can change `max-concurrent-downloads` and `max-tries` options of `aria2c`
dynamically with `toke change-options`. But when you restart `aria2c`, it
recovers the original options in `aria2.conf`.

```
toke change-options :max-tries=6 :max-concurrent-downloads=50
```

You can list all the global options of `aria2c` with the command
`toke show-options`.

For more information on OPT (variable with colon prefix, i.e., the global
options of `aria2c`) in `change-options` and `add` actions, please see
<http://aria2.github.io/manual/en/html/aria2c.html#aria2.changeGlobalOption>
and <http://aria2.github.io/manual/en/html/aria2c.html#aria2.addTorrent>.
