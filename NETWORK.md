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