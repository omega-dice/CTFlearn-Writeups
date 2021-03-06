
# Bite-code

* **Category:** Reverse Engineering
* **Points:** 60
* **level:** Medium


## [Challenge](https://ctflearn.com/challenge/368)

>I dunno what bytecode is. Could you tell me what input of 'checkNum' will return true? The flag is just a 32-bit signed integer as a decimal (nothing else.) https://mega.nz/#!qfATFaKR!zaTNExq3Bm1MjJnePjTGQyvnvLX_xZxhbGaMv_ypaxo

## Solution
We get a txt file of java bytecode , For those who dont familiar wtih that, you have to read and explore before you continue.   
There are some good resources:         
1.[Java Bytecode Crash Course](https://www.youtube.com/watch?v=e2zmmkc5xI0)   
2.[A Java Programmer's Guide to Byte Code](https://www.beyondjava.net/java-programmers-guide-java-byte-code)    
3.[Introduction to Java Bytecode](https://dzone.com/articles/introduction-to-java-bytecode)   

Ok , Now that we are pretty understood what is a bytecode and how the code executed i will show you some comments that i wrote for better understanding. I also upload the txt file with the comments.

<img width="1059" alt="Capture" src="https://user-images.githubusercontent.com/57364083/69759540-0cac1b00-116b-11ea-992b-e24d773e7b97.PNG">

Now, its easy to under stand whats going on...  
**x1 = flag << 3**  
**x2 = flag  ^ 525024598**  
**x3 = x1 ^ x2**  
**If (x3==-889275714) The flag is x3.**  

I also wirte for you in C language whats going on for better understanding.  

<img width="254" alt="Capture" src="https://user-images.githubusercontent.com/57364083/69760066-39acfd80-116c-11ea-9287-52e302dcb311.PNG">

Ok , We understood perfectly the code but we need to notice somthing **very** important:    
The flag is **32 bit integer** , So its can be a **negative** and he most be between **-2,147,483,648 to 2,147,483,647**  
Now lets write bruteforce script to get the flag !

<img width="604" alt="Capture" src="https://user-images.githubusercontent.com/57364083/69760726-f3589e00-116d-11ea-88eb-6e567b90190c.PNG">

And we Get the flag.

<img width="177" alt="Capture" src="https://user-images.githubusercontent.com/57364083/69761134-28192500-116f-11ea-9af7-040a8b4de878.PNG">


I wrote also python script for those who prefer , **But** notice - In default python numbers are up to **63 bit**   
So you must do casting to **int - 32bit** because of the **shifting that change the flag from negative to positive**    
The flag in binary is  **10101111010111010001001010101000** = **-1352854872** int-32bit  
After shifting  3  is   **01111010111010001001010101000000** = **2062062912** int 32bit  
So we can import ctypes in python and convert to 32bit.

<img width="444" alt="Capture" src="https://user-images.githubusercontent.com/57364083/69761115-1041a100-116f-11ea-95da-f367db86f961.PNG">





Flag : ```-1352854872```

