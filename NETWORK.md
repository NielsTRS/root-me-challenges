# Root Me Network Challenges
## ETHERNET - trame
Just need to open the file with wireshark

## Bluetooth - Fichier inconnu
.bin file
First looking with strings : 
```sh
$ strings ch18.bin
btsnoop
GT-S7390G
GT-S7390G
```
Then I opened it with WireShark, found the mac addr and name of the phone
Then hashed the sum string with sha1
```sh
$ echo -n "0C:B3:19:B9:4F:C6GT-S7390G" | openssl sha1
```

## CISCO - mot de passe
The documentation says that cat 7 passwords are encrypted using the weak reversible algorithm
In the file, there is 4 cat 7 password : 
```
025017705B3907344E => 6sK0_hub
10181A325528130F010D24 => 6sK0_admin
124F163C42340B112F3830 => 6sK0_guest
144101205C3B29242A3B3C3927 => 6sK0_console
```
We can decrypt using this website : https://www.firewall.cx/cisco/cisco-routers/cisco-type7-password-crack.html

For the cat 5 password protected by the cisco MD5, I just tried : 6sK0_enable

## DNS - transfert de zone
First, we ping to check if the server is online : challenge01.root-me.org
Then we use dig : 
```sh
$ dig @challenge01.root-me.org -p 54011 ch11.challenge01.root-me.org
; <<>> DiG 9.20.2-1-Debian <<>> @challenge01.root-me.org -p 54011 ch11.challenge01.root-me.org
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 42242
;; flags: qr aa rd; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 0c7343c3b0c6fb97010000006739355778b151a7c8da2a81 (good)
;; QUESTION SECTION:
;ch11.challenge01.root-me.org.  IN      A

;; ANSWER SECTION:
ch11.challenge01.root-me.org. 604800 IN A       127.0.0.1

;; Query time: 16 msec
;; SERVER: 212.129.38.224#54011(challenge01.root-me.org) (UDP)
;; WHEN: Sun Nov 17 01:14:15 CET 2024
;; MSG SIZE  rcvd: 101
```
We found a second server : 212.129.38.224 for the same domain.
Let's go for the axfr : 
```sh
$ dig axfr @challenge01.root-me.org -p 54011 ch11.challenge01.root-me.org
; <<>> DiG 9.20.2-1-Debian <<>> axfr @challenge01.root-me.org -p 54011 ch11.challenge01.root-me.org
; (2 servers found)
;; global options: +cmd
ch11.challenge01.root-me.org. 604800 IN SOA     ch11.challenge01.root-me.org. root.ch11.challenge01.root-me.org. 2 604800 86400 2419200 604800
ch11.challenge01.root-me.org. 604800 IN TXT     "DNS transfer secret key : CBkFRwfNMMtRjHY"
ch11.challenge01.root-me.org. 604800 IN NS      ch11.challenge01.root-me.org.
ch11.challenge01.root-me.org. 604800 IN A       127.0.0.1
challenge01.ch11.challenge01.root-me.org. 604800 IN A 192.168.27.101
ch11.challenge01.root-me.org. 604800 IN SOA     ch11.challenge01.root-me.org. root.ch11.challenge01.root-me.org. 2 604800 86400 2419200 604800
;; Query time: 16 msec
;; SERVER: 212.129.38.224#54011(challenge01.root-me.org) (TCP)
;; WHEN: Sun Nov 17 01:18:13 CET 2024
;; XFR size: 6 records (messages 1, bytes 274)
```
We found the flag !