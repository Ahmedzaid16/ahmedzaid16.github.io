---

title: "Enterprise"  
date: "2025-4-18"  
thumbnail: "/assets/img/thumbnail/Enterprise.png"

---

# Enterprise

---

**Challenges Difficulty: Hard**  
**Category: Active Directory**

---
## Initial Access

First things go first and the first here is to scan the ip
![image.png](/assets/img/posts/Enterprise/image.png)
from the scan result we are dealing with a domain controller and the domain name is **Enterprise.THM** 
we shall run a script scan hopefully we might found any known vulnerability but no there isn't 😑 
![image.png](/assets/img/posts/Enterprise/image%201.png)
So now we want to find any credentials to gain our initial access to this DC 
as this DC are running a web server on port 80 it's a good idea to discover it 
![image.png](/assets/img/posts/Enterprise/image%202.png)
noting important in this page but it might have another pages 
![image.png](/assets/img/posts/Enterprise/image%203.png)
another dead end no problem let's continue  
![image.png](/assets/img/posts/Enterprise/image%204.png)
another interesting service are running in this DC which is smb can we access any share in it🤔
yes there is more than one share we can access as guest one of them is **users** share with a remark **Users share. Do not Touch** 
![image.png](/assets/img/posts/Enterprise/image%205.png)
we are able to connect to it using smbclint but to go faster we will use smbmap To narrow our search
```
smbmap -H 10.10.226.245 -u 'a' -p '' -r 'Users'
```
![image.png](/assets/img/posts/Enterprise/Screenshot_2.png)
even smbap showed that we have read permission i got many **NT_STATUS_ACCESS_DENIED listing** so i moved to another path. 
if you searched with the domain name in github you will find an organization in the people section there is a user called Nik-enterprise-dev which has only one repo that contains a script called **mgmtScript.ps1** in the commit history of this ps script there is a user and password 
![image.png](/assets/img/posts/Enterprise/image%206.png)
we can connect with it to make sure that it's still working and it is
![image.png](/assets/img/posts/Enterprise/image%207.png)
in the home directory of this user there is 2 files but they was password protected
![image.png](/assets/img/posts/Enterprise/image%208.png)
we can try to use this user to get user spns using impacket and we got a hash from this
![image.png](/assets/img/posts/Enterprise/image%209.png)
using hashcat we were able to crack it and get **bitbucket** creds
![image.png](/assets/img/posts/Enterprise/image%2010.png)
using rdb we got into his desktop and found the first flag
![image.png](/assets/img/posts/Enterprise/image%2011.png)
## Privilege Escalation
so we don't have much to to with tis user 
![image.png](/assets/img/posts/Enterprise/image%2012.png)
one of the ways to escalate window privilege is [**Unquoted Service Path**](https://www.rapid7.com/db/modules/exploit/windows/local/unquoted_service_path/) which is one of the easiest ways in my mind

```bash
wmic service get name,pathname
```
bingo we found an interesting one here 
![image.png](/assets/img/posts/Enterprise/image%2013.png)
so let's create our revers shell using msfvenom
![image.png](/assets/img/posts/Enterprise/image%2014.png)
after moving our shell to the program file we need to start this service 
![image.png](/assets/img/posts/Enterprise/image%2015.png)
Grate news we escalated our privilege 🎉 
![image.png](/assets/img/posts/Enterprise/image%2016.png)
and here is your flag😉
![image.png](/assets/img/posts/Enterprise/image%2017.png)


