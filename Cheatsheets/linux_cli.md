### Zellij binds
- `Ctrl+P; Z` - switch to compact layout
- ` `

### Command chaining
```
command1 & command2 & command3
command1; command2; command3
command1 && command2 && command3      # Run only if the previous was successful (AND)
command1 || command2 || command3      # Run only if the previous failed (OR)

```
`echo $?` returns 0 if successful and something else otherwise. See bottom for exit codes.

### Package managers
```
which         # To check if something is installed
apt <search/show/install/remove/autoremove/update/upgrade>
apt purge <package_name>   # Removes leftover package config files
dpkg
```

### Redirects and pipes
```
>          # Replaces file content with all command output (STDOUT)
<          # Redirects file content into command (opposite of above)
>>         # Appends to file instead of replacing (STDOUT)
2>         # Sends `STDERR` to file (0=STDIN, 1=STDOUT, 2=STDERR file descriptors)
&>         # Redirects both `STDOUT` and `STDERR` to the same file

find / -name 'sample.txt' > a.txt 2> /dev/null
find ${1} -type f | xargs stat --format '%Y :%y %n' 2>/dev/null | sort -nr | cut -d: -f2-

|          # Pipes output without errors
&|         # Pipes output with errors

ls -l /etc/ | head -n 20 | tail -n 5
```

### Command history
- Configuration options for the history command are found in `.bashrc`, and can be modified to hold more or less values; the defaults are generally sufficient, though
```
history | less
!<history number>
!-1                            # Execute previous command (relative)
!!                             # Same as !-1
!cat                           # Execute last cat command
!cat ^file1.txt^fileA.txt^     # Replace file1.txt with fileA.txt
```

### Environment variables
- By convention, all environment variables are uppercase; you can create one that isn't, but you should probably stick to the rule unless you're using it in a script
- ENVs can be:
    - **Global** - technically called environment variables, can be accessed by anything executing in that shell
    - **Local** - technically called shell variables, can be executed only by the subshell
- `PATH=` contains locations that are to be searched in order to find commands to be executed; when you type a command in shell, it'll search if it is built into the shell, and if it isn't - it will search these directories in order
- including the current working directory (`myvar="$PWD"`) is a common modification, but is not a great practice as an attacker could create a trojan disguised as a command in your pwd
```
printenv
COUNT_LOCAL=69       # Assigns a local (shell) variable
echo $COUNT_LOCAL    # Prints COUNT_LOCAL ENV
export <ENV=...>     # Assigns global ENVs
unset <ENV>
```

### Command substitution
```
ls -l 'cat file-list.txt'    # Output of cat as input for ls
ls -l $(cat file-list.txt)

ls -l cat file-list.txt      # Takes cat and .txt as ls input
```

### Startup files
Interactive non-login shell is what's used by default. Its startup file is `.bashrc`, in the user's home directory.
- `alias` = assignign an alias to a command, i.e. `ll='ls -alF'`
	- users generally add an alias for rm to provide recursivity and a prompt for deletion; add it through `alias del='rm -rfi'` (recursive, forced, interactive)
- `source .bashrc` to make the shell reread its startup file after modification
- other shells use different startup files; cshell uses `.cshrc`; the syntax may differ across shells

### Linking (i.e. based Linux shortcuts, i.e.2 pointers to files)
Hard link:
- `ln` creates hard links by default
- Points to the physical location of the file on storage, hence, if the original file is moved/delete, the hard link continues to work
- Can't create a hard link to a file on a different filesystem
- The file is completely removed when there are no hard links to it
- Hard links towards directories can't be created as they can lead to a circular loop in the filesystem
- Limited use nowadays, to be avoided if possible

Symbolic/soft link:
- A lot like a shortcut in Windows
- Points to the file in the filesystem, not on storage, so the symbolic link won't work if the resource is moved/deleted
- Works across filesystems
- Doesn't understand relative pathing; absolute pathing must be used
- Different coloration through `ls`, usually displays an arrow too
```
ln <options> <target> <link_name>
ln -s <target> <link_name>           # Symbolic/soft link
```

### Filesystem
- `sda` - `s` = SCSI/SATA, `d` = disk, `a` = A
    - `sda1` - SATA disk A, partition 1
- `ext4` - default filesystem for Linux, offers journalism services that help prevent losing data if the system loses power for some reason
- most mounted devices are temporary filesystems used by the OS to function, like `/proc`
```
man hier

[less/vim/cat] /etc/fstab         # Drive mounting config file
mount <optional: arguments>       # No args = shows currently mounted devices
df -h                             # Display filesystem disk usage (h=human)
du -sh <file>                     # Display file disk usage
```

### Searching the filesystem
```
find <startpoint> <expression>
find . -name 'file*.txt'
find . -iname 'file*.txt'    # Case insensitive

locate file.txt              # Uses a databse, generally faster than find
<sudo> updatedb              # Updates filename database

which <command>
whereis <command>
type <command>
```

### Users and permissions
- Normal user accs start with user ID 1000
- `nolong/false` are used 
```
whoami
hostname

users
who
w

less /etc/passwd - user:x(hash, now found in /etc/shadow)/*(disables login but keeps acc info):unique ID (0 for root, normal accounts start from 1000):unique ID of user's primary group:additional info:home dir:default shell (nologin and false are used to disable login for security)
less /etc/group - group:x(password, not used anymore):group unique ID:usernames of members (adm = users that can escalate privileges to admin (sometimes also called wheel group), sudo = users that can run commands as any users in any groups)
```
- `drwxrwxr-x 2 bob bob`
    - `d` = directory, `l` = symlink, `-` = regular file, `c` = character device (allows users/programs to communicate with hardware peripheral devices), `p` = named pipe, `s` = socket (used for communication between processes, by services like syslog, xorg etc), `b` = block device (similar to `c`, mostly governing hard drives, memory), more in `man ls`
    - first `rwx` - owner's permissions (first `bob` denotes the owner)
    - second `rwx` block - permissions for other users in owner's group (second `bob` denotes the group)
    - final `rwx` block - permissions for all other users
        - `x` on a directory determines whether the user can execute commands within that directory; if you don't have the perms, you won't be able to use ls on that directory
    - `2` = number of hardlinks
    - Octal mode > symbolic mode
```
chmod 467 file.txt           # r--rw-rwx
r = 2**2, w = 2**1, x = 2**0

chmod g-w file.txt           # Removes group write permissions
chmod a=,u=r file.txt

chown <user>:<group(optional)> <file>
chgrp <group> <file>

sudo <-u user> <command>
su <user>

passwd
sudo passwd <user>
```

### Text management
```
sed 's/Suite/Ste/' text.txt     # Replaces Suite with Ste, once per line
sed 's/Suite/Ste/g' text.txt    # Replaces all instances of Suite with Ste (g=global)
sed '$s/Suite/Ste/g' text.txt   # Replaces last match found in stream
sed '/Suite/d' text.txt         # Deletes all matches
sed '/ee/ s/Suite/Ste/g'        # Matching criteria, then command for substitution; search for lines with 'ee' and replace expression
sed 's/$/\n/g'                  # Replaces all endlines with newline
sed -e 's/$/\n/g' -e 's/,.\n/g' # Replaces all endlines and commas with newline

awk '{print $3, "likes", $1}'
awk -F ',' '{print $1}' sample.txt | awk '{print $2 ", " #1}'
awk -F ',' '/Dakota/ {print NR,$1}' sample.txt    # Print only people in Dakota, add record number to the front (integrated awk variable)

tr                              # Takes STDIN and replaces characters
<...> | tr ',' '\t'             # Replace , with tab
<...> | tr 'a-z' 'A-Z'
<...> | tr '[:lower:]' '[:upper:]'


head <-<number> -n=number every line, -b=number nonempty lines> <file>      # Defaults to 10
tail <-<number>> <file>      # Defaults to 10, 
tail -f /var/log/auth.log    # Watches file for changes

diff
grep <-n=shows line, -i=case insensitive, -r=recursive, -v=exclude, -w=treat expression as word (not just string)> <expression> <location>
cut
more
less        # In interface, /<keyword> to search, n and N for next/prev
vim -
cat
strings
xxd
base64
sort
uniq (sort -u is the same)   # uniq only filters adjacent lines; sort before uniq
wc <-l>
awk
watch
```

### Compressing/archiving files
```
tar <c=create, x=extract, t=view without extracting, r=add single files to tar><z=gzip archiving - compression><v=verbose><f=use the archive as a parameter(its name)> <tar name/path> <filenames/filename> <--directory $path>
# tar does archiving and compressing in 2 steps, zip does it in one

zip <-r = include files in directories> archive_name.zip <files>
zip -sf archive_name.zip              # Show zip contents, sf=show files
unzip <-l = list files, don't unzip archive_name.zip <filename for individual unzip> <-d directory>
zip -u zipname.zip [files] //add files individually

tar <cvf / tvf / xvf / cvfz <filename.tar.gz>> <file> <files>
gzip <filename.tar>
gunzip <filename.tar.gz>
bzip2
```

### Getting help ([explainshell.com](https://explainshell.com/))
```
man <command>
info <command>
<command> --help
```

### Wildcards
```
*, **, ?, []
ls file*.txt
ls */file*.txt
ls **/file*.txt        # Searches in subdirs too; if this doesn't work, do shopt -s globstar

ls file[123].txt
ls file[a-zA-Z].txt

ls exploi?.py          # Fill in a single char
```

### Getting around spaces in the CLI
```
cat file\ name.txt
cat 'file name.txt'
```

### Shells
- [Bash](https://www.gnu.org/software/bash/manual/bash.html) - the default on most systems, usually also the one used for scripting; it will always be available (i.e. even when remotely connected into another system)
- Zshell - more modern, based
- csh/ecsh
- ksh

### Filesystem navigation
```
pwd
cd
ls
mkdir
rmdir
cp
mv
touch
```

### Networking commands
```
ping <-c>
ifconfig
ip
route
nslookup
dig
netstat
```

### File transfer
```
scp <sourcepath> <destination path>
scp file.txt 192.142.0.2:/home/acordeon/
scp -r ctf@192.142.0.2:/home/acordeon/directory /home/ctf/yeah

rsync -avzh <file> <destination>    # Very capable, it syncs files so it doesn't copy unnecessarily
```

### Absolute and relative paths
- **Absolute** = from root
	- `/home/bob/files/notes.txt`
- **Relative** = from current working directory
	- `../../home/bob/files/notes.txt`
	- Usually shorter and easier to use than absolute paths
	- Relative paths tend to be more portable across systems and hence are preferred for general use
	- If you don't have a need for an absolute path, use a relative path

### Other commands and info
```
bob@linux101:~$ 
user@hostname:[homedir][luser, not root; # denotes root, $ denotes luser]
```
- `ctr+alt+f2` for GUI tty (teletype, alias for terminal - tty2 is GUI tty), `f3-f6` for tty3-6, tty1 is GUI login screen
```
fc-list          # List fonts
fc-match         # Search for font
```

### Exit codes
```
Exit status:
    0      if OK,
    1      if minor problems (e.g., cannot access subdirectory), 
    2      if serious trouble (e.g., cannot access command-line argument).

echo $?    # check exit status
```

### Resources
- [TCM Academy Linux 101](https://academy.tcm-sec.com/courses/1552914)
- [Operating Systems Usage](https://ocw.cs.pub.ro/courses/uso/) (in Romanian)
- [Linux filesystem hierarchy standard](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)