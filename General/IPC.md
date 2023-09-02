# IPC (inter-process communication)
IPC Means the way which processes communicate with each other .

There two types of executing processes in the operating system:
1. Independent processes: They cannot be affect or be affected by the other processes excuting in the system.  
also they can not affect other processes on the system.  

2. Co-operating processes: They can affect or be affected by the other processes executing in the system.  


If two processes are sharing data then those two procceses will be affected by each other.
So those processes known as **co-operating processes.**
**Any process that can share data with other processes is a co-operaing processes**.

**So Independent processes are not sharing any thing**.

## Reason for providing an enviroment that allows process co-operation
### 1. Information sharing
### 2. Computation Speedup
In order to achieve computational speed up , what can do is if we are having a task , we have to divide that task into several sub-tasks and all this sub tasks will be made to run concurrently in that way we can speed up our system.  
So instead of taking one task and waiting for it to complete from begining till the end, what we can do is we can beak it down to sub tasks and hence we can achieve more speed in our system.
So these Subtasks need to share data to each other (Child with parent or parent with child)
So this is another reason that we need IPC.

### 3. Conveniece
For example a user maybe using a system and he maybe doing several diffrent tasks at same time. he may be listening to music, he may typing a document or maybe printing a document at the same time. So that all the tasks can run smoothly without clashing with each other.

### 4. Modularity
We designing a system , this system has some component that processes should share resources to each other.


---

## Fundamental models of interprocess communication
### 1. Shared Memory:
In the shared memory model, a region of memory that is shared by cooperation processes is established.
Processes can then exchange informantion by reading and writing data to the shared region.

![Shared_memory](../Photos/Screenshot%20from%202023-08-18%2015-38-53.png)

### 2. Message passing
The communication between the processes, they takes place via messages which the processes exchanged with each other. so there are messages which will help them in communicating with each other.
![Shared_memory&message_Passing](../Photos/Screenshot%20from%202023-08-18%2015-21-26.png)

When two processes are on different hosts and communicate over a network, they cannot share data via **shared memory**. In this case, **message passing** must be used.  
Message-Passing facility provides at least two operations: 
- send message
- receive message  

Size of message sent by a process can be either 
- fixed  

or 
- variable


Refrence:
https://www.youtube.com/watch?v=dJuYKfR8vec


# Buffer
Buffer also known as shared memory
There are two kinks of buffer :
- Bounded buffer
- Unbounded buffer
![Buffer](../Photos/Screenshot%20from%202023-08-18%2015-49-54.png)
Refrence:
https://www.youtube.com/watch?v=uHtzOFwgD74



# Message Parser  


Refrence:  
https://www.youtube.com/watch?v=LuuSXWkDJOo

> **Note**
> Sockets are aother type of IPC. Their main difference is that in this method, two process which are not located on the same machine can communicate with each other
