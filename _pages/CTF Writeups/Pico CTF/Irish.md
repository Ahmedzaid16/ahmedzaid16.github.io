---
title: "Irish-Name-Repo"
date: "2024-10-15"
thumbnail: "/assets/img/thumbnail/Irish-Name-Repo.png"
---

# Irish-Name-Repo series [1,2,3] 

---

**Challenges Difficulty: Medium**
**Category: Web Exploitation**


## Introduction
This series focus on the fundamentals of SQL Injection vulnerability <a href = "https://owasp.org/www-community/attacks/SQL_Injection" target = "_blank" >A03:2021-Injectionicon</a>
## WALKTHROUGH
This challenges is from PicoCTF 2019 
## Irish-Name-Repo 1
#### Hints:
1. There doesn't seem to be many ways to interact with this. I wonder if the users are kept in a database?
2. Try to think about how the website verifies your login. 

---

![Challenge 1](/assets/img/posts/post-1/chal1.png )
After opening the link we go into a website that have images of Irish people 
![Challange page](/assets/img/posts/post-1/Irish-Name-Repo.png)
We notice that in the top left we have a hamburger menu clicking it gives us three pages including **Admin Login** interesting right!🤔
![Admin login-1](/assets/img/posts/post-1/adminlogin1.png)

Basically SQL injection infected form has no input validation so the website verifies your login by using the input directly in the query like this
```
SELECT * FROM users WHERE username = '$username' AND password = '$password';
```
as an example if we entered ahmed as a user and 123 as a password it will be like this
```
SELECT * FROM users WHERE username = 'ahmed' AND password = '123';
```
So we need to make this statement always true and a simple way is just entering **'or'1'='1** as a password and this will make the statment always true

```
SELECT * FROM users WHERE username = 'ahmed' AND password = ' ' or '1'='1';
```
![log in](/assets/img/posts/post-1/adminlogin1-1.png)
and yes we are in 🤗
![done](/assets/img/posts/post-1/finalso1.png)
## Irish-Name-Repo 2
#### Hints:
1. The password is being filtered.

---

![Challenge 2](/assets/img/posts/post-1/chal2.png )

the website is typically the same as the first one but if we tried the same way we get 
![sqli detected](/assets/img/posts/post-1/sqlidetected.png)
based on the hint we have, we know that **The password is being filtered**
so we try to enter the username as **admin'--** to comment the rest of the query 

```
SELECT * FROM users WHERE username = 'admin'-- AND password = '123';
```
![admin](/assets/img/posts/post-1/adminlogin2.png)
and that's it 
![done2](/assets/img/posts/post-1/finalsol2.png)

## Irish-Name-Repo 3
#### Hints:
1. Seems like the password is encrypted.

---

![Challenge 3](/assets/img/posts/post-1/chal3.png )

know we have something different as the login page just have password without username
![Challenge login](/assets/img/posts/post-1/adminlogin3.png)
Trying to enter any thing gives us login failed
![Challenge 3](/assets/img/posts/post-1/loginfailed.png)
lets try to use Burp to see what is happening in the request
![Challenge 3](/assets/img/posts/post-1/burb1.png)
we notice that the password is sent with a debug=0 Let’s change it to 1 
![Challenge 3](/assets/img/posts/post-1/burb2.png) 
based on the hint the password is encrypted and it seems to be ROT13 using [CyberChef](https://gchq.github.io/CyberChef/#recipe=ROT13(true,true,false,13)&oenc=65001) to decrypt it and yes it is 
![Challenge 3](/assets/img/posts/post-1/rot1-13.png)
so we just need to use our old method **'or'1'='1** but ROT13 it first 
![Challenge 3](/assets/img/posts/post-1/rot13.png)
by sending it using burp we get the flag 
![Challenge 3](/assets/img/posts/post-1/finalsol.png)

---

This was a simple and easy way to solve this series of challenges please leave a comment if you have any question 
by the way this is my first write-up ever if you have any suggestion please let me know🤗
shout out📢 to my friend [Omar Ashraf](https://www.linkedin.com/in/omar-ashraf-abdul-qader/) who encouraged me to start writing and supported me very much