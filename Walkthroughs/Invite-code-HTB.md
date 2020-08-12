# Hack The Box

![](/photos/Invite-code-HTB/htb_page.png)

## What is HTB

Hack The Box is an online platform allowing you to test your penetration testing skills and exchange ideas and methodologies with thousands of people in the security field.       

## How to get started with HTB

To join with HTB, firstly HTB test your capablity that you are having sufficient skills right now or not.    
So to join with HTB we have given a task to enter the invite code.   

## How to get invite code

This is a very tricky question that we don't have any invite code so what can we do now.    
  
The answer is that we have to fuzz the website and enumerate for invite code.

### Let's go

Firstly got to the official HTB website.  
[https://www.hackthebox.eu](https://www.hackthebox.eu)
  
Then there is a page for **Join now**  
[https://www.hackthebox.eu/invite](https://www.hackthebox.eu/invite)

![](/photos/Invite-code-HTB/invite_page.png)

**Now what** we are stuck that we dont have the invite code.  
Let's start enumerating the website. 
  
#### Let's check the inspect element
  
There we found a suspicious path **/js/inviteapi.min.js**

![](/photos/Invite-code-HTB/inspect.png)

Let's check this path  
Then we redirected to a page  

![](/photos/Invite-code-HTB/make.png)

Let's check this content  
  
Wait we got something there is line  
```
'function|console|log|makeInviteCode|ajax|type|POST|dataType
|json|url||api|invite|how|to|generate|success|error'
```
  
In this **makeInviteCode** looks suspicious let's again go to the invite page and check the contents of **makeInviteCode**  
[https://www.hackthebox.eu/invite](https://www.hackthebox.eu/invite)  
  
Again open the **inspect element** and in that open **console tab** and type **makeInviteCode() and press enter**.  

![](/photos/Invite-code-HTB/makeinvitecode.png)

**Hey we got something valuable**  
There is a data 
```
Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb /ncv/vaivgr/trarengr
```   
It is **ROT13** encrypted..
  
Let's decrypt this data for that i prefer [https://cryptii.com/pipes/rot13-decoder](https://cryptii.com/pipes/rot13-decoder)  
  
After decrypting we got a message `In order to generate the invite code, make a POST request to /api/invite/generate`  

![](/photos/Invite-code-HTB/rot.png)

Let's make a post request to **https://www.hackthebox.eu/api/invite/generate**  
Using  
  
```
curl -XPOST https://www.hackthebox.eu/api/invite/generate
```  

![](/photos/Invite-code-HTB/curl.png)

We got a base64 message 
```
VlJOTFUtRlRMTVMtTFNGRUotUFVPT0QtREJVTlE=
```  
let's decode that  



