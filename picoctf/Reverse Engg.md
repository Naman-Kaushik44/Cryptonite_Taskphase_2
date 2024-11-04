# ARMssembly 1

**Flag**: `picoCTF{0000001b}`

How I approached the challenge :

So first of all, I downloaded the `chall_1.S` into my system and opened it to infer some things from the assembly code.
``
.arch armv8-a
.file	"chall_1.c"
 ``
 The first lines of the code tells us that this is a 64 bit arm assembly challenge and the orignal code was in C language.
 I changed directories to the downloads directory where I had the chall_1.S file downloaded and next I tried running the file within my linux terminal with the command that I referenced from the video on Reverse Enginnering by the youtuber *Low Level*.
```
  aarch64-linux-gnu-gcc -o chal ./chall_1.S -static
```
Although it compiled, I wasn't able to run ./chal after it due to some error so as an alternative I installed quemu-arch64 to emulate the ARM architechure on x86 system.

Then next I tried reading the Assembly file and with some references I could understand that in the main function the initial value from w0 was being stored in stack offset 12 and the constant value 81 was moved into it, next that again stored into the stack offset 16.
Then zero was stored in stack offset 20 and 3 is moved in w0 before being stored into stack offset 24.

```
str	w0, [sp, 12]
	mov	w0, 81
	str	w0, [sp, 16]
	str	wzr, [sp, 20]
	mov	w0, 3
	str	w0, [sp, 24]
```

```
naman_kaushik@Ubuntu:~/Downloads$ qemu-aarch64 ./chal 81 0 3
You Lose :(
naman_kaushik@Ubuntu:~/Downloads$ qemu-aarch64 ./chal 81 3 0
You Lose :(
naman_kaushik@Ubuntu:~/Downloads$ qemu-aarch64 ./chal 81 3
You Lose :(
naman_kaushik@Ubuntu:~/Downloads$ qemu-aarch64 ./chal 81 
You Lose :(
naman_kaushik@Ubuntu:~/Downloads$ qemu-aarch64 ./chal 3
You Lose :(
naman_kaushik@Ubuntu:~/Downloads$ qemu-aarch64 ./chal 0
You Lose :(
naman_kaushik@Ubuntu:~/Downloads$ qemu-aarch64 ./chal 0 3
You Lose :(
naman_kaushik@Ubuntu:~/Downloads$ 0 81
0: command not found
naman_kaushik@Ubuntu:~/Downloads$ qemu-aarch64 ./chal 0 81
You Lose :(
naman_kaushik@Ubuntu:~/Downloads$ qemu-aarch64 ./chal 0 27
You Lose :(
naman_kaushik@Ubuntu:~/Downloads$ qemu-aarch64 ./chal 27
You win!
```
After reading the assembly file and 

