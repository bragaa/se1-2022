lab-linux-class3.md

# Contents: 
 - users/groups management 107.1
 - permissions and ownership 104.5
 - basic files/directories management 103.3
 
# Requirements
 - we assume that you have installed ubuntu server system, you called it `mysrvr`, you have created a user named `gcr`, you assigned mode `NAT` to the network interface and you have installed the secure shell (ssh) server on it (otherwise, you have to install it using ```sudo apt-get update && apt-get install openssh-server```). these steps were carried out in the previous class.

 - ensure you have a snapshot before working on this lab

---

 - start `mysrvr` from your vmm (virtual machines manager, that is, vmware or virtual box) and open a session as `gcr` from the console. find the IP of the system using
```bash
ip addr sh | grep '^ *inet '
```

the command will show the following
```
    inet 127.0.0.1/8 scope host lo
    inet 192.168.32.133/24 brd 192.168.32.255 scope global dynamic ens33
```
 in my case, the server's address is `192.168.32.133` (the one not starting with 127). when the machine restarts, it may change, so you need to find it again.

 - from the host system, connect to mysrvr using `ssh gcr@192.168.32.133` (either from the command line if ssh is enabled or using `putty`)

---

- find your **working directory** using command `pwd` (**p**rocess' **w**orking **d**irectory). it should be `/home/gcr/`. a working directory is the locattion where the system will put or look for your files *by default*. check this by creating a file named `f`, then list content of your working directory 
```bash
touch f
ls
```

- open a command line session to your system as user gcr (already opened?)

- you are asked to create user accounts named 'bakhalil' and 'balukman' using the command ```useradd``` as in this example
```bash
sudo useradd balukman -c "Lukman Ben Abdallah"
sudo useradd bakhalil -c "Khalil Ben Abdelfattah"
```

- for each user, assign the password `Pa$$w0rd`
```bash
sudo passwd bakhalil
```

- force the users to change their passwords the first time they connect to the system
```bash
sudo passwd -e bakhalil
```

- now, from a different terminal, try connecting as `bakhalil`. you'll be asked the password to login, then to redefine the password.

- check you working directory using `pwd`, you should find `\`
```bash
pwd
```

 - we'll find more on the working and home directories later. but for now, let's try to create a file called `myfile` containing the words "hello every one" using
```bash
echo "hello every one" > myfile
```
 the system rejects your command and tells you that you are not allowed to create the file in folder `/`. the reason is that we have not created a home directory for `bakhalil` where it can store his own files. we'll solve this.

 - close the session for `bakhalil` by typing `exit`

 - from session of `gcr`, type the following commands
```bash
sudo mkdir /home/bakhalil
sudo chown bakhalil:bakhalil /home/bakhalil
sudo usermod --home /home/bakhalil bakhalil
sudo usermod --shell /bin/bash bakhalil
```
 the first command creates a directory, then defines its owners (more on this later). the usermod (**user** **mod**ify) will set `/home/bakhalil/` as the home directory for `bakhalil` so that every time he opens a session, it is directly put in. finally
 we define a different shell for him. more on this later.

 - check if this works: open a session for `bakhalil`, then check the result of `pwd`. try again creating the file containing "hello everyone". it should work now. you also noticed that the **prompt** for `bakhalil` looks the same as `gcr`:  `bakhalil@mysrvr:~$`

 - now, list the content of the directory `/home/bakhalil/` using ```ls -la /home/bakhalil```. how many files are there? try also the following commands ```ls -a``` ,```Ls``` 

 - in linux, the root can group users into groups for **access control** purposes. users or groups have both names (defined by the root user) and numeric identifiers (assigned by the system).
 when you create a user, the system creates a group with the same name and having this user as a single member. the group is called the **primary group** of the user.

 find more information on the created user using `id bakhalil`: uid/gid, and the groups to which he belongs.

---

 - create the following directory: `/home/groups/` (that is, a directory called `groups` in the directory `home` which in turn is located in the root directory)
```bash
mkdir /home/groups
```
 an error message shows up, explaining that you do not have enough permissions to create the directory. in fact each user can only modify the content of his own home directory and in this case, as `gcr`, you are only allowed to work in `/home/gcr/`, and cannot play outside. so, only root can make this. the correct command line is 
```bash
sudo mkdir /home/groups
```

- each file belongs to one user (called the owner) and one group (called the owning group). for example, a file created by user `gcr`, belongs by default to the user `gcr` and to its primary group. check this by using command stat
```bash
stat /home/gcr
```

try this example as well:

```bash
stat /home/gcr/ --printf "User id is: %u    User name is: %U \nGroup id is: %g    Group Name is:: %G\n"
```

---

 - create a group called `students` using `groupadd`

 - make both `bakhalil` and `balukman` members of group `students` using 
```bash
sudo usermod -aG students balukman
```

 - check the groups to which they belong using `id` or `groups`

 - from the manual of `usermod`, find the meaning of options `a` and `G`: type `man usermod`, then type `/-a ` then Enter. (occurrences will be highlighted). you can quit the manual by typing `q`.

---

 - create a directory `/home/groups/students/` to be used by the group members

 - change its owning group to the group `students`
 ```bash
 chown :students -R /home/groups/students
 ```

 - change the primary group of `bakhalil` and `balukman` to students
 ```bash
 usermod -g students bakhalil
 ```

 - check the permissions related to `/home/groups/students`
 ```bash
 stat /home/group/students --printf="%A\n" 
 ```

 - now change permissions of `/home/groups/students` so that members of its owning group can modify its content (write). use
 ```bash
 chmod g+w -R /home/group/students/
 ```

 - make a test: as `bakhalil`, would it possible to create a file in `/home/group/students/` instead of its own home diretory `/home/bakhalil/`?
 
---

 - connect as user `bakhalil`

 - create the following hierarchy in your home directory. use `mkdir`. for example, to create directory `lpi101` in `workspace`, use `mkdir workspace/lpi101/`. The `~` denotes your home directory (i.e. `/home/bakhalil/`)
```
~
├── documents/
│    └── notes-cours/
│        ├── rx1/
│        └── se1/
└── workspace/
    ├── lpi101/
    └── projet-java/
```

 - find out the following informations regarding `~/workspace/lpi101/`: file type, permissions, owner, size, last modification time. use
```bash
stat ~/workspace/lpi101/
```
 
 - copy `~/workspace/` in / using
```bash
cp -R ~/workspace /
```
 why it was not possible to make the operation?

 - create a file containing `hello world` using command `nano`, then a different file using `vi`
