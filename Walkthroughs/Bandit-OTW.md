# Bandit Walkthrough (Level 0 - 1)  
  
Today we will play a war-game names **BANDIT**.  
This war-game is hosted by Overthewire organisation.  

![](/photos/bandit-photos/cat.png)

It is absolutely for beginners.  

**It is basically teaches the basic knowledge of linux and ssh in a fun challenging way**

## Objective 
  
* Catch the hints given to you. 
* Login to ssh with credentials.
* Enumerate for credentials for the next Level.

# Level 0
** **
**Clue**
```
The goal of this level is for you to log into the game using SSH.  
The host to which you need to connect is bandit.labs.overthewire.org,  
on port 2220.  
The username is bandit0 and the password is bandit0.   
Once logged in, go to the Level 1 page to find out how to beat Level 1.
```
  
The clue tells us that just we have to just login to given host with ssh on port 2220
```
ssh -p 2220 bandit0@bandit.labs.overthewire.org
password : bandit0
```

![](/photos/bandit-photos/bandit0.png)

# Level 0-1
** **
**Clue**
```
The password for the next level is stored in a file called readme  
located in the home directory.  
Use this password to log into bandit1 using SSH.
Whenever you find a password for a level,
use SSH (on port 2220) to log into that level and continue the game.
```

The clue tells us that
* Find the credentials for the next level stored in readme file
* Username for next level is bandit1
* Whenever you found password do login with port 2220.

![](/photos/bandit-photos/bandit0-1.png)

We got the password for next level
```
username : bandit1
password : boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

# Level 1-2
** **
**Clue**
```
The password for the next level is stored in a file  
called - located in the home directory
```

The clue tells us that
* password for the next level is stored in a file called -

Let's login to **bandit1**
```
ssh -p 2220 bandit1@bandit.labs.overthewire.org
password : boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

Now we are login to bandit1  
We have found the - file but we to read this file  
cat - doesn't works as cat takes -(hypen) as stdin/stout.  
So we have to prefix it by using **./-**

![](/photos/bandit-photos/bandit1-2.png)

We got the password for next level
```
username : bandit2
password : CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```
