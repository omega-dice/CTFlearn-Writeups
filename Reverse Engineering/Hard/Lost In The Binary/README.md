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

So its pretty clear that this is our  **anti-debugging technique** that leads to **LABEL_2**.  
We will avoid from that by changing **jnz** to **jmp** in IDA.  

<img width="769" alt="Capture" src="https://user-images.githubusercontent.com/57364083/78189147-2ee7d980-747a-11ea-9376-41f8b97e3a49.PNG">

The next compare is if a1 > 4. a1 is our **argc**.  
So we need to provide 4 arguments - (argv[1] ,argv[2] ,argv[3] argv[4]) + argv[0] (our path) = 5 > 4 !


Flag : ```333333```
