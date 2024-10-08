# CAP *NIX notes
*** ALWAYS CHANGE YOUR PASSWORD ON A NEW SYSTEM***
- `passwd` -change your password

## A brief history
### Unix and Linux
- Unix started back in 1960.
- The first version of Unix OS came out in 1971. 
- Bourne Shell was born in 1975! 

- In the summer of 1991, Linux Torvalds wrote his  first code of today's Linux.  
- In the same year, Richard Stallman   pushed for open source, and GNU was born. 

- 1993 Slackware (became Suse)
- 1993 Few months later, Debian. It is one of the most purest distributions out there +=> Ubuntu forks 2004; Mint in 2005
 
- 1995 RedHat, till today, is probably the most established Commercial 
- Distribution (free forks/versions exist) - > Fedora (2003), CentOS 

- 2000 -Gentoo  ->  Chrome OS (2011)
- 2002 -> Arch

- **2008** - Android is based on Linux kernel (that is still true today)

- 2012 Tizen OS -> used in TVs, Fridges, etc.
- 2013 Firefox Mobile OS 


=====================================================
### A little bit about Microsoft
- founded in 1972 as a company
- in 1980, signs a contract with IBM to write an OS
- 1981 - **PURCHASE** of RIGHTS to what becomes MS-DOS
- 1983 - First Windows
- 1985 - Windows 1.0 (on top of DOS)
- 1995 - Windows 95 (Network)

  <br>

  To run multiple operating systems at once, install `https://www.virtualbox.org/`. Then, you can install a full OS instance and run them simultaneously. It is even possible to emulate an entire network of computers if your host machine has enough resources (RAM +CPU  + DISK space). Remember, you may need to install the addition after you install the OS to expand the VB capabilities. 
.....
## Privileges, owners, users, and groups:
- `whoaim` - command returns who is logging in
- `who` - lists who is logged in to the machine (all users)

### Types of users:
- regular users that by default have rights only to their own user space /home/<user>
- specialized system accounts 
- and root used by the system itself


### Excercise
`cd` - change directory
 - special character `~`  (pronounce: tilda) will get you back to home; <br>
 - NOTE `~` can be used with other programs like cp (copy), mv (move)<br>
 - cd `/` (root directory) will get you to the top of the file system tree called root (don't confuse it with a root user)

RUN:
```
$ cd /
$ ls -la
```


### Let's talk about file system and its structure
If you run the commands above you are in the root (top) of filesystemm. What you see are standard Linux directories. Let's talk about them:
- /var (here you find logs for example)
- /etc (important)
- /proc -> This is a virtual filesystem (run: `cat /proc/cpuinfo`)
- /bin
- /bin
- /boot
- /root
- /usr
- /dev (important)
- /home (very important :) )
- /mnt
 ^^^^
How does this differ from Windows?  You don't see disk letters like in a Windows OS. E.g., C: D: drives are invisible and seamlessly mounted to a file system.

run:
```
$ mount 
$ df -h 
```
- `mount` will print how directories are mounted
- `df` - disk free. This will print different mounts and their utilization.
- `du -h` (this can be very verbose) will show disk utilization (This can be used to find the x biggest files in FS for example)

=====================================================
### Users, groups, privileges:
First let's print the content of the present directory:<br>
`ls` - (list) print the content of the (present) directory
  - useful flags: (they start with -)
  - -a all (shows hidden files that start on . (dot))
  - -l (long)
  - -h human readable
  - -t sort in time 
  - -r reverse (youngest list last)


`pwd` - shows where in the files system you (present working directory)

Run:
```
cd /home
ls -la
```
What do you see? Do you see something like this?
```
drwxr-x--- 41 ed    ed    4096 Aug 18 12:23 ed
```
What is this, and why is it important?

After the first letter are three groups consisting of three lettered positions:
- Fist three: 
    - owner – Owner permissions apply ONLY to the file or directory owner. 
- Second three: 
    - group - The Group permissions apply ONLY to the group (grouped have members) that has been assigned to the file or directory.
- Last three:
    - all users – All users' permissions apply to all other users on the system; this is the permission group that you want to watch.

#### Permissions explained: 
Why letters:  It's easier to read than remember the combination of numbers.
- -r read – The Read permission refers to a user’s capability to read the file's contents. (and nothing else !)
- -w write – The Write permissions refer to a user’s capability to write or modify a file or directory. (OR DIRECTORY)
- -e execute – Execute permission affects a user’s capability to execute a file or view the contents of a directory. ( OR DIRECTORY)

Each letter has a numerical value that, when added together, expresses the permission:
- r = 4
- w = 2
- x = 1
- 
So what is 755? or 600?<be>
- 7= 4+2+1 **The Owner** Can do EVERYTHING (read, write and execute)
- 5 = 4+1 **The group** can Read and Execute. Can NOT modify the files!
- 5= 4+1 **Everyone else**, same as above 
<br>
700 => ONLY OWNER CAN do EVERYTHING. Everyone ELSE can do NOTHING. NOT EVEN THE MEMBERS OF THE GROUP can do anything
<br> 
600 =>  you tell me!

A question worth asking yourself:<br>
What are logs usually set to? (Why are we interested in log?)<br>
HINT run: `ls -la /var/log |grep -i log`


### User management 
#### How do you create a user?
There are two commands: `useradd` and `adduser`.<br> 
What's the difference between them?  User friendlines. `adduser` is a script using `useradd`. <br>
`useradd` uses flags where you can or have to add a home directory, etc.
`adduser` will prompt questions and will create the most commonly used feature, like a home directory

When to us each....

#### How do I add a user to a specific group?
`sudo usermod -a -G examplegroup exampleusername`

#### How to create a new group:
`sudo sudo groupadd new_group`


========================================= <br>
### Setting, and modifying Ownership:
#### Assign or modify group and user ownship 
use `chown` command to set/change ownership for both `user:groups`<br>
Usage:
`chown user:group <Whats_is_being_modified>` 
NOTE: You need proper privileges

#### Modify only the user and file:  
`chown user some.file`

#### Modify only the  group:
`chown :group1 some.file`

#### How about changing ownership of subdirectories and files? <br>
`chown -R someuser:somegroup *.some_extention` 

flag `-r` is used for recursivness.

=======================================
### Setting and modifying file permissions:

Similarly, you can change who can do what with files both on user, owner, and group levels. Use command:
` chmod permission file(s)`
Example: ` chmod 755 my_executable.script`
<br>
or you can add, sustract priviledes using letters:

**who =>**
- u: User, e.g. owner of the file.
- g: Group
- o: Others
- a: All, meaning all of the above.

**Action on permissions:**
- -: Minus sign. Removes the permission.
- +: Plus sign. Grants the  permission.
- =: Equals sign. SET  permission! This will REMOVE other permissions.

**The "which " permissions:**
- r: The read permission.
- w: The write permission.
- x: The execute permission.
<br>
Example: 
`chmod ug+rw,o-x file_name.file_type`

### Mode details on administering user passowrds
`passwd` -change password
  <ul style="list-style-type:none;">
   <li>-e set expiration</li>
  <li>-i make inactive</li>
  <li>-s status</li>
  </ul>


=============================================
### Commands related to files:
what can we do with the file?
- `cp` (copy)
- `mv` (move)
- `rm` (remove - delete)
- `touch` - create an empty file
and more. (in fact there is `more` and `less` command as well :) )
- `cat` - print whats inside a file

================================================
### Some other useful tools:
`grep` - will search for a pattern in files
 - -r recursively, meaning it will go  down into subdirectories
 - -i will ignore the case: <br>

Example: `grep -r -i error * `
                  will find: error, Error, ERROR, ErRoR, etc

find -> `find [path] [options] [expression]` 
- -name pattern
-  -f file
- -d directories
Example: <br>
`find ~ -name "example.txt"` <br>
`find ./ -type f -name "*.txt"`

Example to find a subdirectory called `cap` we would run it like this<br>
`find / -type d -name cap`

NOTE: The directory to start from is root / 
           type is set to the directory 
           and search is for something called "cap"

### Processes
Processes are programs running on top of the OS. Each has an ID (parent and child).
The operating system starts up as process `1` All system processes will have parent PID 1 (systems). There is one that has zero that is splash. 
 `ps` will list process
 -f full list
 -e everything
 Most commonly used:
 `ps -ef` or `ps aux` <br>
`kill -l`
```
 **1) SIGHUP**	 2) SIGINT	 3) SIGQUIT	 4) SIGILL	 **5) SIGTRAP**
 6) SIGABRT	 7) SIGBUS	 8) SIGFPE	 **9) SIGKILL**	10) SIGUSR1
11) SIGSEGV	12) SIGUSR2	13) SIGPIPE	14) SIGALRM	15) SIGTERM
16) SIGSTKFLT	17) SIGCHLD	18) SIGCONT	19) SIGSTOP	20) SIGTSTP
21) SIGTTIN	22) SIGTTOU	23) SIGURG	24) SIGXCPU	25) SIGXFSZ
26) SIGVTALRM	27) SIGPROF	28) SIGWINCH	29) SIGIO	30) SIGPWR
31) SIGSYS	34) SIGRTMIN	35) SIGRTMIN+1	36) SIGRTMIN+2	37) SIGRTMIN+3
38) SIGRTMIN+4	39) SIGRTMIN+5	40) SIGRTMIN+6	41) SIGRTMIN+7	42) SIGRTMIN+8
43) SIGRTMIN+9	44) SIGRTMIN+10	45) SIGRTMIN+11	46) SIGRTMIN+12	47) SIGRTMIN+13
48) SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12
53) SIGRTMAX-11	54) SIGRTMAX-10	55) SIGRTMAX-9	56) SIGRTMAX-8	57) SIGRTMAX-7
58) SIGRTMAX-6	59) SIGRTMAX-5	60) SIGRTMAX-4	61) SIGRTMAX-3	62) SIGRTMAX-2

```

`kill` command  can manually stop a process.
 
`killall` will kill all the processes related to the the command. 

Created by Ed Breuer <br>
Date: 2024-07-18 <br>
Last Update: 2024-07-18
