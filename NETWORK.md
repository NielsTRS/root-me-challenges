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