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

Next, let's use the original `useradd` command to create user `mouse`<br>
Here lets make the home `/home/forest` and ID `3000`<br>
Run: `useradd -m -d \home\forest -u 3000 mouse`<br>
NOTE: you need the flag `-m` or the directory will be set but not created

####Now set the password:
Run: `sudo passwd  mouse` set `ILoveMouse123`

If you get: `passwd: password updated successfully` Then the password WAS created; if you get: 
```
passwd: Authentication token manipulation error
passwd: password unchanged
```
You had a typo when you re-typed the password, and the password was NOT created.

Next, log is as `micky` and create an empty file `my.file`<br>
`su - micky`<br>
`touch my.file`<br>
List the directory with file privileges:<br>
`ls -la`<br>
You should see the file that you just created. Take note of the privileges for user, group and others:<br>
Example: `-rw-rw-r-- 1 micky micky    0 Aug 25 17:46 my.file`

Let go back to your sudo enabled user and run: `sudo chmod 770 /home/micky` to set more restrictive access privileges to Micky's home directory.

Now, change use to `mouse` (HINT: use `su - mouse` as above)<br>
After you log in run `bash` so your shell becomes `bash`

List/print the content of user `micky` home directory:<br>
HINT: run `pwd`<br>
you get ...?<br>
then run `ls -la /home`<br>

OK, run: `ls -la ../micky/`<br>
so you got this: `ls: cannot open directory '../micky/': Permission denied`<br>
Aha, you can't see what's in the Micky home because you are NOT the owner or a member of the Micky group, and the group "others" has NO PRIVILEGES (---).

####Let create a new group `mice`:
Let run: `sudo groupadd mice`
You got a response that the user is not allowed to run as sudo. Okay, let's do it as a user who can do it. To log out from the account `mouse,` hit `CTRL+D` on your keyboard or type exit.

Since you run two shells, you may need to log out/exit the shell twice to access your' Micky` account and one more time to access your main account, which has sudo privileges.

Go ahead and create the `mice` group. (use the command above)
Now let's check on the new group `mice`
There is two ways to achieve it:
1) `getent group mice`
2) it to `cat` the `group` file in the `/etc` directory and use `grep` to filer for mice
   Run: `cat /etc/group |grep -i mice`
   HINT: run the above command to see all the groups and their IDs

COOL HINT: So that you do not have to always come back to the sudo enabled user, you could add the ser `mouse` to the sudo group. The instruction here wants to to be safe and not give `mouse` or `micky` higher privileges  they should have. But if would need to give a user `sudo` group membership its done by running: 
`sudo adduser <user> sudo`<br>
Example: `sudo adduser mouse sudo`

Now add `micky` and `mouse` to group `mice`:<br>
What we need to is modify user and add group:<br>
`sudo usermod -a -G <group> <user>`<br>
`sudo usermod -a -G mice micky`<br>


Now check if `micky` is a member of `mice` group:<br>
`getent group mice`<br>

Next, repeat the same for user `mouse`.

Let's try again `su - mouse` then run `bash`  and run `ls -la /home/micky/`<br>
Loks like still no luck: `ls: cannot open directory '/home/micky/': Permission denied`<br>
Hint answer is: `ls -la /home`<br>
Solution: add `mouse` to group `micky`.<br>
After you are done check if `mouse` is part of group `micky`<br>
HINT: 
```
id mouse
Result: uid=3000(mouse) gid=3000(mouse) groups=3000(mouse),1003(micky),3001(mice)
```

Now as user  `mouse` run: `ls -la /home/micky/`

### File privilege exercise:
First, let's change the file my.file as follows: The owner (Micky) can READ, WRITE, and execute, the group can only read and execute, and others can do nothing. <br>
As your main user run: `sudo chmod 750 /home/micky/my.file` <br>
Now log in as mouse and try to write to the file:<br>
`echo "my message" >>  /home/micky/my.file`<br>

You will get the permission denied. 

### Exercise ownerhip/group:
As the primary user change the owner and the permission of the file to group mice and file permission owner everything, group everything, and other nothing<br>
1) `sudo chown micky:mice /home/micky/my.file`<br>
2) `sudo chmod 770 /home/micky/my.file`<br>

Log back in as `mouse` into the `bash` session and try to write to the file `my.file` in Micky's home. <br>
No error?<br>
Remember, this used to be an empty file. Is it still empty? Let's check:<br>
Run `cat /home/micky/my.file`<br>

Why did this work? <br>
Because `mouse` while living in `forest` is a member of the `mice` group that can read, write, and execute on this file. (not any other files).









