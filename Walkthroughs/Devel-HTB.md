# Hack The Box - Devel walkthrough

![](/photos/devel-photos/devel.png)

Today I am writing a walkthrough of the retired Devel machine of Hack The box.  
It is a Windows based machine.  
This machine has a static IP address <10.10.10.5>  
It was a nice easy and interesting machine.  
  
You have to found **user.txt** and **root.txt** flag. 

## Penetration Methodology

* Scanning
  * open ports, versions and services
* Enumeration
  * ftp enumeration
  * http enumeration

## Scanning

Let's start our nmap to know about **open ports**, **versions** and **services** running on the machine.

![](/photos/devel-photos/devel-nmap.png)

Now we are done with our nmap.  

## Enumeration

As nmap shows us that ftp service is open and  
we can login to ftp with anonymous user.

![](/photos/devel-photos/ftp-acess.png)

As we can see that there is image name **welcome.png**  
Let's try we can access that image from the website or not.

