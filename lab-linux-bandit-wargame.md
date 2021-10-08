# Contents
 - an on line game to help you acquire general linux/programming culture
 - the game follows a [Capture The Flag](https://en.wikipedia.org/wiki/Wargame_(hacking) style
<<<<<<< HEAD
 - this is a challenge game, you will encounter many situations in which you have no idea on what you are supposed to do or where to start. don’t panic! don’t give up! the purpose of this game is for you to learn the basics. part of learning the basics, is reading a lot of new information to get new ideas.
 - here's an example from level 5 and how to get to level 6, from [https://overthewire.org/wargames/bandit/bandit6.html](https://overthewire.org/wargames/bandit/bandit6.html):

> Bandit Level 5 → Level 6
> Level Goal
>
> The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:
>
>    human-readable
>    1033 bytes in size
>    not executable
>
> Commands you may need to solve this level
> 
> ls, cd, cat, file, du, find

=======
 - this is a challenge! you will encounter many situations in which you have no idea on what you are supposed to do or where to start. don’t panic! don’t give up! the purpose of this game is for you to learn the basics. part of learning the basics, is reading a lot of new information to get new ideas.
>>>>>>> b38b9c8d3cc0e86c7101e8a6c1c00cdda0515731
 
# Requirements
 - you do not need linux virtual machine to start playing.
 - all you need is an ssh client program, available by default in linux. if you are on windows, you may need to use putty (check it first). download putty [here](https://www.microsoft.com/en-us/p/putty-unofficial/9n8pdn6ks0f8#activetab=pivot:overviewtab)

# Tasks
 - start the game at level 0, you have to open a session on the game host as user bandit0 (password bandit0). more instructions [here](https://overthewire.org/wargames/bandit/bandit0.html). use
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```
<<<<<<< HEAD
 - each time you find the password for the next level, copy it and close the session with `exit`. then open session for the next level
    - copy using (Ctrl+Shit+C), paste using (Ctrl+Shit+V)
=======
- try to find the password for the next level. it is hidden in a file located somewhere on the system, embedded in an image, it may be encoded, encrypted, etc. FIND IT!
- each time you find the password for the next level, copy it and close the session with `exit`. then open session for the next level
  - copy using (Ctrl+Shit+C), paste using (Ctrl+Shit+V)
>>>>>>> b38b9c8d3cc0e86c7101e8a6c1c00cdda0515731
