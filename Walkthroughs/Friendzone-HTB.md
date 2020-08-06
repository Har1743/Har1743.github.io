# Hack The Box - FriendZone walkthrough

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/Friendzone-photos/logo.png)

Today I am writing a walkthrough of the retired FriendZone machine of Hack The box. </br>
It is a Linux based machine. </br>
This machine has a static IP address <10.10.10.123> </br>
It was a nice bit tricky and interesting machine. </br>
</br>
You have to found **user.txt** and **root.txt** flag. 

## Penetration Methodology

* Scanning
  * open ports, versions and services
* Enumeration
  * Enumerating the smb services
  * DNS enumeration
  
## Scanning

Let's start our nmap to know about **open ports**, **versions** and **services** running on the machine.

```
nmap -sC -sV -sT -A 10.10.10.123
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-06 20:23 IST
Nmap scan report for 10.10.10.123
Host is up (0.47s latency).
Not shown: 993 closed ports
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 3.0.3
22/tcp  open  ssh         OpenSSH 7.6p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 a9:68:24:bc:97:1f:1e:54:a5:80:45:e7:4c:d9:aa:a0 (RSA)
|   256 e5:44:01:46:ee:7a:bb:7c:e9:1a:cb:14:99:9e:2b:8e (ECDSA)
|_  256 00:4e:1a:4f:33:e8:a0:de:86:a6:e4:2a:5f:84:61:2b (ED25519)
53/tcp  open  domain      ISC BIND 9.11.3-1ubuntu1.2 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.11.3-1ubuntu1.2-Ubuntu
80/tcp  open  http        Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Friend Zone Escape software
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
443/tcp open  ssl/http    Apache httpd 2.4.29
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: 404 Not Found
| ssl-cert: Subject: commonName=friendzone.red/organizationName=CODERED/stateOrProvinceName=CODERED/countryName=JO
| Not valid before: 2018-10-05T21:02:30
|_Not valid after:  2018-11-04T21:02:30
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
445/tcp open  netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
Service Info: Hosts: FRIENDZONE, 127.0.1.1; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: -6h27m25s, deviation: 1h43m53s, median: -5h27m27s
|_nbstat: NetBIOS name: FRIENDZONE, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: friendzone
|   NetBIOS computer name: FRIENDZONE\x00
|   Domain name: \x00
|   FQDN: friendzone
|_  System time: 2020-08-06T12:27:21+03:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2020-08-06T09:27:20
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 81.10 seconds

```

## Enumeration

As we can see that smb services are open so try to enumerate it. </br>
* Using smbmap

smbmap allows users to enumerate samba share drives across an entire domain. List share drives, drive permissions, share contents, upload/download
functionality,file name auto-download pattern matching, and even execute remote commands. This tool was designed with pen testing in mind, and is intended to
simplify searching for potentially sensitive data across large networks. </br>

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/Friendzone-photos/smbmap.png)

As by using smbmap we found two share drives
* general (READ only)
* Development (READ, WRITE)

Now try to connect with the smbclient service to the shared drives </br>
* Using smbclient
let us assume there might some malicious content.

smbclient is a client that can 'talk' to an SMB/CIFS server.

**Let's start with the Development folder**

`smbclient //10.10.10.123/Development`

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/Friendzone-photos/dev.png)

As there is nothing in this folder. </br>

**Let's try with the general folder**

`smbclient //10.10.10.123/general`

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/Friendzone-photos/gen.png)

We found a txt file named **creds.txt** </br>
Download the file `get creds.txt` </br>
Read that file `cat creds.txt` </br>

We found a credential but we dont know where to use this
* `admin:WORKWORKHhallelujah@#`

**We are done with smb enumeration** </br>

I have tried the credentials which we have found from the creds.txt but it didn't work. </br>

**I have also tried to enumerate the website but didn't found suspicious.**</br>

As nmap aslo shows us that dns service is open. </br>

**Let's try to enumerate dns service**

`dig axfr friendzone.red @10.10.10.123`

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/Friendzone-photos/dig.png)

We have found many subdomains
```
friendzone.red.
administrator1.friendzone.red.
hr.friendzone.red.
uploads.friendzone.red.
```

Let's add these to **/etc/hosts**

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/Friendzone-photos/etc.png)

Let's enumerate the `administrator1.friendzone.red.`

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/Friendzone-photos/http.png)

We got this page but nothing suspicious like administration  </br>

**Wait nmap shows https service is also open**</br>
**Let's try** `administrator1.friendzone.red.` **with https**

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/Friendzone-photos/https.png)

**Let's try those credentials which we have found in creds.txt**

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/Friendzone-photos/login.png)

**YUPS we have login successfully**

Here we got a message
`Login Done ! visit /dashboard.php`

Now let's move on to `https://administrator1.friendzone.red./dashboard.php`

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/Friendzone-photos/dash.png)

Now the page shows me some message

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/Friendzone-photos/text.png)

We got a **HAHA** image 

![](https://github.com/Har1743/Hardik-writeups/blob/master/Walkthroughs/photos/Friendzone-photos/haha.png)

But at bottom of the page we got something suspicious</br>
**Final Access timestamp is 1596732990**


