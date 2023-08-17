# IPC (inter-process communication)


**Why we need ipc ?**  
1. To **inform processes that some event has occurred** .  
> exp: Kernal should inform the parent process when a child terminates.
2. **To transfer data** from one process to another
> exp: A child process needs to read input data from its parent  


A process can be two type :
- Independent process: Is not affected by the execution of other processes.
- Co-operating proccess: This type of process can be affected by other executing processes.

Process can communicate with each other using these two ways :  
1. Shared Memory
2. Message Passing

# Sockets
