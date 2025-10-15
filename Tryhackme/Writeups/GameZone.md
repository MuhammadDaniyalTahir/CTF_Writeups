# GameZone

#### Task 1

Completed all required steps like to start the machine and connect with tryhackme vpn. 

##### Question: What is the name of the large cartoon avatar holding a sniper on the forum?
##### Solution:

I entered the IP of target machine in browser and gun man shown in the page. I took it's screenshot and done google lens on it. I came to found it's name.
##### Answer: agent47

#### Task 2

##### Question: When you've logged in, what page do you get redirected to? 
##### Solution: 
I tried to log in with the given payload of sqli. And I left the 

```bash
' or 1=1-- -
```

We successfully logged in. 

![Alt text](../Screenshots/GameZone/Portal.png)

Now look into the browser search bar at the top.
##### Answer: portal.php

#### Task 3

Now at portal.php, there is a search option. I captured the request in burpsuite and saved it into request.txt so that I can use it into sqlmap.

```bash 
sqlmap -r request.txt --dump
```

And it dump the whole database because searchitem parameter is vulnerable. It gave me the database tables like post and users.

##### Question: In the users table, what is the hashed password?
##### Solution: 
![Alt text](../Screenshots/GameZone/hash.png)

##### Question: What was the username associated with the hashed password?
##### Solution: `agent47`(Can see in the above image).

##### Question: What was the other table name?
##### Solution: 
![Alt text](../Screenshots/GameZone/table_name.png)
##### Answer: post

