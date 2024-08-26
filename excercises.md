Hello world
How to go to home directory: re member home is the special character called "tilda" `~` <br>
`cd ~`
<br>
What is in my home directory?<br>
`ls -la`
<br>
Where am I on the tree of the filesystem?  <br>
`pwd`<br>

### Exercise:
Let's create a user "micky" and a user "mouse". <br>
To accomplish this, you need to have elevated privileges, and thus must be run as a root user or with a `sudo`<br>
Run: <br>
`sudo adduser micky` set password `ILoveMicky13!` . Do you think this is a strong password? How would you improve it? <br>
Let's check what is the UID (unique ID), group ID (GUID), and to what groupd of the user `micky` belongs:<br>
Run: `id micky`<br>
You may get a response similar to this: ` uid=1003(micky) gid=1003(micky) groups=1003(micky)`<br>

udo passwd  mouse
Next, let's use the original `useradd` command tpo create user `mouse`
Here lets make the home `\home\forest` and ID `3000`
Run: `useradd -m -d \home\forest -u 3000 mouse`
NOTE: you need the flag `-m` or the directory will be set but not created

Now set the password:
Run: `sudo passwd  mouse` set `ILoveMouse123`

If you get: `passwd: password updated successfully` Then the password WAS created; if you get: 
```
passwd: Authentication token manipulation error
passwd: password unchanged
```
You had a typo, and the password was NOT created.

log is as `micky` and create an empty file `my.file`
`su -i micky`
`touch my.file`
List the directory with file privileges:
`ls -la`
You should see amongst file your file that you just created. Take a note of the privileges for user, group and others:
`-rw-rw-r-- 1 micky micky    0 Aug 25 17:46 my.file`

Now change use to `mouse` (HINT: use su - as above)
after you log in run `bash` so your shell becomes `bash`

List/print the content of user `micky` home directory:
HINT: run `pwd`
you get ...?
than run `ls -la /home`

OK, run: `ls -la ../micky/
so you got this: `ls: cannot open directory `'../micky/': Permission denied`
Aha, you can't see what's in the Micky home because you are NOT the owner or a member of the Micky group, and the group "others" has NO PRIVILEGES (---).

Let create a new group mice:
Let run: `sudo groupadd mice`
You got a response that the user is not allowed to run as sudo. Okay, let's do it as a user who can do it. To log out from the account `mouse,` hit `CTRL+D` on your keyboard or type exit.

Since you run two shells, you may need to log out/exit the shell twice to access your' Micky` account and one more time to access your main account, which has sudo privileges.

Go ahead and create the `mice` group. (use the command above)
Now let's check on the new group `mice`
There is two ways to achieve it:
1) `getent group mice`
2) it to `cat` the `group` file in the `/etc` directory and use `grep` to filer for mice
   Run: `cat /etc/roup |grep -i mice`
   HINT: run the above command to see all the groups and their IDs

Now add micky and mouse to group mice:
What we need to is modify user and add group:
`sudo usermod -a -G <group> <user>`
`sudo usermod -a -G mice micky`


Now check if `micky` is a member of `mice` group:
`getent group mice`

Repeat for user `mouse`.

Let's try again `su - mouse` than run `bash`  and run `ls -la /home/micky/`
Loks like still no luck: `ls: cannot open directory '/home/micky/': Permission denied`
Hint answer is: `ls -la /home`
Solution: add mouse to group micky.
After you are done check if `mouse` is part of group `micky`
HINT: 
```
id mouse
Result: uid=3000(mouse) gid=3000(mouse) groups=3000(mouse),1003(micky),3001(mice)
```
Now as user  `mouse` run: `ls -la /home/micky/`

### file privilege exercise:
First, let's change the file my.file as follows: The owner (Micky) can READ, WRITE, and execute, the group can only read and execute, and others can do nothing. 
As your main user run: `sudo chmod 750 /home/micky/my.file` 
Now log in as mouse and try to write to the file:
`echo "my message" >>  /home/micky/my.file`

You will get the permission denied. 

Extra exercise:
As the primary user change the owner and the permission of the file to group mice and file permission owner everything, group everything, and other nothing
1) `sudo chown micky:mice /home/micky/my.file`
2) `sudo chmod 770 /home/micky/my.file`

Log back in as `mouse` into the `bash` session and try to write to the file `my.file` in Micky's home. 
No error?
Remember, this used to be an empty file. Is it still empty? Let's check:
Run `cat /home/micky/my.file`

Why did this work? 
Because `mouse` while living in `forest` is a member of the `mice` group that can read, write, and execute on this file. (not any other files).









