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

**Clue**
```
The goal of this level is for you to log into the game using SSH.  
The host to which you need to connect is bandit.labs.overthewire.org, on port 2220.  
The username is bandit0 and the password is bandit0.   
Once logged in, go to the Level 1 page to find out how to beat Level 1.
```
  
The clue tells us that just we have to just login to given host with ssh on port 2220
```
ssh -p 2220 bandit@bandit.labs.overthewire.org
password : bandit0
```

![](/photos/bandit-photos/bandit0.png)




