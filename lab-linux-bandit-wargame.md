# Contents
 - an on line game to help you acquire general linux/programming culture
 - the game follows a [Capture The Flag](https://en.wikipedia.org/wiki/Wargame_(hacking) style
 - this is a challenge! you will encounter many situations in which you have no idea on what you are supposed to do or where to start. don’t panic! don’t give up! the purpose of this game is for you to learn the basics. part of learning the basics, is reading a lot of new information to get new ideas.
 
# Requirements
 - you do not need linux virtual machine to start playing.
 - ssh client program: available by default in linux. if not available on windows, you'll need to use putty, download it [here](https://www.microsoft.com/en-us/p/putty-unofficial/9n8pdn6ks0f8#activetab=pivot:overviewtab))

# Tasks
 - start the game at level 0, you have to open a session on the game host as user bandit0 (password bandit0). more instructions [here](https://overthewire.org/wargames/bandit/bandit0.html). use
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```
- try to find the password for the next level. it is hidden in a file located somewhere on the system, embedded in an image, it may be encoded, encrypted, etc. FIND IT!
- each time you find the password for the next level, copy it and close the session with `exit`. then open session for the next level
  - copy using (Ctrl+Shit+C), paste using (Ctrl+Shit+V)
