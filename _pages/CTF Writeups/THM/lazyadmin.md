---

title: "LazyAdmin"  
date: "2025-4-16"  
thumbnail: "/assets/img/thumbnail/lazy.png"

---

# LazyAdmin
---
**Challenges Difficulty: Easy**  
**Category: Privilege Escalation** 

---
## Initial Access 
As usual we will start with with nmap to scan the machine 
![image.png](/assets/img/posts/lazy/image%205.png)
There is 2 open ports 22, 80 so let's open this webpage
![image.png](/assets/img/posts/lazy/image%207.png)
ok it's an apache server but we want to fuzz this ip to find more interesting pages
![image.png](/assets/img/posts/lazy/image%206.png)
great there is a directory called **content**
![image.png](/assets/img/posts/lazy/image%208.png)
going deeper in this directory we found several interesting pages 
![image.png](/assets/img/posts/lazy/image%209.png)
the **/as** one is a login page but we don't have credentials yet ;)
![alt text](/assets/img/posts/lazy/image.png)
now this is an interesting one as we can find a working creds from this backup
![alt text](/assets/img/posts/lazy/Screenshot_1.png)
![image.png](/assets/img/posts/lazy/image%2010.png)
and we got a user called **manager** with admin role and a hash for his password
![image.png](/assets/img/posts/lazy/image%2011.png)
and we got the password
![image.png](/assets/img/posts/lazy/image%2012.png)
browsing this site we can find a **Media Center** tab with a file upload 
![alt text](/assets/img/posts/lazy/Screenshot_2.png)
just upload a reverse shell and you will get a shell but remember to change the .php to .php5 or any other extension 
![image.png](/assets/img/posts/lazy/image%2013.png)
## Privilege Escalation  
Great we got initial access with a **www-data** now we need to escalate to root 
checking our permissions we saw that there is a file that we can run as sudo 
![image.png](/assets/img/posts/lazy/image%2014.png)
and we also has write permission to this file so overwrite it with another shell and run as sudo and you will get a root shell  
![image.png](/assets/img/posts/lazy/image%2015.png)

