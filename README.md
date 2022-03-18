# linux-cli-cheat-sheet
My personal clipboard of handy commands that I use frequently.

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

### Send e-mail from command line CLI including setting from: header and bodytext
`mail -a From:MAIL@ADDRESS.NL -s 'Mail Testing lalala' TO@ADDRESS.NL <<< 'Dit is het bericht. Over en sluiten.'`

### One-line cache warmer using wget
1. Create one directory in home dir using `mkdir ~/warmertmp`.
2. Then run command:
`wget --directory-prefix=~/warmertmp --reject jpg,png --reject-regex "(.*)\?(.*)" --spider --recursive --no-directories https://www.DOMAINNAME.nl`

You can also set this up in a cron job if you want.

Explanation:

`--recursive` will force wget to crawl the website recursively.

`--spider` is for "not downloading anything". However, this directive results in files created and deleted. Thus the following is useful:

`--directory-prefix=~/warmertmp` ensures that temporary files will end up in that temp dir.

`--no-directories` will ensure no empty directories are left out after running

`--reject-regex "(.*)\?(.*)"` This will fetch all pages, BUT will disregard everything after the ? -- so good for layered navigation, for example

Optional: `--quiet` is just a good way to silence any output to avoid cron email being sent.

