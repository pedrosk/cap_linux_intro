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

- 2013 Firefox Mobile OS 
- 2012 Tizen OS -> used in TVs, Fridges, etc.

=====================================================
### A little bit about Microsoft
- founded in 1972 as a company
- in 1980, signs a contract with IBM to write an OS
- 1981 - **PURCHASE** of RIGHTS to what becomes MS-DOS
- 1983 - First Windows
- 1985 - Windows 1.0 (on top of DOS)
- 1995 - Windows 95 (Network)
.....
## Privileges, owners, users, and group:
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

-`ls` - (list) print the content of the (present) directory
  - useful flags: (they start with -)
  - -a all (shows hidden files that start on . (dot))
  - -l (long)
  - -h human readable
  - -t sort in time 
  - -r reverse (youngest list last)

- `pwd` - shows where in the files system you (present working directory)

cd /home
ls -la
 what do you see?
drwxr-x--- 41 ed    ed    4096 Aug 18 12:23 ed
What is this and why is it important?

after the first letter are three groups consisting of three lettered possitions:
Fist three: 
     owner – Owner permissions apply ONLY  to the owner of the file or directory. 
Second three: 
    group – The Group permissions apply ONLY to the  group (groupd have members) that has been assigned to the file or directory.
Last three:
    all users – The All Users permissions apply to all other users on the system, this is the permission group that you want to watch.

Permissions explained: 
Why letters:  Its easier to read than remember the combination of numbers.
read – The Read permission refers to a user’s capability to read the contents of the file. (and nothing else !)
write – The Write permissions refer to a user’s capability to write or modify a file or directory. (OR DIRECTORY)
execute – The Execute permission affects a user’s capability to execute a file or view the contents of a directory. ( OR DIRECTORY)

r = 4
w = 2
x = 1
so what is 755? or 600?
7= 4+2+1 Owner Can do EVERYTHING
5 = group can 4+1 => Read and Execute. Can NOT modify the files!
5= everyone else same as above 

700 => ONLY OWNER CAN do EVERYTHING. Everyone ELSE can do NOTHING. NOT EVEN THE MEMBERS OF THE GROUP can do anything
 
600 =>  you tell me!

What are logs usually set to? 
HINT run: `ls -la /var/log |grep -i log`


==========================================
How do you create a user?
there is useradd and adduser. 
whats the difference? +> userfriendlines. adduser is a script using useradd. 
useradd uses flags where you can or have to add home directory, etc.
adduser will prompt for questiions and will create the most commonly used feature like a home directory

When to us each....

How to add user to a specific group?
sudo usermod -a -G examplegroup exampleusername

To create a new group:
sudo sudo groupadd new_group

=========================================
back to files, directories and modifying who can do what
assign or modify group and user ownship 
use chown command
two usages when setting / changing ownership for both user:groups

chown user:group <Whats_is_being_modified> 
NOTE: You need proper privileges

just user and file:  
chown user some.file

just group:
chown :group1 some.file

how about to change ownership of subdirectiries and files?
chown -R someuser:somegroup *.some_extention

=======================================
Similarly, you can change who can do what with files both on user, owner, and group levels. Use command:
 chmod permission files
example chmod 755 my_executable.script

or you can add, sustract priviledes using letters:

who =>
u: User, e.g. owner of the file.
g: Group
o: Others
a: All, meaning all of the above.

Action on permissions:
-: Minus sign. Removes the permission.
+: Plus sign. Grants the  permission.
=: Equals sign. SET  permission! This will REMOVE other permissions.

The "which " permissions:
r: The read permission.
w: The write permission.
x: The execute permission.

example: 
chmod ug+rw,o-x file_name.file_type

- - `passwd` -change your password
  <ul style="list-style-type:none;">
   <li>-e set expiration</li>
  <li>-i make inactive</li>
  <li>-s status</li>
  </ul>


=============================================
what can we do with file
cp (copy)
mv (move)
rm (remove - delete)
touch - create an empty file
and more
cat - print whats inside a file

================================================
other useful tools:
grep - will search for a pattern in files
r recursively, meaning it will go  down into subdirectories
         -i will ignore the case:
            Example: grep -r -i error * 
                  will find: error, Error, ERROR, ErRoR, etc

find -> `find [path] [options] [expression]` 
-name pattern
 -f file
-d directories
Example: find ~ -name "example.txt"
find ./ -type f -name "*.txt"

Find a subdirectory called cap
NOTE: The directory to start from is root / 
           type is set to the directory 
           and search is for something called "cap"
find / -type d -name cap
