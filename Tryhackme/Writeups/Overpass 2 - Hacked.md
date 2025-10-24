# Overpass 2 - Hacked

#### Task - 1

##### Question: What was the URL of the page they used to upload a reverse shell?
##### Solution: 
We already know that for uploding POST request method is used. I simply applied the filter in wireshark:

```
http.request.method == "POST"
```

And got exactly that packet.
##### Answer: /development/

##### Question: What payload did the attacker use to gain access?
##### Solution: 
As I mentioned that I found single packet on filter. I open the http stream of that packet and got the payload, you can see in the following:

![Alt text](../Screenshots/Overpass%202%20-%20Hacked/payload.png)

Question: What password did the attacker use to privesc?
Solution: Now we know that after getting the initial access, attacker tried to enter the password. It means after the payload ran, the password was entered. Now I tried to find the http stream where connection was build for initial access. Remember the port in payload: 4242. 

Now I applied following filter and open http stream: 

```
tcp.port == 4242
```

And found that:
![Alt text](../Screenshots/Overpass%202%20-%20Hacked/stream.png)

In that stream I found the password:
![Alt text](../Screenshots/Overpass%202%20-%20Hacked/password.png)
##### Answer: whenevernoteartinstant

##### Question: How did the attacker establish persistence?
##### Solution: 
In the same stream, I scrolled down and found a github repository that contains backdoor in it's name. 
##### Answer: https://github.com/NinjaJc01/ssh-backdoor

##### Question: Using the fasttrack wordlist, how many of the system passwords were crackable?
##### Solution: 
For that I searched about this fasttrack wordlist and found the following link:
[fastrack wordlist](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwjEzKG_8ryQAxWCVqQEHb6YKUwQFnoECAwQAQ&url=https%3A%2F%2Fweakpass.com%2Fwordlists%2Ffasttrack.txt&usg=AOvVaw1mVhNfkEX3zS-cQR3s1ILv&opi=89978449). Then I ran the following command to crack the hashes. But remember to save all the 5 hashes that we see in the packet stream, save into a file:
![Alt text](../Screenshots/Overpass%202%20-%20Hacked/hashes.png)

But remember to save only hashes in the file, starting from $6 and upto coming colon(:), colon must not included.

```bash
hashcat -m 1800 -a 0 hashes.txt fasttrack.txt -w 3 --status --status-timer=10
```

It cracked two passwords but answer was not 2. 
![Alt text](../Screenshots/Overpass%202%20-%20Hacked/cracked_password1.png)

Then I tried to crack them using rockyou.txt and again 2 hashes are cracked. 
![Alt text](../Screenshots/Overpass%202%20-%20Hacked/cracked_password2.png)
And the correct answer was :
##### Answer: 4.

#### Task - 2

##### Question: What's the default hash for the backdoor?
##### Solution:
For that purpose we need to visit the backdoor code. I went to the repository > main.go and I found a string:
![Alt text](../Screenshots/Overpass%202%20-%20Hacked/default_hash.png)

##### Question: What's the hardcoded salt for the backdoor?
##### Solution: 
I wondered in the code and saw that there is a function getting **salt** as parameter:
![Alt text](../Screenshots/Overpass%202%20-%20Hacked/salt_func.png)
Then I searched where this function is being called and found the value of salt as it is the second value passed to the function:
![Alt text](../Screenshots/Overpass%202%20-%20Hacked/salt.png)

##### Question: What was the hash that the attacker used? - go back to the PCAP for this!
##### Solution: 
Again I went to that http stream that gave us a lot answers because anything that attacker have done can be found in that stream:
![Alt text](../Screenshots/Overpass%202%20-%20Hacked/attacker's_hash.png)
Above picture shows the hash used by the attacker.

##### Question: Crack the hash using rockyou and a cracking tool of your choice. What's the password?
##### Solution: 
I used the following command:
```bash
hashcat -m 1710 -a 0 backdoor.hash /usr/share/wordlists/rockyou.txt 
```
And found the value:
![Alt text](../Screenshots/Overpass%202%20-%20Hacked/hash_cracked_password.png)

##### Answer: november16

