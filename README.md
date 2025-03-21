# linux-cli-cheat-sheet
My personal clipboard of handy commands that I use frequently and that I tend to forget.

### Install btop (latest version) quickly
btop is a great terminal based system monitor. It is a fine way to quickly get a thorough picture of the system’s status in the terminal. It's htop+++.
```
mkdir -p btop-install/output
wget -qO - https://github.com/aristocratos/btop/releases/latest/download/btop-x86_64-linux-musl.tbz | tar -xj -C btop-install/output
(cd btop-install/output/btop && sudo make install && sudo make setuid)
rm -rf btop-install
```

### tmux (very) basic use
- Start new session: just `tmux`
- Detach: ctrl+b, d
- List current: `tmux ls`
- Attach: `tmux attach-session -t 0`

### Easy format to search/replace text strings in many files using sed
```
SRC=Old Text here
DST=New Text here
find . *.php -type f -print0 | xargs -0 sed -i "/ipsum/s,$SRC,$DST,g"
```
(/ipsum/ selects lines containing "ipsum" and only on these lines the command(s) that follow are executed.)

### Disk space usage analyser
`ncdu`

### Linux find largest files
`find . -xdev -type f -size +100M -print | xargs ls -lh | sort -k5,5 -h -r | head`

Search only for files (-type f) in the current working directory (.), larger than than 100MB (-size +100M), don’t descend directories on other filesystems (-xdev) and print the full file name on the standard output, followed by a new line. The output of the find command is piped to xargs which executes the ls -lh command that will print the output in long listing human-readable format, and sorts lines based on the 5th column (-k5,5), compare the values in human-readable format (-h) and reverse the result (-r).
head : prints only the first 10 lines of the piped output.

### Linux find largest files and/or directories recursively
`du -a  | sort -n -r | head -n 50`

Finds largest files and directories and lists largest 50, sorted on size.

### Linux find files recursively larger than XXX 
`find . -type f -size +10M`

### find string of text in all files
`grep -rnw '/path/to/somewhere/' -e "pattern"`

### Start Logrotate on Hypernode servers
`/usr/sbin/logrotate -v /data/web/hypernode_logrotate.conf --state /data/web/.logrot_state`

### Simpler display of only bytes used and bytes free in Varnish (with varnishstat)
`varnishstat -f SMA.s0.g_bytes -f SMA.s0.g_space`

### Varnish show updating list of top MISSes
`varnishtop -i BereqURL`

### Varnish show updating list of all requests
`varnishtop -i ReqURL`

### Varnish monitor purge requests
`varnishlog -g request -q 'ReqMethod eq "PURGE"'`

### Send e-mail from command line CLI including setting from: header and bodytext
`mail -a From:MAIL@ADDRESS.NL -s 'Mail Testing lalala' TO@ADDRESS.NL <<< 'Dit is het bericht. Over en sluiten.'`

### Tar complete directory, recursive, including hidden files, while preserving file permissions
`tar -cvpzf FILENAME.tgz . `

### Auto-archive all pages in a sitemap to the Internet Archive's Wayback Machine (archive.org)
Install globally using `npm install --global wayback-sitemap-archive`

then run using: `wsa <SITEMAP_URL>`


### One-line cache warmer using wget
1. Create one directory in home dir using `mkdir ~/warmertmp`.
2. Then run command:
`wget -U "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.0.0 Safari/537.36" --directory-prefix=~/warmertmp --reject jpg,png --reject-regex "(.*)\?(.*)" --spider --recursive --no-directories https://www.DOMAINNAME.nl`

You can also set this up in a cron job if you want.

Explanation:

`--recursive` will force wget to crawl the website recursively.

`--spider` is for "not downloading anything". However, this directive results in files created and deleted. Thus the following is useful:

`--directory-prefix=~/warmertmp` ensures that temporary files will end up in that temp dir.

`--no-directories` will ensure no empty directories are left out after running

`--reject-regex "(.*)\?(.*)"` This will fetch all pages, BUT will disregard everything after the ? -- so good for layered navigation, for example

Optional: `--quiet` is just a good way to silence any output to avoid cron email being sent.

