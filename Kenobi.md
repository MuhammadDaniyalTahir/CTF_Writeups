By reading the description, we can understand that there is a linux machine and have some network share services on. We need to do some reconnaissance and then go for exploitation. Let's dive.

Question: Scan the machine with nmap, how many ports are open?
Solution: We can use nmap for port scanning.

`sudo nmap -sS --top-ports 100 10.10.76.165`

```
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
2049/tcp open  nfs
```
Answer: 7.

