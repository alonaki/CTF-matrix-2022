Connection_failed - Pwn

![image](https://user-images.githubusercontent.com/81467647/179481600-1e3d3a2b-7846-4013-9463-89d937bb3eea.png)


We also get 2 files one client and one server:

start with the server when we try to run it we get Segmentation Fault 

![image](https://user-images.githubusercontent.com/81467647/179482134-4fb6a902-7d3a-441d-bfc2-6c1e8d8d9773.png)

So I try to decomile with Ghidra and we can see that its look for file name flag.txt, and because its not exists in this folder we get Segmentation Fault.

![image](https://user-images.githubusercontent.com/81467647/179489542-bf7ae561-1cf7-4ef7-b4ec-37e5a19697f8.png)


So I create empty file name flag.txt and run the server again. 

![image](https://user-images.githubusercontent.com/81467647/179489651-0405e8a2-cfaa-4cf8-9e48-0faf09318ba3.png)

And as you can see no more Segmentation Fault. lets check which port is open in which address.

we will run the command "sudo lsof -i -P -n" and can see that the ip address is 127.1.33.7 in port 5000. (keep it in mind).

lets open in Ghidra the client binary.

![image](https://user-images.githubusercontent.com/81467647/179490543-9b814058-8d83-4db6-a9ea-64304e026067.png)

So the client looking for some input and if the answer is "YES" its try to open connection to the server.

lets check whats happened when we send "YES"

![image](https://user-images.githubusercontent.com/81467647/179553936-715d1314-a9a3-445f-bbc8-4ded9ac89aa2.png)

Its seems like we are missing somthing, another look in Ghidra we can see that we want to overwrite the IP address with The IP we find earlier 127.1.33.7

So I write a python script and get the flag!

![image](https://user-images.githubusercontent.com/81467647/179555169-d9a61c83-f5d7-4cd5-8436-f96a2466e941.png)

MCL{1t5_51mpLE_b0f}
