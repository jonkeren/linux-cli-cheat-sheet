# linux-cli-cheat-sheet
My personal clipboard of handy commands that I use frequently.

### Linux find largest files
`find . -xdev -type f -size +100M -print | xargs ls -lh | sort -k5,5 -h -r | head`

Search only for files (-type f) in the current working directory (.), larger than than 100MB (-size +100M), don’t descend directories on other filesystems (-xdev) and print the full file name on the standard output, followed by a new line. The output of the find command is piped to xargs which executes the ls -lh command that will print the output in long listing human-readable format, and sorts lines based on the 5th column (-k5,5), compare the values in human-readable format (-h) and reverse the result (-r).
head : prints only the first 10 lines of the piped output.

### find string of text in all files
`grep -rnw '/path/to/somewhere/' -e "pattern"`
