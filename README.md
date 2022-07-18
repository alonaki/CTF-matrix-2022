Cookies - Pwn

![image](https://user-images.githubusercontent.com/81467647/179555684-50ac7bf3-09a4-46fb-8f96-7af86dd2a145.png)

So we start to run the file to check what we dealing with

![image](https://user-images.githubusercontent.com/81467647/179555910-af130a3f-8c38-424b-9836-9003cbe33b54.png)

After some playing around few things i discoverd:

1) The number of coockies need to be smaller then 11 or we get:
      ![image](https://user-images.githubusercontent.com/81467647/179556250-99b8f0d1-84f1-476b-99d4-a72dfa1c7a7e.png)

2) we can get infinite loop by input "-1" (later we understand why)

3) After we enter the first input we can see that the binary crash if we enter input longer then 8
![image](https://user-images.githubusercontent.com/81467647/179557118-24ed5e54-78d6-4816-b027-87e979903093.png)

Now lets check the source code with Ghidra.

![image](https://user-images.githubusercontent.com/81467647/179559851-5086781b-d241-4bd7-85dd-b837772be8ab.png)

In the end of the picture there is a weird if statement that if its not succeed we get "__stack_chk_fail", this value is the canary value, every time the program run its get a diffrent value
this is a protect mechmisam agianst BOF.

Bypass the canary:

We going to use Brute force , its mine that once the canary value is set it remiend the same until the end of the program so we will check bit after bit and we will found the canary value.

![image](https://user-images.githubusercontent.com/81467647/179561465-4fdac3ee-a32f-42be-957f-c001e9ebbe77.png)

After we found the canary value , we change the EIP to the address 0x00000000004012bb and we get the flag!

MCL{ALL_0fF_7H3_c00Ki35_7a573_7H3_5am3}


