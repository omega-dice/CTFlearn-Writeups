# Lost In The Binary

* **Category:** Reverse Engineering
* **Points:** 80
* **level:** Hard


## [Challenge](https://ctflearn.com/challenge/285)

> I lost a flag inside this binary, please help me to find it.  
> If you trigger certain anti-debugging techniques, you might get false flags….  
> flag format: FLAG-(str)  https://mega.nz/#!ifgzQQCC!E1W0cSOFRvi7bE_v419rzwQB2jAHF0IsIRAWL6H1RNE  


## Solution

We have a clue in the description about **anti-debugging techniques** that this lead to a **false flag**  

<img width="298" alt="Capture" src="https://user-images.githubusercontent.com/57364083/78188191-78372980-7478-11ea-898f-5efeca408b93.PNG">

In the main we can notice immediately the function **ptrace**  

`The ptrace() system call provides a means by which one process (the
       "tracer") may observe and control the execution of another process
       (the "tracee"), and examine and change the tracee's memory and
       registers.  It is primarily used to implement breakpoint debugging
       and system call tracing. On error, all requests return -1`

<img width="1089" alt="Capture" src="https://user-images.githubusercontent.com/57364083/78188751-702bb980-7479-11ea-8a35-0edb4724abc5.PNG">

### Conclusions

So its pretty clear that this is our  **anti-debugging technique** that leads to **LABEL_2**.  
We will avoid from that by changing **jnz** to **jmp** in IDA.  

<img width="769" alt="Capture" src="https://user-images.githubusercontent.com/57364083/78189147-2ee7d980-747a-11ea-9376-41f8b97e3a49.PNG">

The next compare is if a1 > 4. a1 is our **argc**.  
So we need to provide 4 arguments - (argv[1] ,argv[2] ,argv[3] argv[4]) + argv[0] (our path) = 5 > 4.  

### The question is which arguments ?

`The strtol() function converts the initial part of the string in nptr to a long integer value according to the given base, which must be between 2 and 36 inclusive, or be the special value 0.`

So we need to provide 4 numbers that will confirm the equations :  

`<img width="707" alt="Capture" src="https://user-images.githubusercontent.com/57364083/78189871-8f2b4b00-747b-11ea-9006-41441e7c041a.PNG">

### Z3

We will use Z3 library to calculate this.  

<img width="571" alt="Capture" src="https://user-images.githubusercontent.com/57364083/78190511-d0702a80-747c-11ea-99bc-3b31287daedd.PNG">

### Final Stage  

Lets put the valus and run the program.  

![Screenshot from 2020-04-02 04-00-56](https://user-images.githubusercontent.com/57364083/78194484-f4843980-7485-11ea-8c24-303fd31ec133.png)




Flag : ```FLAG-21a84f2c7c7fd432edf1686215db05ea```

