# CTF-matrix-2022
category - Warm up
Pring Flag

![image](https://user-images.githubusercontent.com/81467647/179411343-f67006b4-fafc-438c-bf25-7d7b64200d12.png)

We also get a python file:
![image](https://user-images.githubusercontent.com/81467647/179412452-4e68865f-818c-4e73-ba86-94e2cd8077c5.png)

Its seems that that in flag_array the first value is anumber that need to covert for string and the second value is a index in the string.
lets check our theroy using the python library num2words.

![image](https://user-images.githubusercontent.com/81467647/179416225-53e7f6ff-5edd-4b46-a4ae-255b6a9b4736.png)

And now we will run the script:
![image](https://user-images.githubusercontent.com/81467647/179416277-4b9d37bb-6671-4eb6-978b-2bfa9754b0a2.png)

We get something that look like a flag but not exactly, so let find out what the function num2words returns, simply by using print.
 ![image](https://user-images.githubusercontent.com/81467647/179416366-ec1cfd20-a914-4133-8e69-f3307e9a40fc.png)

And one more time we will run the script and we can see that 100 is translted to "one hundred" so if we just consider this as "hundred" and take the third index letter we get:
![image](https://user-images.githubusercontent.com/81467647/179416453-408cb991-ef38-4a16-82d7-57e0d43aaf6d.png)

We get the flag!
![image](https://user-images.githubusercontent.com/81467647/179416498-2ce22be1-1411-4ddf-8d9f-b957f18862ec.png)
